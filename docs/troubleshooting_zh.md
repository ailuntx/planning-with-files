# 故障排除

常见问题及其解决方案。

---

## 更新后模板在缓存中找不到

**问题：** 更新到新版本后，`/planning-with-files` 失败，提示“模板文件在缓存中未找到”或类似错误。

**原因：** Claude Code 会缓存插件文件，更新后缓存可能未能正确刷新。

**解决方案：**

### 方案 1：完全重新安装（推荐）

```bash
/plugin uninstall planning-with-files@planning-with-files
/plugin marketplace add OthmanAdi/planning-with-files
/plugin install planning-with-files@planning-with-files
```

### 方案 2：清除 Claude Code 缓存

完全重启 Claude Code（关闭并重新打开终端/IDE）。

### 方案 3：手动清除缓存

```bash
# 查找并删除缓存的插件
rm -rf ~/.claude/cache/plugins/planning-with-files
```

然后重新安装插件。

**注意：** 此问题已在 v2.1.2 版本中通过在仓库根目录添加模板得到修复。

---

## 规划文件创建在错误目录

**问题：** 使用 `/planning-with-files` 时，文件（`task_plan.md`、`findings.md`、`progress.md`）被创建在技能安装目录，而不是你的项目目录。

**原因：** 当技能作为子代理运行时，可能无法继承你终端的当前工作目录。

**解决方案：**

### 方案 1：调用时指定项目路径

```
/planning-with-files - 我在 /path/to/my-project/ 目录下工作，请将所有文件创建在那里
```

### 方案 2：调用前添加上下文

```
我正在处理位于 /path/to/my-project/ 的项目
```
然后运行 `/planning-with-files`。

### 方案 3：在项目根目录创建 CLAUDE.md 文件

```markdown
# 项目上下文

所有规划文件（task_plan.md、findings.md、progress.md）
都应创建在此目录中。
```

### 方案 4：直接使用技能而不通过子代理

```
请使用 planning-with-files 方法帮我规划这个任务。
在此处创建 task_plan.md、findings.md 和 progress.md 文件。
```

**注意：** 此问题已在 v2.0.1 版本中修复。技能指令现在明确指定规划文件应创建在你的项目目录，而不是技能安装文件夹。

---

## 文件在会话间不持久化

**问题：** 恢复工作时，规划文件似乎消失或找不到。

**解决方案：** 确保文件位于项目根目录，而不是临时位置。

使用以下命令检查：
```bash
ls -la task_plan.md findings.md progress.md
```

如果文件缺失，它们可能被创建在：
- 技能安装文件夹（`~/.claude/skills/planning-with-files/`）
- 临时目录
- 不同的工作目录

---

## 钩子未触发

**问题：** PreToolUse 钩子（在操作前读取 task_plan.md）似乎没有运行。

**解决方案：**

1. **检查 Claude Code 版本：**
   ```bash
   claude --version
   ```
   钩子功能需要 Claude Code v2.1.0 或更高版本才能获得完整支持。

2. **验证技能安装：**
   ```bash
   ls ~/.claude/skills/planning-with-files/
   ```
   或
   ```bash
   ls .claude/plugins/planning-with-files/
   ```

3. **检查 task_plan.md 是否存在：**
   PreToolUse 钩子会运行 `cat task_plan.md`。如果文件不存在，钩子会静默成功（设计如此）。

4. **检查 YAML 错误：**
   在调试模式下运行 Claude Code：
   ```bash
   claude --debug
   ```
   查找技能加载错误。

---

## SessionStart 钩子未显示消息

**问题：** 启动 Claude Code 时未出现“就绪”消息。

**解决方案：**

1. SessionStart 钩子需要 Claude Code v2.1.0+
2. 钩子每个会话只触发一次
3. 如果已经启动会话，请重启 Claude Code

---

## PostToolUse 钩子未运行

**问题：** Write/Edit 操作后未出现提醒消息。

**解决方案：**

1. PostToolUse 钩子需要 Claude Code v2.1.0+
2. 钩子仅在成功的 Write/Edit 操作后触发
3. 检查匹配器模式：它仅设置为 `"Write|Edit"`

---

## 技能未自动检测复杂任务

**问题：** Claude 未自动为复杂任务使用规划模式。

**解决方案：**

1. **手动调用：**
   ```
   /planning-with-files
   ```

2. **触发词：** 技能根据其描述自动激活。尝试使用以下短语：
   - “复杂的多步骤任务”
   - “研究项目”
   - “需要多个步骤的任务”

3. **明确说明：**
   ```
   这是一个复杂任务，将需要超过 5 次工具调用。
   请使用 planning-with-files 模式。
   ```

---

## Stop 钩子阻止完成

**问题：** Claude 无法停止，因为 Stop 钩子提示阶段未完成。

**解决方案：**

1. **检查 task_plan.md：** 所有阶段都应标记为 `**状态：** 完成`

2. **手动覆盖：** 如果需要立即停止：
   ```
   覆盖完成检查 - 我现在想停止。
   ```

3. **修复状态：** 如果阶段实际已完成，将未完成的阶段更新为 `完成`

---

## YAML 前置元数据错误

**问题：** 由于 YAML 错误导致技能无法加载。

**解决方案：**

1. **检查缩进：** YAML 要求使用空格，而非制表符
2. **检查第一行：** 必须恰好是 `---`，前面不能有空行
3. **验证 YAML：** 使用在线 YAML 验证器

常见错误：
```yaml
# 错误 - 使用制表符
hooks:
	PreToolUse:

# 正确 - 使用空格
hooks:
  PreToolUse:
```

---

## Windows 特定问题

请参阅 [docs/windows.md](windows.md) 了解 Windows 特定的故障排除。

---

## Cursor 特定问题

请参阅 [docs/cursor.md](cursor.md) 了解 Cursor IDE 故障排除。

---

## 仍然无法解决？

在 [github.com/OthmanAdi/planning-with-files/issues](https://github.com/OthmanAdi/planning-with-files/issues) 提交问题，包含以下信息：

- 你的 Claude Code 版本（`claude --version`）
- 你的操作系统
- 你运行的命令
- 实际发生的情况与你的预期
- 任何错误信息
