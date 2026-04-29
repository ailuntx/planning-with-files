# Codex 设置

使用 [OpenAI Codex](https://developers.openai.com/codex/) 搭配 planning-with-files。

---

## 概述

Codex 从 `.codex/skills/` 中发现技能，从 `.codex/hooks.json` 或 `~/.codex/hooks.json` 中发现钩子。

此集成包含两项内容：

- 技能本身对应的 `.codex/skills/planning-with-files/` 目录
- 用于生命周期自动化的 `.codex/hooks.json` 及 `.codex/hooks/` 目录

钩子行为复用了与 Cursor 集成相同的成熟 shell 脚本，并针对钩子协议的差异添加了一个薄薄的 Codex 适配层。

> **重要提示：** Codex 钩子需要在 `~/.codex/config.toml` 中设置 `codex_hooks = true`。

---

## 安装

### 方法 1：工作区安装（推荐）

将 `.codex/` 提交到仓库，与整个团队共享技能和钩子：

```bash
# 在项目仓库中
git clone https://github.com/OthmanAdi/planning-with-files.git /tmp/planning-with-files

# 将 Codex 集成文件复制到你的仓库
cp -r /tmp/planning-with-files/.codex .

# 提交以与团队共享
git add .codex/
git commit -m "添加 planning-with-files 技能给 Codex"
git push

# 清理
rm -rf /tmp/planning-with-files
```

### 方法 2：个人安装

仅为自己安装：

```bash
# 克隆仓库
git clone https://github.com/OthmanAdi/planning-with-files.git /tmp/planning-with-files

# 复制技能
mkdir -p ~/.codex/skills
cp -r /tmp/planning-with-files/.codex/skills/planning-with-files ~/.codex/skills/

# 复制钩子脚本
mkdir -p ~/.codex/hooks
cp -r /tmp/planning-with-files/.codex/hooks/* ~/.codex/hooks/

# 复制 hooks.json
# 如果你已有 ~/.codex/hooks.json，请手动合并 planning-with-files 的条目
cp /tmp/planning-with-files/.codex/hooks.json ~/.codex/hooks.json

# 清理
rm -rf /tmp/planning-with-files
```

> **注意：** 如果已有 `~/.codex/hooks.json`，请勿盲目覆盖。请将 `SessionStart`、`UserPromptSubmit`、`PreToolUse`、`PostToolUse` 和 `Stop` 条目合并到现有文件中。

### 在 `config.toml` 中启用 Hooks

确保 `~/.codex/config.toml` 包含：

```toml
[features]
codex_hooks = true
```

如果已有 `[features]` 节，请在其下添加 `codex_hooks = true`，而不要创建重复的节。

### 验证

```bash
codex --version
codex features list | rg '^codex_hooks\s'
ls -la ~/.codex/skills/planning-with-files/SKILL.md
ls -la ~/.codex/hooks.json ~/.codex/hooks/
```

如果 `codex_hooks` 未出现在 `codex features list` 的输出中，请在排查技能问题前升级 Codex。

---

## 工作原理

### 钩子

Codex 从以下位置读取钩子：

1. 项目根目录下的 `.codex/hooks.json`
2. 全局安装的 `~/.codex/hooks.json`

此集成包含了全部五个 Codex 生命周期钩子：

| 钩子 | 作用 |
|------|--------------|
| **SessionStart** | 运行 `session-catchup.py`，然后注入活动计划上下文 |
| **UserPromptSubmit** | 在每条用户消息上重新注入计划和最近进度 |
| **PreToolUse** | 在 Bash 执行前重新读取 `task_plan.md` 的前 30 行 |
| **PostToolUse** | 在 Bash 活动后提醒智能体更新 `progress.md` |
| **Stop** | 当阶段未完成时阻止一次，然后回退为后续提醒 |

### 三个文件

激活后，该技能会创建并维护：

| 文件 | 用途 | 位置 |
|------|---------|----------|
| `task_plan.md` | 阶段、进度、决策 | 项目根目录 |
| `findings.md` | 研究、发现 | 项目根目录 |
| `progress.md` | 会话日志、测试结果 | 项目根目录 |

---

## 团队工作流

### 工作区安装

通过工作区安装（`.codex/` 已提交到仓库）：

- 团队每个人都获得相同的技能和钩子
- Codex 设置与项目一起受版本控制
- 更新通过正常的 git 审查过程推送

### 个人安装

通过个人安装（`~/.codex/`）：

- 你可以在所有项目中使用该技能
- 即使切换仓库，设置也会保留
- 可能需要手动合并现有的全局钩子

---

## 故障排除

### 钩子未运行？

1. 检查 `~/.codex/config.toml` 中是否包含 `codex_hooks = true`
2. 验证 `.codex/hooks.json` 或 `~/.codex/hooks.json` 是否存在
3. 添加或更改钩子后重启 Codex
4. 运行 `codex features list | rg '^codex_hooks\s'`

### 已经在使用其他全局钩子？

没关系，但不要覆盖现有的 `~/.codex/hooks.json`。改为合并 planning-with-files 的条目。

### 看到重复的钩子消息？

避免同时在两个地方安装相同的 planning-with-files 钩子：

- 工作区的 `.codex/hooks.json`
- 全局的 `~/.codex/hooks.json`

如果同时启用，Codex 可能会运行两套钩子并产生重复的提醒。

### Windows 支持

OpenAI 当前的 Codex 钩子文档说明钩子在 Windows 上被禁用。技能文件仍可安装，但钩子自动化目前适用于 macOS/Linux Codex 环境。

---

## 了解更多

- [安装指南](installation.md)
- [快速入门](quickstart.md)
- [工作流图示](workflow.md)

---

## 支持

- **GitHub Issues：** https://github.com/OthmanAdi/planning-with-files/issues
- **OpenAI Codex 钩子文档：** https://developers.openai.com/codex/hooks
- **OpenAI Codex 技能文档：** https://developers.openai.com/codex/skills
