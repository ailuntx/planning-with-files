# 迁移指南：从 v1.x 到 v2.0.0

## 概述

版本 2.0.0 增加了钩子集成和增强的模板，同时保持与现有工作流的向后兼容性。

## 新增功能

### 1. 钩子（自动行为）

v2.0.0 添加了 Claude Code 钩子，可自动执行关键的 Manus 原则：

| 钩子 | 触发时机 | 行为 |
|------|---------|----------|
| `PreToolUse` | 在 Write/Edit/Bash 之前 | 读取 `task_plan.md` 以刷新目标 |
| `Stop` | 在停止之前 | 验证所有阶段是否完成 |

**好处：** 您不再需要手动记住重新阅读计划。钩子会自动完成。

### 2. 模板目录

新模板提供了结构化的起点：

```
templates/
├── task_plan.md    # 包含状态字段的阶段跟踪
├── findings.md     # 研究存储，附带 2 行动提醒
└── progress.md     # 会话日志，附带 5 问题重启测试
```

### 3. 脚本目录

用于常见操作的辅助脚本：

```
scripts/
├── init-session.sh     # 创建所有 3 个规划文件
└── check-complete.sh   # 验证任务完成情况
```

## 迁移步骤

### 步骤 1：更新插件

```bash
# 如果通过市场安装
/plugin update planning-with-files

# 如果手动安装
cd .claude/plugins/planning-with-files
git pull origin master
```

### 步骤 2：现有文件继续工作

您现有的 `task_plan.md` 文件将继续工作。钩子会查找此文件，并在其缺失时优雅处理。

### 步骤 3：采用新模板（可选）

要使用新的结构化模板，您可以：

1. **重新开始**：使用 `./scripts/init-session.sh`
2. **复制模板**：从 `templates/` 目录复制
3. **保留现有格式** - 它仍然有效

### 步骤 4：更新阶段状态格式（推荐）

v2.0.0 模板使用更结构化的状态格式：

**v1.x 格式：**
```markdown
- [x] Phase 1: Setup ✓
- [ ] Phase 2: Implementation (CURRENT)
```

**v2.0.0 格式：**
```markdown
### Phase 1: Setup
- **状态：** 已完成

### Phase 2: Implementation
- **状态：** 进行中
```

新格式使 `check-complete.sh` 脚本能够自动验证完成情况。

## 破坏性变更

**无。** v2.0.0 完全向后兼容。

如果您更喜欢没有钩子的 v1.x 行为，请使用 `legacy` 分支：

```bash
git checkout legacy
```

## 建议采用的新功能

### 2 行动规则

每进行 2 次查看/浏览/搜索操作后，将发现保存到文件：

```
WebSearch → WebSearch → 必须写入 findings.md
```

### 3 次错误协议

结构化的错误恢复流程：

1. 诊断与修复
2. 替代方法
3. 重新全面思考
4. 上报给用户

### 5 问题重启测试

您的规划文件应回答：

1. 我在哪里？ → 当前阶段
2. 我要去哪里？ → 剩余阶段
3. 目标是什么？ → 目标陈述
4. 我学到了什么？ → findings.md
5. 我做了什么？ → progress.md

## 有问题？

提交问题：https://github.com/OthmanAdi/planning-with-files/issues
