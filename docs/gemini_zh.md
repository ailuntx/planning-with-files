# Gemini CLI 设置指南

本指南介绍如何安装和使用 planning-with-files 技能，配合 [Gemini CLI](https://geminicli.com/) 使用。

## 前提条件

- Gemini CLI v0.23 或更高版本
- 在设置中启用 Agent Skills（代理技能）

## 启用 Agent Skills

Agent Skills 是一项实验性功能。请先启用它：

```bash
# 打开设置
gemini /settings

# 搜索 "Skills" → 将 "Agent Skills" 切换为 true → 按 Esc 保存
```

或者编辑 `~/.gemini/settings.json` 文件：

```json
{
  "experimental": {
    "skills": true
  }
}
```

## 安装方法

### 方法 1：从 GitHub 安装（推荐）

```bash
gemini skills install https://github.com/OthmanAdi/planning-with-files --path .gemini/skills/planning-with-files
```

### 方法 2：手动安装（用户级别）

```bash
# 克隆仓库
git clone https://github.com/OthmanAdi/planning-with-files.git

# 复制到 Gemini 技能文件夹
cp -r planning-with-files/.gemini/skills/planning-with-files ~/.gemini/skills/
```

### 方法 3：手动安装（工作区级别）

用于项目特定安装：

```bash
# 在你的项目目录中
mkdir -p .gemini/skills

# 复制技能
cp -r /path/to/planning-with-files/.gemini/skills/planning-with-files .gemini/skills/
```

## 验证安装

```bash
# 列出所有技能
gemini skills list

# 应该显示：
# - planning-with-files: 实现基于文件的 Manus 风格规划...
```

或者在交互式会话中：

```
/skills list
```

## 技能发现层级

技能从三个位置加载，优先级如下：

| 层级 | 位置 | 作用范围 |
|------|----------|-------|
| 工作区 | `.gemini/skills/` | 项目特定（最高优先级） |
| 用户 | `~/.gemini/skills/` | 所有项目 |
| 扩展 | 与扩展捆绑 | 最低优先级 |

当名称冲突时，高优先级位置会覆盖低优先级位置。

## 使用方法

### 自动激活

Gemini 会根据任务描述自动检测何时使用此技能。对于复杂的多步骤任务，它会提示你：

```
Gemini 想要激活技能：planning-with-files
用途：为复杂任务实现基于文件的 Manus 风格规划
允许？[y/n]
```

### 手动激活

你也可以手动启用/禁用技能：

```
/skills enable planning-with-files
/skills disable planning-with-files
/skills reload
```

## 钩子（v2.26.0）

Gemini CLI 支持[钩子](https://geminicli.com/docs/hooks/)——自动运行 shell 脚本的生命周期事件。此技能附带一个配置了 4 个钩子的 `settings.json` 文件：

| 钩子事件 | 功能说明 |
|------------|-------------|
| **SessionStart** | 通过 `session-catchup.py` 从之前的会话恢复上下文 |
| **BeforeTool** | 在执行写/读/Shell 操作前读取 `task_plan.md` 的前 30 行 |
| **AfterTool** | 文件更改后提醒更新 `progress.md` |
| **BeforeModel** | 在每次模型调用前注入当前阶段意识（Gemini 独有功能！） |

### 安装钩子

将钩子配置复制到你的项目：

```bash
# 复制 settings.json（与现有设置合并）
cp /path/to/planning-with-files/.gemini/settings.json .gemini/settings.json

# 复制钩子脚本
cp -r /path/to/planning-with-files/.gemini/hooks .gemini/hooks
```

或者安装用户级别的钩子：

```bash
# 复制到用户设置（适用于所有项目）
cp /path/to/planning-with-files/.gemini/settings.json ~/.gemini/settings.json
cp -r /path/to/planning-with-files/.gemini/hooks ~/.gemini/hooks
```

> **注意：** 如果已有 `settings.json` 文件，请手动合并 `"hooks"` 键。

---

## 工作原理

1. **会话开始**：Gemini 加载技能名称和描述，钩子运行会话恢复
2. **任务检测**：当你描述复杂任务时，Gemini 将其与技能匹配
3. **激活提示**：你批准技能激活
4. **加载指令**：完整的 SKILL.md 内容被添加到上下文中
5. **执行**：Gemini 遵循规划工作流程，钩子强制执行纪律

## 技能结构

```
.gemini/
├── settings.json             # 钩子配置（v2.26.0）
├── hooks/                    # 钩子脚本
│   ├── session-start.sh      # 会话恢复
│   ├── before-tool.sh        # 计划上下文注入
│   ├── after-tool.sh         # 进度更新提醒
│   └── before-model.sh       # 阶段意识（Gemini 独有）
└── skills/planning-with-files/
    ├── SKILL.md              # 主要技能指令
    ├── templates/
    │   ├── task_plan.md      # 阶段跟踪模板
    │   ├── findings.md       # 研究存储模板
    │   └── progress.md       # 会话日志模板
    ├── scripts/
    │   ├── init-session.sh   # 初始化规划文件
    │   ├── check-complete.sh # 验证完成情况
    │   ├── init-session.ps1  # Windows PowerShell 版本
    │   └── check-complete.ps1
    └── references/
        ├── reference.md      # Manus 原则
        └── examples.md       # 实际示例
```

## 与 Claude Code 共享技能

如果你同时使用 Gemini CLI 和 Claude Code，可以共享技能：

```bash
# 创建符号链接（Linux/macOS）
ln -s ~/.claude/skills ~/.gemini/skills

# 或者在两者之间复制
cp -r ~/.claude/skills/planning-with-files ~/.gemini/skills/
```

## 故障排除

### 技能未显示

1. 检查技能是否启用：`gemini /settings` → 搜索 "Skills"
2. 验证安装：`gemini skills list`
3. 重新加载技能：`/skills reload`

### 技能未激活

- 确保你的任务描述与技能的用途匹配
- 尝试手动启用：`/skills enable planning-with-files`

### Windows 上的路径问题

使用 PowerShell：

```powershell
# 复制到用户技能文件夹
Copy-Item -Recurse -Path ".\.gemini\skills\planning-with-files" -Destination "$env:USERPROFILE\.gemini\skills\"
```

## 资源

- [Gemini CLI 文档](https://geminicli.com/docs/)
- [Agent Skills 指南](https://geminicli.com/docs/cli/skills/)
- [钩子指南](https://geminicli.com/docs/hooks/)
- [技能教程](https://geminicli.com/docs/cli/tutorials/skills-getting-started/)
