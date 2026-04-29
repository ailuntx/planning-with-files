# Antigravity IDE 设置

在 [Google Antigravity](https://antigravity.google/) 中使用 planning-with-files。

---

## 安装

Antigravity 会自动从 `.agent/skills/` 目录发现技能。提供两种安装方法：

### 方法 1：工作区安装（推荐）

通过将技能添加到你的仓库，与整个团队共享：

```bash
# 在你的项目仓库中
git clone https://github.com/OthmanAdi/planning-with-files.git /tmp/planning-with-files

# 将 Antigravity 技能复制到你的仓库
cp -r /tmp/planning-with-files/.agent .

# 提交以与团队共享
git add .agent/
git commit -m "为 Antigravity 添加 planning-with-files 技能"
git push

# 清理
rm -rf /tmp/planning-with-files
```

现在，你团队中使用 Antigravity 的每个人都可以访问该技能！

### 方法 2：个人安装

仅为你自己安装：

```bash
# 克隆仓库
git clone https://github.com/OthmanAdi/planning-with-files.git /tmp/planning-with-files

# 复制到你的个人 Antigravity 技能文件夹
mkdir -p ~/.gemini/antigravity/skills
cp -r /tmp/planning-with-files/.agent/skills/planning-with-files ~/.gemini/antigravity/skills/

# 清理
rm -rf /tmp/planning-with-files
```

### 验证

重启你的 Antigravity 会话，然后当你处理复杂任务时，技能将自动激活。

无需斜杠命令——Antigravity 会根据你的任务描述自动调用技能！

---

## 工作原理

### 自动激活

Antigravity 扫描你的任务，并在以下情况时自动激活技能：
- 提及“复杂任务”或“多步骤项目”
- 请求规划或组织
- 开始研究任务
- 处理需要超过 5 个步骤的项目

### 触发短语

使用以下短语可增加自动激活的可能性：
- “为……创建任务计划”
- “这是一个多步骤项目……”
- “我需要为……做规划”
- “帮我组织这个复杂任务……”

### 示例请求

```
我需要构建一个具有身份验证、
数据库集成和全面测试的 REST API。
这是一个复杂的多步骤项目，
需要仔细规划。
```

Antigravity 将自动调用 `planning-with-files` 并创建三个规划文件。

---

## 三个文件

激活后，技能会创建：

| 文件 | 用途 | 位置 |
|------|---------|----------|
| `task_plan.md` | 阶段、进度、决策 | 你的项目根目录 |
| `findings.md` | 研究、发现 | 你的项目根目录 |
| `progress.md` | 会话日志、测试结果 | 你的项目根目录 |

### 模板

技能在 `.agent/skills/planning-with-files/references/` 中包含入门模板：
- `task_plan.md` — 阶段跟踪模板
- `findings.md` — 研究存储模板
- `progress.md` — 会话日志模板

---

## 使用模式

### 1. 开始复杂任务

使用复杂性指标描述你的任务：

```
我正在构建一个用户身份验证系统。
这是一个多阶段项目，需要数据库
设置、API 端点、测试和文档。
```

### 2. 技能自动激活

Antigravity 调用 `planning-with-files` 并创建规划文件。

### 3. 按阶段工作

AI 将：
- ✅ 创建包含阶段的 `task_plan.md`
- ✅ 在工作完成时更新进度
- ✅ 将研究存储在 `findings.md` 中
- ✅ 在 `progress.md` 中记录操作
- ✅ 在重大决策前重新阅读计划

### 4. 跟踪一切

所有重要信息都会写入磁盘，不会在上下文窗口中丢失。

---

## 技能特性

### 三击错误协议

当发生错误时，AI 会：
1. **尝试 1：** 诊断并修复
2. **尝试 2：** 尝试替代方法
3. **尝试 3：** 更广泛地重新思考
4. **3 次失败后：** 上报给你

### 双操作规则

每进行 2 次搜索/查看操作后，发现的内容都会保存到 `findings.md`。

防止丢失视觉/多模态信息。

### 决策前阅读

在重大决策之前，AI 会重新阅读规划文件以刷新目标。

防止在长时间会话中出现目标漂移。

---

## 团队工作流

### 工作区技能（推荐）

使用工作区安装（`.agent/skills/`）：
- ✅ 团队中的每个人都拥有该技能
- ✅ 跨项目保持一致的规划
- ✅ 与你的仓库一起进行版本控制
- ✅ 通过 git 同步更改

### 个人技能

使用个人安装（`~/.gemini/antigravity/skills/`）：
- ✅ 在所有项目中使用
- ✅ 即使更换团队也能保留
- ❌ 不与队友共享

---

## 为何有效

这种模式是 Manus AI（被 Meta 以 20 亿美元收购）成功的原因：

> “Markdown 是我磁盘上的‘工作记忆’。由于我迭代处理信息，且我的活动上下文有限，Markdown 文件充当笔记的草稿纸、进度的检查点以及最终交付成果的构建块。”
> — Manus AI

**关键见解：** 上下文窗口 = RAM（易失性）。文件系统 = 磁盘（持久性）。

将重要信息写入磁盘，而非上下文。

---

## 故障排除

### 技能未激活？

1. **添加触发短语：** 在请求中使用“复杂任务”、“多步骤”、“规划”
2. **明确说明：** 提及阶段数量或复杂性
3. **重启 Antigravity：** 代理在重启时会重新扫描技能

### 文件未创建？

检查：
- 当前目录可写
- 没有文件权限问题
- 代理具有文件系统访问权限

### 需要模板？

模板位于：
- **工作区：** `.agent/skills/planning-with-files/references/`
- **个人：** `~/.gemini/antigravity/skills/planning-with-files/references/`

将它们复制到你的项目根目录并进行自定义。

---

## 高级：自定义

### 修改技能

编辑 `.agent/skills/planning-with-files/SKILL.md` 以自定义：
- 更改描述中的触发短语
- 调整规划模式
- 添加团队特定规则

### 添加自定义模板

将自定义模板放在：
```
.agent/skills/planning-with-files/references/
```

Antigravity 将自动引用它们。

---

## 代理技能标准

此技能遵循 [Agent Skills 规范](https://agentskills.io/specification)，这是一个用于 AI 编码助手的开放标准。

相同的技能格式适用于：
- Google Antigravity
- Claude Code
- Cursor
- 以及其他兼容 Agent Skills 的 IDE

---

## 支持

- **GitHub Issues：** https://github.com/OthmanAdi/planning-with-files/issues
- **Antigravity 文档：** https://antigravity.google/docs/skills
- **Agent Skills 规范：** https://agentskills.io/specification
- **作者：** [@OthmanAdi](https://github.com/OthmanAdi)

---

## 另请参阅

- [快速入门指南](quickstart.md)
- [工作流程图](workflow.md)
- [Manus 原则](../skills/planning-with-files/reference.md)
- [真实示例](../skills/planning-with-files/examples.md)
