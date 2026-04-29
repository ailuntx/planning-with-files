# Pi Agent 设置

如何在 [Pi Coding Agent](https://pi.dev) 中使用 planning-with-files。

---

## 安装

### Pi 安装

```bash
pi install npm:pi-planning-with-files
```

### 手动安装

1. 导航到你的项目根目录。
2. 如果不存在，创建 `.pi/skills` 目录。
3. 复制 `planning-with-files` 技能。

```bash
# 克隆仓库
git clone https://github.com/OthmanAdi/planning-with-files.git
# 复制技能
mkdir -p ~/.pi/agent/skills
cp -r planning-with-files/.pi/skills/planning-with-files .pi/skills/
```

---

## 使用方法

Pi Agent 会自动发现 `.pi/skills` 中的技能。

要使用该技能，你可以显式调用它，或者让 Pi 根据任务描述自动发现。

### 显式调用

```bash
/skill:planning-with-files
```

或者直接告诉 Pi：

```
使用 planning-with-files 技能来帮我处理这个任务。
```

---

## 重要限制

> **注意：** 钩子（PreToolUse、PostToolUse、Stop）是 **Claude Code 特有** 的，目前在 Pi Agent 中不受支持。

### 在 Pi Agent 中可用的功能：

- 核心的 3 文件规划模式
- 模板（task_plan.md、findings.md、progress.md）
- 所有规划规则和指南
- 2 行动规则
- 3 次错误协议
- 读取与写入决策矩阵
- 辅助脚本（通过显式调用或技能指令）

### 工作方式不同的功能：

- **会话恢复：** 如果需要，你必须手动运行 catchup 脚本：
  ```bash
  python3 .pi/skills/planning-with-files/scripts/session-catchup.py .
  ```
  （该技能会提供相关指令）

---

## 手动工作流程

由于钩子不会自动运行，请遵循以下模式：

### 1. 首先创建规划文件

技能指令会引导 Pi 创建这些文件。
如果没有，请询问：
```
首先使用 planning-with-files 模板创建 task_plan.md、findings.md 和 progress.md。
```

### 2. 在决策前重新阅读计划

定期询问：
```
阅读 task_plan.md 以刷新我们的上下文。
```

### 3. 阶段完成后更新文件

完成一个阶段后：
```
更新 task_plan.md，将此阶段标记为完成。
更新 progress.md，记录已完成的工作。
```

---

## 文件结构

```
你的项目/
├── .pi/
│   └── skills/
│       └── planning-with-files/
│           ├── SKILL.md
│           ├── templates/
│           ├── scripts/
│           └── ...
├── task_plan.md
├── findings.md
├── progress.md
└── ...
```

---

## 故障排除

如果 Pi 似乎没有遵循规划规则：
1. 确保技能已加载（询问“你有什么可用的技能？”）。
2. 显式要求它读取 `SKILL.md` 文件：`读取 .pi/skills/planning-with-files/SKILL.md`。
3. 如果启用，使用 `/skill:planning-with-files` 命令。
