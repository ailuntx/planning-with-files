# Mastra Code 设置指南

使用 [Mastra Code](https://code.mastra.ai/) 的 planning-with-files 功能。

---

## 概述

Mastra Code 会自动从 `.mastracode/skills/` 目录中发现技能。它还具有内置的 Claude Code 兼容性，因此也能读取 `.claude/skills/` 目录——但专用的 `.mastracode/` 集成为您提供了原生钩子支持。

## 安装

### 方法一：工作区安装（推荐）

通过将技能添加到您的仓库，与整个团队共享：

```bash
# 在您的项目仓库中
git clone https://github.com/OthmanAdi/planning-with-files.git /tmp/planning-with-files

# 将 Mastra Code 技能复制到您的仓库
cp -r /tmp/planning-with-files/.mastracode .

# 提交以与团队共享
git add .mastracode/
git commit -m "为 Mastra Code 添加 planning-with-files 技能"
git push

# 清理
rm -rf /tmp/planning-with-files
```

现在，您团队中使用 Mastra Code 的每个人都可以访问该技能。

### 方法二：个人安装

仅为自己安装：

```bash
# 克隆仓库
git clone https://github.com/OthmanAdi/planning-with-files.git /tmp/planning-with-files

# 将技能复制到您的个人 Mastra Code 技能文件夹
mkdir -p ~/.mastracode/skills
cp -r /tmp/planning-with-files/.mastracode/skills/planning-with-files ~/.mastracode/skills/

# 复制钩子（计划执行所必需）
# 如果您已有 ~/.mastracode/hooks.json，请手动合并条目
cp /tmp/planning-with-files/.mastracode/hooks.json ~/.mastracode/hooks.json

# 清理
rm -rf /tmp/planning-with-files
```

> **注意：** 如果您已有 `~/.mastracode/hooks.json` 文件，请不要覆盖它。相反，请将技能 hooks.json 中的 PreToolUse、PostToolUse 和 Stop 条目合并到您现有的文件中。

### 验证

```bash
ls -la ~/.mastracode/skills/planning-with-files/SKILL.md
```

重启您的 Mastra Code 会话。当您处理复杂任务时，技能会自动激活。

---

## 工作原理

### 钩子（通过 hooks.json）

Mastra Code 使用单独的 `hooks.json` 文件来处理生命周期钩子。这与 Claude Code 不同，后者在 SKILL.md 的前置元数据中定义钩子。Mastra Code 从以下位置读取钩子：

1. `.mastracode/hooks.json`（项目级别，优先级最高）
2. `~/.mastracode/hooks.json`（全局）

此集成包含一个预配置的 `hooks.json`，其中包含所有三个钩子：

| 钩子 | 匹配器 | 功能 |
|------|---------|--------------|
| **PreToolUse** | Write, Edit, Bash, Read, Glob, Grep | 读取 `task_plan.md` 的前 30 行，以保持目标在注意力中 |
| **PostToolUse** | Write, Edit | 在文件更改后提醒您更新计划状态 |
| **Stop** | (全部) | 运行 `check-complete.sh` 以验证所有阶段是否完成 |

### 自动激活

当您执行以下操作时，技能会激活：
- 提及“复杂任务”或“多步骤项目”
- 要求“规划”或“分解”工作
- 请求帮助组织或跟踪进度
- 启动需要超过 5 个工具调用的研究任务

### 三个文件

激活后，技能会创建：

| 文件 | 用途 | 位置 |
|------|---------|----------|
| `task_plan.md` | 阶段、进度、决策 | 您的项目根目录 |
| `findings.md` | 研究、发现 | 您的项目根目录 |
| `progress.md` | 会话日志、测试结果 | 您的项目根目录 |

---

## Claude Code 兼容性

Mastra Code 会回退读取 `.claude/skills/` 目录。如果您已为 Claude Code 安装了 planning-with-files，它将正常工作——但专用的 `.mastracode/` 安装为您提供：

- 通过 `hooks.json` 的原生钩子（PreToolUse、PostToolUse、Stop）
- 针对 Mastra Code 目录的正确脚本路径解析
- 与 Claude Code 插件根目录无路径冲突

---

## 会话恢复

当您的上下文已满并运行 `/clear` 时，技能可以恢复您之前的会话。

手动运行：

```bash
# Linux/macOS
python3 ~/.mastracode/skills/planning-with-files/scripts/session-catchup.py "$(pwd)"
```

```powershell
# Windows PowerShell
python "$env:USERPROFILE\.mastracode\skills\planning-with-files\scripts\session-catchup.py" (Get-Location)
```

---

## 团队工作流

### 工作区技能（推荐）

使用工作区安装（`.mastracode/skills/`）：
- 团队中的每个人都拥有该技能
- 跨项目的一致规划
- 与您的仓库一起进行版本控制
- 通过 git 同步更改

### 个人技能

使用个人安装（`~/.mastracode/skills/`）：
- 在所有项目中使用
- 即使切换团队也能保留
- 不与队友共享

---

## 故障排除

### 技能未激活？

1. 验证文件是否存在：`ls ~/.mastracode/skills/planning-with-files/SKILL.md`
2. 重启 Mastra Code——技能在启动时扫描
3. 使用触发短语：“plan out”、“break down”、“organize”、“track progress”

### 钩子未运行？

Mastra Code 从 `hooks.json` 读取钩子，而不是从 SKILL.md 的前置元数据中读取。请验证：

1. 检查您的项目根目录中是否存在 `.mastracode/hooks.json`（工作区安装）或 `~/.mastracode/hooks.json`（个人安装）
2. 验证文件是否包含 PreToolUse、PostToolUse 和 Stop 条目
3. 在添加或修改 hooks.json 后重启 Mastra Code

### 已经在使用 Claude Code？

无冲突。Mastra Code 首先检查 `.mastracode/skills/`，然后回退到 `.claude/skills/`。您可以同时安装两者。

---

## 支持

- **GitHub Issues:** https://github.com/OthmanAdi/planning-with-files/issues
- **Mastra Code 文档:** https://code.mastra.ai/configuration
- **作者:** [@OthmanAdi](https://github.com/OthmanAdi)

---

## 另请参阅

- [快速入门指南](quickstart.md)
- [工作流程图](workflow.md)
- [Manus 原则](../skills/planning-with-files/reference.md)
- [实际示例](../skills/planning-with-files/examples.md)
