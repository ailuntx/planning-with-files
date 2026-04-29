# 继续设置

如何在 Continue（VS Code / JetBrains）中使用 planning-with-files。

---

## 此集成添加的功能

- 项目级技能：`.continue/skills/planning-with-files/`
- 项目级斜杠命令提示：`.continue/prompts/planning-with-files.prompt`（Markdown 格式）

Continue 支持 **项目级**（`<repo>/.continue/...`）和 **全局**（`~/.continue/...`）两种位置。

---

## 安装（项目级，推荐）

在项目根目录执行：

```bash
git clone https://github.com/OthmanAdi/planning-with-files.git
cp -r planning-with-files/.continue .continue
```

重启 Continue（或重新加载 IDE）以加载新文件。

---

## 安装（全局）

将技能和提示复制到全局 Continue 目录：

```bash
git clone https://github.com/OthmanAdi/planning-with-files.git
mkdir -p ~/.continue/skills ~/.continue/prompts
cp -r planning-with-files/.continue/skills/planning-with-files ~/.continue/skills/
cp planning-with-files/.continue/prompts/planning-with-files.prompt ~/.continue/prompts/
```

重启 Continue（或重新加载 IDE）以加载新文件。

---

## 使用方法

1. 在 Continue 聊天中运行：
   - `/planning-with-files`
2. 提示将引导您：
   - 确保仓库根目录存在 `task_plan.md`、`findings.md`、`progress.md` 文件
   - 在整个任务过程中更新这些文件

---

## 辅助脚本（可选）

在项目根目录执行：

```bash
# 创建 task_plan.md / findings.md / progress.md（如果缺失）
bash .continue/skills/planning-with-files/scripts/init-session.sh

# 验证所有阶段是否标记为完成（需要 task_plan.md 符合格式）
bash .continue/skills/planning-with-files/scripts/check-complete.sh

# 从上次 Claude Code 会话恢复未同步的上下文（如果您也使用 Claude Code）
python3 .continue/skills/planning-with-files/scripts/session-catchup.py "$(pwd)"
```

---

## 注意事项与限制

- Continue 不运行 Claude Code 钩子（PreToolUse/PostToolUse/Stop）。工作流程是手动的：在决策前重新阅读 `task_plan.md`，并在每个阶段后更新它。
- 这三个规划文件是工具无关的，可在 Claude Code、Cursor、Gemini CLI 和 Continue 中通用。
