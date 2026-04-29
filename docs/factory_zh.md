# FactoryAI Droid 设置

使用 [FactoryAI Droid](https://docs.factory.ai/) 的 planning-with-files 功能。

---

## 安装

FactoryAI Droid 会自动从 `.factory/skills/` 目录发现技能。提供两种安装方法：

### 方法 1：工作区安装（推荐）

将技能添加到你的代码仓库，与整个团队共享：

```bash
# 在你的项目仓库中
git clone https://github.com/OthmanAdi/planning-with-files.git /tmp/planning-with-files

# 将 Factory 技能复制到你的仓库
cp -r /tmp/planning-with-files/.factory .

# 提交以与团队共享
git add .factory/
git commit -m "为 Factory Droid 添加 planning-with-files 技能"
git push

# 清理
rm -rf /tmp/planning-with-files
```

现在，团队中使用 Factory Droid 的每个人都可以访问此技能！

### 方法 2：个人安装

仅为自己安装：

```bash
# 克隆仓库
git clone https://github.com/OthmanAdi/planning-with-files.git /tmp/planning-with-files

# 复制到个人 Factory 技能文件夹
mkdir -p ~/.factory/skills
cp -r /tmp/planning-with-files/.factory/skills/planning-with-files ~/.factory/skills/

# 清理
rm -rf /tmp/planning-with-files
```

### 验证

重启你的 Factory Droid 会话，之后当你处理复杂任务时，技能将自动激活。

无需斜杠命令——Factory Droid 会根据你的任务描述自动调用技能！

---

## 工作原理

### 自动激活

Factory Droid 扫描你的任务，并在以下情况自动激活技能：
- 提及“复杂任务”或“多步骤项目”
- 请求规划或组织
- 开始研究任务
- 处理需要超过 5 个步骤的项目

### 触发短语

使用以下短语可提高自动激活的可能性：
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

Factory Droid 将自动调用 `planning-with-files` 并创建三个规划文件。

---

## 三个文件

激活后，技能会创建：

| 文件 | 用途 | 位置 |
|------|---------|----------|
| `task_plan.md` | 阶段、进度、决策 | 你的项目根目录 |
| `findings.md` | 研究、发现 | 你的项目根目录 |
| `progress.md` | 会话日志、测试结果 | 你的项目根目录 |

### 模板

技能在 `.factory/skills/planning-with-files/templates/` 中包含起始模板：
- `task_plan.md` — 阶段跟踪模板
- `findings.md` — 研究存储模板
- `progress.md` — 会话日志模板

---

## 使用模式

### 1. 开始复杂任务

用复杂性指标描述你的任务：

```
我正在构建一个用户身份验证系统。
这是一个多阶段项目，需要数据库
设置、API 端点、测试和文档。
```

### 2. 技能自动激活

Factory Droid 调用 `planning-with-files` 并创建规划文件。

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

## 钩子（v2.23.0）

Factory Droid 支持钩子——自动运行 shell 命令的生命周期事件。SKILL.md 包含的钩子功能如下：

| 钩子事件 | 功能 |
|------------|-------------|
| **UserPromptSubmit** | 检测活动计划并提醒读取规划文件 |
| **PreToolUse** | 在 Write/Edit/Bash/Read 操作前读取 `task_plan.md` 的前 30 行 |
| **PostToolUse** | 提醒在文件更改后更新 `progress.md` |
| **Stop** | 运行 `check-complete.sh` 以在停止前验证所有阶段是否完成 |

这些钩子自动工作——除了复制 `.factory/` 目录外，无需额外配置。

---

## 技能特性

### 三击错误协议

发生错误时，AI 会：
1. **尝试 1：** 诊断并修复
2. **尝试 2：** 尝试替代方法
3. **尝试 3：** 进行更广泛的重新思考
4. **3 次失败后：** 上报给你

### 双操作规则

每进行 2 次搜索/查看操作后，发现的内容会保存到 `findings.md`。

防止丢失视觉/多模态信息。

### 决策前阅读

在重大决策前，AI 会重新阅读规划文件以刷新目标。

防止在长时间会话中出现目标漂移。

---

## 团队工作流

### 工作区技能（推荐）

使用工作区安装（`.factory/skills/`）：
- ✅ 团队中的每个人都拥有该技能
- ✅ 跨项目保持一致的规划
- ✅ 与你的仓库一起进行版本控制
- ✅ 通过 git 同步更改

### 个人技能

使用个人安装（`~/.factory/skills/`）：
- ✅ 在所有项目中使用
- ✅ 即使切换团队也能保留
- ❌ 不与队友共享

---

## 为何有效

这种模式是 Manus AI（被 Meta 以 20 亿美元收购）成功的原因：

> “Markdown 是我在磁盘上的‘工作记忆’。由于我迭代处理信息且活动上下文有限，Markdown 文件充当笔记的草稿纸、进度的检查点以及最终交付成果的构建块。”
> — Manus AI

**关键洞察：** 上下文窗口 = RAM（易失性）。文件系统 = 磁盘（持久性）。

将重要信息写入磁盘，而非上下文。

---

## 故障排除

### 技能未激活？

1. **添加触发短语：** 在请求中使用“复杂任务”、“多步骤”、“规划”等词语
2. **明确说明：** 提及阶段数量或复杂性
3. **重启 Droid：** Factory Droid 在重启时会重新扫描技能

### 文件未创建？

检查：
- 当前目录可写
- 没有文件权限问题
- Droid 具有文件系统访问权限

### 需要模板？

模板位于：
- **工作区：** `.factory/skills/planning-with-files/templates/`
- **个人：** `~/.factory/skills/planning-with-files/templates/`

将它们复制到你的项目根目录并进行自定义。

---

## 高级：自定义

### 修改技能

编辑 `.factory/skills/planning-with-files/SKILL.md` 以自定义：
- 更改描述中的触发短语
- 调整规划模式
- 添加团队特定规则

### 添加自定义模板

将自定义模板放在：
```
.factory/skills/planning-with-files/templates/
```

Factory Droid 将自动引用它们。

---

## 支持

- **GitHub Issues：** https://github.com/OthmanAdi/planning-with-files/issues
- **Factory 文档：** https://docs.factory.ai/cli/configuration/skills
- **作者：** [@OthmanAdi](https://github.com/OthmanAdi)

---

## 另请参阅

- [快速入门指南](quickstart.md)
- [工作流程图](workflow.md)
- [Manus 原则](../.factory/skills/planning-with-files/references.md)
- [实际示例](../.factory/skills/planning-with-files/examples.md)
