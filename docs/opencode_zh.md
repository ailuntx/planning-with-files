# OpenCode IDE 支持

## 概述

planning-with-files 可作为个人或项目技能与 OpenCode 配合使用。

## 安装

详细安装说明请参阅 [docs/installation.md](installation.md)。

### 快速安装（全局）

```bash
mkdir -p ~/.config/opencode/skills
cd ~/.config/opencode/skills
git clone https://github.com/OthmanAdi/planning-with-files.git
```

### 快速安装（项目级）

```bash
mkdir -p .opencode/skills
cd .opencode/skills
git clone https://github.com/OthmanAdi/planning-with-files.git
```

## 配合 Superpowers 插件使用

如果已安装 [obra/superpowers](https://github.com/obra/superpowers) OpenCode 插件：

```
使用 use_skill 工具并设置 skill_name: "planning-with-files"
```

## 无 Superpowers 插件的使用方式

在开始复杂任务时手动读取技能文件：

```bash
cat ~/.config/opencode/skills/planning-with-files/planning-with-files/SKILL.md
```

## oh-my-opencode 兼容性

oh-my-opencode 具备 Claude Code 的技能兼容性。要在 oh-my-opencode 中使用 planning-with-files：

### 步骤 1：安装技能

```bash
mkdir -p ~/.config/opencode/skills/planning-with-files
cp -r .opencode/skills/planning-with-files/* ~/.config/opencode/skills/planning-with-files/
```

### 步骤 2：配置 oh-my-opencode

将技能添加到 `~/.config/opencode/oh-my-opencode.json`（项目级配置使用 `.opencode/oh-my-opencode.json`）：

```json
{
  "skills": {
    "sources": [
      { "path": "~/.config/opencode/skills/planning-with-files", "recursive": false }
    ],
    "enable": ["planning-with-files"]
  },
  "disabled_skills": []
}
```

### 步骤 3：验证加载

询问智能体："你是否能访问 planning-with-files 技能？能否创建 task_plan.md？"

### 故障排除

如果智能体忘记规划规则：

1. **检查技能是否加载**：该技能应出现在 oh-my-opencode 识别的技能列表中
2. **显式调用**：告诉智能体"对此任务使用 planning-with-files 技能"
3. **检查冲突**：如果同时使用 superpowers 插件和 oh-my-opencode，请选择一种方法：
   - 使用 oh-my-opencode 的原生技能加载（推荐）
   - 或使用 superpowers 的 `use_skill` 工具，但不要同时使用两者

## 已知限制

### 会话恢复

由于会话存储格式不同，`session-catchup.py` 脚本目前对 OpenCode 的支持有限：

- **Claude Code**：使用 `~/.claude/projects/` 路径下的 `.jsonl` 文件
- **OpenCode**：使用 `~/.local/share/opencode/storage/session/` 路径下的 `.json` 文件

在 OpenCode 中执行 `/clear` 命令时，会话恢复功能会检测到 OpenCode 并显示提示信息。**临时解决方案**：清空上下文后，手动读取 `task_plan.md`、`progress.md` 和 `findings.md` 文件来恢复进度。

完整的 OpenCode 会话解析支持计划在未来的版本中实现。

## 验证

**全局安装：**
```bash
ls -la ~/.config/opencode/skills/planning-with-files/planning-with-files/SKILL.md
```

**项目级安装：**
```bash
ls -la .opencode/skills/planning-with-files/planning-with-files/SKILL.md
```

## 了解更多

- [安装指南](installation.md)
- [快速开始](quickstart.md)
- [工作流程示意图](workflow.md)
