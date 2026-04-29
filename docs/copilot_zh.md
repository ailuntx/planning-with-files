# GitHub Copilot 设置

为 GitHub Copilot（CLI、VS Code 和 Coding Agent）设置 planning-with-files。

---

## 先决条件

- 已启用钩子支持的 GitHub Copilot
- 对于 VS Code：Copilot Chat 扩展 v1.109.3+
- 对于 CLI：启用代理模式的 GitHub Copilot CLI
- 对于 Coding Agent：自动与 `.github/hooks/` 配合工作

---

## 安装方法

### 方法 1：仓库级别（推荐）
将 `.github/hooks/` 目录和 `skills/planning-with-files/` 目录复制到你的项目中：

```bash
# 复制钩子（必需 — Copilot 钩子配置和脚本）
cp -r .github/hooks/ 你的项目/.github/hooks/

# 复制技能（必需 — 模板、session-catchup 脚本和 SKILL.md）
cp -r skills/planning-with-files/ 你的项目/.github/skills/planning-with-files/

# 使脚本可执行（macOS/Linux）
chmod +x 你的项目/.github/hooks/scripts/*.sh
```

钩子将对所有团队成员自动激活。这适用于 Copilot CLI、VS Code 和 Coding Agent。

### 方法 2：手动设置
1. 创建 `.github/hooks/planning-with-files.json`
2. 将钩子脚本复制到 `.github/hooks/scripts/`
3. 将 `skills/planning-with-files/` 复制到 `.github/skills/planning-with-files/`（模板、session-catchup 脚本）
4. 确保所有脚本可执行（`chmod +x .github/hooks/scripts/*.sh`）

---

## 钩子的作用

| 钩子 | 目的 | 行为 |
|------|---------|----------|
| `sessionStart` | 初始化 | 通过 session-catchup 恢复之前的上下文 |
| `preToolUse` | 上下文注入 | 在工具操作前读取 `task_plan.md` |
| `postToolUse` | 更新提醒 | 文件编辑后提示更新计划 |
| `agentStop` | 完成检查 | 在停止前验证所有阶段是否完成 |

---

## 文件结构

```
.github/
└── hooks/
    ├── planning-with-files.json    # 钩子配置
    └── scripts/
        ├── session-start.sh        # 会话初始化
        ├── session-start.ps1
        ├── pre-tool-use.sh         # 计划上下文注入
        ├── pre-tool-use.ps1
        ├── post-tool-use.sh        # 更新提醒
        ├── post-tool-use.ps1
        ├── agent-stop.sh           # 完成验证
        └── agent-stop.ps1
```

---

## 工作原理

1. **会话开始**：运行 `session-catchup` 脚本。如果你清除了会话，这会恢复之前的上下文。
2. **工具使用前**：`pre-tool-use` 钩子将 `task_plan.md` 注入上下文。这使代理能持续看到目标。
3. **文件编辑后**：任何写入或编辑操作后会出现提醒。这有助于确保计划保持更新。
4. **代理尝试停止**：`agent-stop` 钩子检查 `task_plan.md` 中的阶段状态。如果任务未完成，它会阻止停止。

---

## 与 Claude Code 插件的区别

- **钩子配置**：Claude Code 使用 `SKILL.md` 前置元数据钩子。Copilot 使用 `.github/hooks/` JSON 配置文件。
- **停止钩子**：Claude 的 `Stop` 钩子对应 Copilot 的 `agentStop`。
- **计划文件**：两者使用相同的核心文件（task_plan.md、findings.md、progress.md）。
- **协议**：钩子脚本适配了 Copilot 的 stdin JSON 和 stdout JSON 协议。

---

## 故障排除

- **钩子未运行**：检查文件权限。确保 `.github/hooks/` 目录已提交到你的仓库。
- **脚本失败**：验证 `bash` 和 `python3` 是否在你的系统 PATH 中可用。
- **Windows**：在 Windows 系统上会自动使用 PowerShell 脚本（.ps1）。
- **VS Code**：你可能需要在 Copilot Chat 扩展设置中启用钩子。

---

## 兼容性

此设置适用于整个 GitHub Copilot 生态系统：

- GitHub Copilot CLI（终端）
- VS Code Copilot Chat（代理模式）
- GitHub Copilot Coding Agent（github.com）
