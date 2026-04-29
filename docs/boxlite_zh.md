# BoxLite 设置

通过 [ClaudeBox](https://github.com/boxlite-ai/claudebox) 在 [BoxLite](https://boxlite.ai) 微虚拟机沙箱中使用 planning-with-files。

---

## 概述

BoxLite 是一个微虚拟机沙箱运行时——硬件隔离、有状态、可作为库嵌入，无需守护进程，也无需 root 权限。其团队使用的类比是“沙箱化的 SQLite”：你可以直接将其导入到你的应用程序中，每个盒子都拥有自己的内核，而不仅仅是命名空间或容器。

[ClaudeBox](https://github.com/boxlite-ai/claudebox) 是 BoxLite 官方的 Claude Code 集成层。它在 BoxLite 微虚拟机中运行 Claude Code CLI，提供持久化工作空间、可选技能、安全策略和资源限制。planning-with-files 作为 Skill 对象加载到 ClaudeBox 中——技能的 SKILL.md 和脚本在启动时被注入到虚拟机文件系统中，正好位于 Claude Code 期望找到它们的位置。

**为什么使用这种组合：**
- 在硬件隔离的环境中运行不受信任的 AI 生成代码，不接触主机文件系统
- 规划文件 (`task_plan.md`, `findings.md`, `progress.md`) 通过 ClaudeBox 在 `~/.claudebox/sessions/` 下的有状态工作空间在会话间持久化
- 在风险阶段前创建快照，需要时可回滚
- 所有钩子（PreToolUse、PostToolUse、Stop）都能正常工作，因为 Claude Code 在虚拟机内原生运行

---

## 先决条件

- Python 3.10+
- BoxLite 运行时（作为 ClaudeBox 依赖项自动安装）
- Docker —— 用于拉取和管理 OCI 镜像（沙箱本身作为微虚拟机运行，而非 Docker 容器）
- 设置 `CLAUDE_CODE_OAUTH_TOKEN` 或 `ANTHROPIC_API_KEY`

---

## 安装

### 安装 ClaudeBox

```bash
pip install claudebox
```

### 设置 API 凭证

```bash
export CLAUDE_CODE_OAUTH_TOKEN=sk-ant-oat01-...
# 或
export ANTHROPIC_API_KEY=sk-ant-...
```

---

## 快速开始

在 BoxLite 虚拟机内使用 planning-with-files 的最小示例：

```python
import asyncio
from pathlib import Path
from claudebox import ClaudeBox, Skill

# 从已安装的技能中加载 SKILL.md 内容
SKILL_MD = Path.home() / ".claude" / "skills" / "planning-with-files" / "SKILL.md"

PLANNING_SKILL = Skill(
    name="planning-with-files",
    description="Manus 风格的文件规划，使用持久化 Markdown",
    files={
        "/root/.claude/skills/planning-with-files/SKILL.md": SKILL_MD.read_text()
    }
)

async def main():
    async with ClaudeBox(
        session_id="my-project",
        skills=[PLANNING_SKILL]
    ) as box:
        result = await box.code(
            "为 Express API 规划和实现用户认证功能"
        )
        print(result.response)

asyncio.run(main())
```

Claude Code 在虚拟机内的 `/root/.claude/skills/planning-with-files/SKILL.md` 找到该技能。三个规划文件 (`task_plan.md`, `findings.md`, `progress.md`) 被写入盒子的工作目录，并在会话间持久化。

---

## 持久化会话

ClaudeBox 工作空间在重启后仍然存在。可以精确地从上次中断的地方继续：

```python
async def main():
    # 第一次会话 —— 开始规划
    async with ClaudeBox(session_id="auth-feature", skills=[PLANNING_SKILL]) as box:
        await box.code("为认证功能创建 task_plan.md")

    # 后续会话 —— 相同的工作空间，规划文件仍在
    async with ClaudeBox.reconnect("auth-feature") as box:
        await box.code("根据计划继续实现登录端点")

    # 完成后清理
    await ClaudeBox.cleanup_session("auth-feature", remove_workspace=True)

asyncio.run(main())
```

---

## 钩子在虚拟机内如何工作

ClaudeBox 在 BoxLite 微虚拟机内原生运行 Claude Code CLI。这意味着 planning-with-files 的钩子执行方式与在本地机器上完全相同：

| 钩子 | 触发时机 | 作用 |
|------|---------|--------------|
| **PreToolUse** | 在 Write、Edit、Bash、Read、Glob、Grep 之前 | 读取 `task_plan.md` 的前 30 行 —— 保持目标在注意力中 |
| **PostToolUse** | 在 Write、Edit 之后 | 提醒代理在文件更改后更新计划状态 |
| **Stop** | 当代理完成时 | 在停止前运行完成检查脚本 |

钩子脚本需要与 SKILL.md 一起注入。有关如何包含它们的完整示例，请参见 `examples/boxlite/quickstart.py`。

---

## 会话恢复

当你的上下文填满并运行 `/clear` 时，技能会在下次激活时自动恢复之前的会话。在 ClaudeBox 会话内，可以手动运行：

```python
async with ClaudeBox.reconnect("my-project") as box:
    await box.code(
        "运行会话追赶：python3 ~/.claude/skills/planning-with-files/scripts/session-catchup.py $(pwd)"
    )
```

---

## 快照

BoxLite 支持检查点。在风险实现阶段前创建快照：

```python
from claudebox import ClaudeBox
from boxlite import Box

async def main():
    async with ClaudeBox(session_id="risky-refactor", skills=[PLANNING_SKILL]) as box:
        # 第一阶段完成 —— 在破坏性重构前创建快照
        await box.box.snapshot("pre-refactor")

        result = await box.code("重构数据库层")

        if "error" in result.response.lower():
            await box.box.restore("pre-refactor")
            print("已回滚到重构前快照")

asyncio.run(main())
```

---

## 故障排除

### 技能在虚拟机内未激活？

技能文件必须在虚拟机内的 `/root/.claude/skills/planning-with-files/SKILL.md`。通过注入验证步骤来确认：

```python
result = await box.code("ls ~/.claude/skills/planning-with-files/")
print(result.response)
```

### 钩子未运行？

Stop 钩子脚本 (`check-complete.sh`) 也必须被注入。将其添加到你的 Skill 对象的 `files` 字典中。完整实现请参见 `examples/boxlite/quickstart.py`。

### 找不到 Docker？

ClaudeBox 使用 Docker 拉取虚拟机的 OCI 镜像。请安装 Docker Desktop（macOS/Windows）或 Docker Engine（Linux）。沙箱本身并不作为 Docker 容器运行——Docker 仅用于镜像管理。

### Windows 主机上 BoxLite 不可用？

BoxLite 需要 Linux KVM（Linux）或 Hypervisor.framework（macOS）。在 Windows 上，请使用启用了 KVM 的 WSL2。

---

## 另请参阅

- [BoxLite 文档](https://docs.boxlite.ai)
- [ClaudeBox 仓库](https://github.com/boxlite-ai/claudebox)
- [boxlite-mcp](https://github.com/boxlite-labs/boxlite-mcp) —— BoxLite 沙箱化的 MCP 服务器
- [快速开始指南](quickstart.md)
- [工作流程图](workflow.md)
- [作者: @OthmanAdi](https://github.com/OthmanAdi)
