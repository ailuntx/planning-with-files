# AdaL CLI / Sylph AI 设置

如何在 AdaL CLI（Sylph AI）中使用 planning-with-files。

---

## 关于 AdaL

[AdaL](https://docs.sylph.ai/) 是 SylphAI 推出的 CLI 工具，为你的终端带来 AI 驱动的编码辅助。AdaL 技能**与 Claude Code 技能兼容**，因此你可以使用相同的技能格式。

---

## 安装

### 选项 1：通过插件市场安装（推荐）

```bash
# 添加市场（一次性设置）
/plugin marketplace add OthmanAdi/planning-with-files

# 通过对话框浏览并安装
/plugin

# 或直接安装
/plugin install planning-with-files@planning-with-files
```

### 选项 2：复制到个人技能目录

```bash
git clone https://github.com/OthmanAdi/planning-with-files.git
cp -r planning-with-files/.adal/skills/planning-with-files ~/.adal/skills/
```

### 选项 3：复制到项目技能目录

```bash
git clone https://github.com/OthmanAdi/planning-with-files.git
cp -r planning-with-files/.adal/skills/planning-with-files .adal/skills/
```

### 选项 4：Windows（PowerShell）

```powershell
git clone https://github.com/OthmanAdi/planning-with-files.git
Copy-Item -Recurse -Path "planning-with-files\.adal\skills\planning-with-files" -Destination "$env:USERPROFILE\.adal\skills\"
```

---

## 技能位置

AdaL 支持三种技能来源：

| 来源 | 位置 | 使用场景 |
|--------|----------|----------|
| 个人 | `~/.adal/skills/` | 你的自定义技能（最高优先级） |
| 项目 | `.adal/skills/` | 通过 git 共享的团队技能 |
| 插件 | `~/.adal/plugin-cache/` | 来自 GitHub 的外部技能（最低优先级） |

---

## 使用 /skills 命令

在 AdaL 中，查看所有活动技能：

```
/skills
```

这会显示所有来源（个人、项目、插件）的技能：

```
> 技能 (第 1 页/共 1 页)

个人 (~/.adal/skills/):
  planning-with-files

项目 (.adal/skills/):
  (无)

插件:
  (无)
```

---

## 使用 /plugin 命令

管理来自 GitHub 的外部技能：

| 命令 | 描述 |
|---------|-------------|
| `/plugin marketplace add <owner/repo>` | 添加一个市场（一次性设置） |
| `/plugin` | 浏览可用插件和市场 |
| `/plugin install <plugin>@<marketplace>` | 安装插件 |
| `/plugin uninstall <plugin>@<marketplace>` | 卸载插件 |
| `/plugin marketplace remove <name>` | 移除市场 |

---

## 文件结构

```
你的项目/
├── .adal/
│   └── skills/
│       └── planning-with-files/
│           ├── SKILL.md
│           ├── templates/
│           │   ├── task_plan.md
│           │   ├── findings.md
│           │   └── progress.md
│           ├── scripts/
│           │   ├── init-session.sh
│           │   ├── init-session.ps1
│           │   ├── check-complete.sh
│           │   ├── check-complete.ps1
│           │   └── session-catchup.py
│           └── references/
│               ├── examples.md
│               └── reference.md
├── task_plan.md        ← 你的规划文件放在这里
├── findings.md
├── progress.md
└── ...
```

---

## 模板

`.adal/skills/planning-with-files/templates/` 中的模板在 AdaL 中可用：

- `task_plan.md` - 阶段跟踪模板
- `findings.md` - 研究存储模板
- `progress.md` - 会话日志模板

开始新任务时，将它们复制到你的项目根目录。

---

## 脚本

用于手动执行的辅助脚本（AdaL 通过 bash 执行脚本）：

```bash
# 初始化规划文件
bash ~/.adal/skills/planning-with-files/scripts/init-session.sh

# 检查任务完成情况
bash ~/.adal/skills/planning-with-files/scripts/check-complete.sh

# 会话恢复
python ~/.adal/skills/planning-with-files/scripts/session-catchup.py $(pwd)
```

Windows PowerShell：
```powershell
# 初始化规划文件
powershell -File "$env:USERPROFILE\.adal\skills\planning-with-files\scripts\init-session.ps1"

# 检查任务完成情况
powershell -File "$env:USERPROFILE\.adal\skills\planning-with-files\scripts\check-complete.ps1"
```

---

## 给 AdaL 用户的提示

1.  **使用 /skills 命令：** 安装后验证 planning-with-files 是否处于活动状态。

2.  **使用明确的提示：** 开始复杂任务时请明确说明：
    ```
    这是一个复杂任务。让我们使用 planning-with-files 模式。
    首先创建包含目标和阶段的 task_plan.md。
    ```

3.  **固定规划文件：** 保持 task_plan.md 打开以便轻松参考。

4.  **决策前重新阅读计划：** 定期让 AdaL 读取 task_plan.md 以刷新目标。

5.  **定期检查状态：** 在完成前验证所有阶段是否已完成。

---

## 需要帮助？

- AdaL 文档：[docs.sylph.ai](https://docs.sylph.ai/)
- 在 [github.com/OthmanAdi/planning-with-files/issues](https://github.com/OthmanAdi/planning-with-files/issues) 提交问题
