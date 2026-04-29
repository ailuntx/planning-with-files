```markdown
---
name: planning-with-files
description: 实现Manus风格的文件驱动规划方法，用于组织和跟踪复杂任务进度。创建task_plan.md、findings.md和progress.md文件。适用于需要规划、拆解或组织多步骤项目、研究任务或任何需要超过5次工具调用的工作场景。支持在/clear后自动恢复会话。
user-invocable: true
allowed-tools: "Read, Write, Edit, Bash, Glob, Grep"
hooks:
  UserPromptSubmit:
    - hooks:
        - type: command
          command: "if [ -f task_plan.md ]; then echo '[planning-with-files] ACTIVE PLAN — current state:'; head -50 task_plan.md; echo ''; echo '=== recent progress ==='; tail -20 progress.md 2>/dev/null; echo ''; echo '[planning-with-files] Read findings.md for research context. Continue from the current phase.'; fi"
  PreToolUse:
    - matcher: "Write|Edit|Bash|Read|Glob|Grep"
      hooks:
        - type: command
          command: "cat task_plan.md 2>/dev/null | head -30 || true"
  PostToolUse:
    - matcher: "Write|Edit"
      hooks:
        - type: command
          command: "if [ -f task_plan.md ]; then echo '[planning-with-files] Update progress.md with what you just did. If a phase is now complete, update task_plan.md status.'; fi"
  Stop:
    - hooks:
        - type: command
          command: "export SD=\"${CODEX_SKILL_ROOT:-$HOME/.codex/skills/planning-with-files}/scripts\"; powershell.exe -NoProfile -ExecutionPolicy Bypass -File \"$SD/check-complete.ps1\" 2>/dev/null || sh \"$SD/check-complete.sh\""
metadata:
  version: "2.33.0"
---

# 文件驱动规划

像Manus一样工作：使用持久化Markdown文件作为"磁盘上的工作记忆"。

## 第一步：检查历史会话 (v2.2.0)

**开始工作前**，检查历史会话中未同步的上下文：

```bash
# Linux/macOS (自动检测python3或python)
$(command -v python3 || command -v python) ~/.codex/skills/planning-with-files/scripts/session-catchup.py "$(pwd)"
```

```powershell
# Windows PowerShell
python "$env:USERPROFILE\.codex\skills\planning-with-files\scripts\session-catchup.py" (Get-Location)
```

若同步报告显示未同步上下文：
1. 运行 `git diff --stat` 查看实际代码变更
2. 阅读当前规划文件
3. 根据同步报告和git diff更新规划文件
4. 继续执行任务

## 重要：文件存放位置

- **模板文件**位于 `~/.codex/skills/planning-with-files/templates/`
- **您的规划文件**应放在**项目目录中**

| 位置 | 存放内容 |
|----------|-----------------|
| 技能目录 (`~/.codex/skills/planning-with-files/`) | 模板、脚本、参考文档 |
| 项目目录 | `task_plan.md`, `findings.md`, `progress.md` |

## 快速开始

执行任何复杂任务前：

1. **创建 `task_plan.md`** — 参考[templates/task_plan.md](templates/task_plan.md)
2. **创建 `findings.md`** — 参考[templates/findings.md](templates/findings.md)
3. **创建 `progress.md`** — 参考[templates/progress.md](templates/progress.md)
4. **决策前重读计划** — 刷新注意力窗口中的目标
5. **每阶段完成后更新** — 标记完成状态，记录错误

> **注意：** 规划文件应放在项目根目录，而非技能安装目录。

## 核心模式

```
上下文窗口 = 内存（易失，有限）
文件系统 = 磁盘（持久，无限）

→ 所有重要内容写入磁盘
```

## 文件用途

| 文件 | 用途 | 更新时间 |
|------|---------|----------------|
| `task_plan.md` | 阶段划分、进度跟踪、决策记录 | 每阶段完成后 |
| `findings.md` | 研究结果、关键发现 | 任何新发现后 |
| `progress.md` | 会话日志、测试结果 | 会话全程 |

## 核心规则

### 1. 先创建计划
没有`task_plan.md`绝不开始复杂任务。不可妥协。

### 2. 双操作原则
> "每进行2次查看/浏览/搜索操作后，立即将关键发现保存到文本文件"

防止视觉/多模态信息丢失。

### 3. 决策前阅读
重大决策前阅读计划文件。保持目标在注意力窗口内。

### 4. 行动后更新
完成任何阶段后：
- 更新阶段状态：`in_progress` → `complete`
- 记录遇到的错误
- 注明创建/修改的文件

### 5. 记录所有错误
所有错误记入计划文件。积累经验避免重复。

```markdown
## 遇到的错误
| 错误 | 尝试次数 | 解决方案 |
|-------|---------|------------|
| FileNotFoundError | 1 | 创建默认配置 |
| API超时 | 2 | 添加重试逻辑 |
```

### 6. 永不重复失败
```
if 操作失败:
    下个动作 ≠ 相同动作
```
记录尝试过程。改变方法。

## 三振错误协议

```
尝试1：诊断修复
  → 仔细阅读错误
  → 定位根本原因
  → 针对性修复

尝试2：替代方案
  → 相同错误？换方法
  → 换工具？换库？
  → 绝不重复失败动作

尝试3：全局反思
  → 质疑前提假设
  → 搜索解决方案
  → 考虑更新计划

3次失败后：上报用户
  → 说明尝试过程
  → 分享具体错误
  → 请求指导
```

## 读写决策矩阵

| 场景 | 操作 | 原因 |
|-----------|--------|--------|
| 刚写入文件 | 不读取 | 内容仍在上下文中 |
| 查看图片/PDF | 立即记录发现 | 多模态→文本易丢失 |
| 浏览器返回数据 | 写入文件 | 截图无法持久保存 |
| 开始新阶段 | 阅读计划/发现 | 上下文陈旧需重新定位 |
| 发生错误 | 阅读相关文件 | 需当前状态修复 |
| 中断后恢复 | 阅读所有规划文件 | 恢复状态 |

## 五问重启测试

能回答这些问题说明上下文管理合格：

| 问题 | 答案来源 |
|----------|---------------|
| 当前进度？ | task_plan.md中的阶段 |
| 后续步骤？ | 剩余阶段 |
| 最终目标？ | 计划中的目标声明 |
| 学到什么？ | findings.md |
| 完成什么？ | progress.md |

## 适用场景

**适用于：**
- 多步骤任务（3步以上）
- 研究任务
- 构建/创建项目
- 跨多次工具调用的任务
- 需组织管理的任务

**不适用于：**
- 简单问题
- 单文件编辑
- 快速查询

## 模板

复制这些模板开始：

- [templates/task_plan.md](templates/task_plan.md) — 阶段跟踪
- [templates/findings.md](templates/findings.md) — 研究存储
- [templates/progress.md](templates/progress.md) — 会话日志

## 脚本

自动化辅助脚本：

- `scripts/init-session.sh` — 初始化所有规划文件
- `scripts/check-complete.sh` — 验证阶段完成状态
- `scripts/session-catchup.py` — 从历史会话恢复上下文 (v2.2.0)

## 高级主题

- **Manus原则：** 参见[references/reference.md](references/reference.md)
- **实际案例：** 参见[references/examples.md](references/examples.md)

## 反面模式

| 禁止行为 | 替代方案 |
|-------|------------|
| 用TodoWrite持久化 | 创建task_plan.md文件 |
| 目标陈述后遗忘 | 决策前重读计划 |
| 隐藏错误静默重试 | 错误记入计划文件 |
| 所有内容塞入上下文 | 大内容存入文件 |
| 立即开始执行 | 先创建计划文件 |
| 重复失败操作 | 记录尝试并改变方法 |
| 在技能目录创建文件 | 在项目目录创建文件 |
```
