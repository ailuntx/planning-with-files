# Cursor IDE 设置指南

如何在 Cursor IDE 中使用 planning-with-files —— 现已支持完整钩子功能。

---

## 安装方法

### 选项一：复制 .cursor 目录（推荐）

```bash
git clone https://github.com/OthmanAdi/planning-with-files.git
cp -r planning-with-files/.cursor .cursor
```

此方法将技能文件、钩子配置和钩子脚本复制到您的项目中。

### 选项二：手动设置

1. 复制 `.cursor/skills/planning-with-files/` 到您的项目
2. 复制 `.cursor/hooks.json` 到您的项目
3. 复制 `.cursor/hooks/` 目录到您的项目

---

## 钩子功能支持

Cursor 现已通过 `.cursor/hooks.json` 原生支持钩子功能。本技能包含三个钩子，复现了 Claude Code 的使用体验：

| 钩子 | 用途 | Cursor 功能 |
|------|------|-------------|
| `preToolUse` | 在工具操作前重新读取 task_plan.md | 保持目标在上下文中 |
| `postToolUse` | 文件编辑后提醒更新计划 | 防止忘记更新 |
| `stop` | 检查所有阶段是否完成 | 如果未完成则**自动继续** |

### 停止钩子的工作原理

停止钩子是最强大的功能。当代理尝试停止时：

1. 检查 `task_plan.md` 中的阶段完成状态
2. 如果所有阶段都已完成 → 允许代理停止
3. 如果阶段未完成 → 发送 `followup_message` 自动提示代理继续工作

这意味着代理**在所有阶段完成前无法停止**（最多重试 `loop_limit` 3次）。

### 钩子文件结构

```
your-project/
├── .cursor/
│   ├── hooks.json                  ← 钩子配置文件
│   ├── hooks/
│   │   ├── pre-tool-use.sh         ← 预工具使用脚本
│   │   ├── post-tool-use.sh        ← 后工具使用脚本
│   │   ├── stop.sh                 ← 完成检查脚本
│   │   ├── pre-tool-use.ps1        ← PowerShell 版本
│   │   ├── post-tool-use.ps1
│   │   └── stop.ps1
│   └── skills/
│       └── planning-with-files/
│           ├── SKILL.md
│           ├── examples.md
│           ├── reference.md
│           └── templates/
├── task_plan.md                     ← 您的计划文件（按任务创建）
├── findings.md
├── progress.md
└── ...
```

---

## Windows 系统设置

默认的 `hooks.json` 使用 bash 脚本（适用于 macOS、Linux 和带 Git Bash 的 Windows）。

**如果需要原生 PowerShell 支持**，请重命名配置文件：

```powershell
# 备份默认配置
Rename-Item .cursor\hooks.json hooks.unix.json

# 使用 PowerShell 配置
Rename-Item .cursor\hooks.windows.json hooks.json
```

`.cursor/hooks.windows.json` 文件使用 PowerShell 直接执行 `.ps1` 钩子脚本。

---

## 各钩子功能详解

### 预工具使用钩子

**触发时机：** 在写入、编辑、Shell 或读取操作之前

**功能：** 读取 `task_plan.md` 的前 30 行并记录到 stderr 以供上下文参考。始终返回 `{"decision": "allow"}` —— 从不阻止工具使用。

**Claude Code 等效命令：** `cat task_plan.md 2>/dev/null | head -30 || true`

### 后工具使用钩子

**触发时机：** 在写入或编辑操作之后

**功能：** 如果某个阶段已完成，则输出提醒以更新 `task_plan.md`。

**Claude Code 等效命令：** `echo '[planning-with-files] File updated...'`

### 停止钩子

**触发时机：** 当代理尝试停止工作时

**功能：**
1. 统计 `task_plan.md` 中的总阶段数（`### Phase` 标题）
2. 统计已完成阶段数（支持 `**Status:** complete` 和 `[complete]` 两种格式）
3. 如果未完成，返回 `followup_message` 以自动继续
4. 通过 `loop_limit` 限制最多重试 3 次，防止无限循环

**Claude Code 等效命令：** `scripts/check-complete.sh` —— 但 Cursor 版本**更强大**，因为它可以自动继续代理工作，而不仅仅是报告状态。

---

## 技能文件

`.cursor/skills/planning-with-files/SKILL.md` 文件包含所有规划指南：

- 核心 3 文件规划模式
- 模板（task_plan.md、findings.md、progress.md）
- 2 操作规则
- 3 次错误协议
- 读取与写入决策矩阵

Cursor 在打开项目时会自动从 `.cursor/skills/` 加载技能。

---

## 模板文件

`.cursor/skills/planning-with-files/templates/` 中的模板在开始新任务时使用：

- `task_plan.md` - 阶段跟踪模板
- `findings.md` - 研究存储模板
- `progress.md` - 会话日志模板

代理在开始新的规划会话时会将这些模板复制到您的项目根目录。

---

## Cursor 用户提示

1. **固定计划文件：** 在分屏视图中保持 task_plan.md 打开，便于随时参考。

2. **信任钩子功能：** 停止钩子会防止过早完成 —— 您无需手动验证阶段状态。

3. **复杂任务使用明确提示：**
   ```
   这是一个复杂任务。让我们使用 planning-with-files 模式。
   首先创建包含目标和阶段的 task_plan.md。
   ```

4. **检查钩子日志：** 如果钩子不工作，请检查 Cursor 的输出面板中的钩子执行日志。

---

## 与 Claude Code 的兼容性

您的计划文件（task_plan.md、findings.md、progress.md）在 Cursor 和 Claude Code 之间完全兼容。您可以在两者之间切换，无需更改计划文件。

---

## 需要帮助？

请在 [github.com/OthmanAdi/planning-with-files/issues](https://github.com/OthmanAdi/planning-with-files/issues) 提交问题。
