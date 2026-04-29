# 基准测试结果 — planning-with-files v2.22.0

使用 Anthropic 的 [skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) 框架对 `planning-with-files` 进行的正式评估。本文档记录了完整的方法论、测试用例、评分标准和结果。

---

## 评估背景

2026年3月的一次主动安全审计发现了钩子系统中的一个提示注入放大向量。PreToolUse 钩子在每次工具调用前都会重新读取 `task_plan.md` —— 这是该技能生效的机制 —— 但在 `allowed-tools` 中声明 `WebFetch` 和 `WebSearch` 创建了一条路径，使得不受信任的网络内容能够到达该文件，并在每次后续工具使用时被重新注入到上下文中。

在 v2.21.0 版本中进行了加固：从 `allowed-tools` 中移除了 `WebFetch`/`WebSearch`，并在 SKILL.md 中添加了明确的安全边界指导。这些评估记录了性能基线，并验证了工作流保真度零回归。

---

## 测试环境

| 项目 | 值 |
|------|-------|
| 测试的技能版本 | 2.21.0 |
| 评估框架 | Anthropic skill-creator (github.com/anthropics/skills) |
| 执行模型 | claude-sonnet-4-6 |
| 评估日期 | 2026-03-06 |
| 评估仓库 | 本地副本 (planning-with-files-eval-test/) |
| 子代理 | 10个并行 (5个 with_skill + 5个 without_skill) |
| 比较代理 | 3个盲法 A/B 比较 |

---

## 测试 1：评估 + 基准测试

### 技能类别

`planning-with-files` 是一项**编码偏好技能**（非能力提升）。Claude 可以在没有该技能的情况下进行规划 —— 该技能编码了一种特定的 3 文件工作流模式。断言测试的是工作流保真度，而非通用规划能力。

### 测试用例（5个评估）

| ID | 名称 | 任务 |
|----|------|------|
| 1 | todo-cli | 构建一个具有持久化功能的 Python CLI 待办事项工具 |
| 2 | research-frameworks | 研究 Python 测试框架，比较 3 个，推荐一个 |
| 3 | debug-fastapi | 系统性地调试 FastAPI 中的 TypeError |
| 4 | django-migration | 规划一个 5 万行代码的 Django 3.2 → 4.2 迁移 |
| 5 | cicd-pipeline | 为 TypeScript 单体仓库创建一个 CI/CD 计划 |

每个评估同时运行两个子代理：
- **with_skill**：读取 `SKILL.md`，遵循它，在输出目录中创建规划文件
- **without_skill**：自然地执行相同任务，不使用技能或模板

### 每个评估的断言

所有断言都是**客观可验证的**（文件存在性、章节标题、字段计数）：

| 断言 | 评估 |
|-----------|-------|
| 在项目目录中创建了 `task_plan.md` | 全部 5 个 |
| 在项目目录中创建了 `findings.md` | 评估 1,2,4,5 |
| 在项目目录中创建了 `progress.md` | 全部 5 个 |
| `task_plan.md` 中有 `## Goal` 章节 | 评估 1,5 |
| `task_plan.md` 中有 `### Phase` 章节（1个以上） | 全部 5 个 |
| 阶段上有 `**Status:**` 字段 | 全部 5 个 |
| `## Errors Encountered` 章节 | 评估 1,3 |
| `## Current Phase` 章节 | 评估 2 |
| 研究内容在 `findings.md` 中（不在 `task_plan.md` 中） | 评估 2 |
| 4个以上阶段 | 评估 4 |
| `## Decisions Made` 章节 | 评估 4 |

**总断言数：30**

### 结果

| 评估 | with_skill | without_skill | with_skill 文件 | without_skill 文件 |
|------|-----------|---------------|-----------------|---------------------|
| 1 todo-cli | 7/7 (100%) | 0/7 (0%) | task_plan.md, findings.md, progress.md | plan.md, todo.py, test_todo.py |
| 2 research | 6/6 (100%) | 0/6 (0%) | task_plan.md, findings.md, progress.md | framework_comparison.md, recommendation.md, research_plan.md |
| 3 debug | 5/5 (100%) | 0/5 (0%) | task_plan.md, findings.md, progress.md | debug_analysis.txt, routes_users_fixed.py |
| 4 django | 5/6 (83.3%) | 0/6 (0%) | task_plan.md, findings.md, progress.md | django_migration_plan.md |
| 5 cicd | 6/6 (100%) | 2/6 (33.3%) | task_plan.md, findings.md, progress.md | task_plan.md (结构错误) |

**汇总：**

| 配置 | 通过率 | 总通过数 |
|---------------|-----------|-------------|
| with_skill | **96.7%** | 29/30 |
| without_skill | 6.7% | 2/30 |
| **差值** | **+90.0 个百分点** | +27 个断言 |

**时间和令牌使用情况**（来自任务完成通知 —— 运行时捕获）：

| 评估 | with_skill 令牌数 | with_skill 时间 | without_skill 令牌数 | without_skill 时间 |
|------|------------------|-----------------|---------------------|-------------------|
| 1 todo-cli | 17,802 | 99.7s | 13,587 | 76.2s |
| 2 research | 22,150 | 128.7s | 13,610 | 127.3s |
| 3 debug | 17,506 | 93.4s | 11,525 | 66.5s |
| 4 django | 24,049 | 147.9s | 12,351 | 141.4s |
| 5 cicd | 18,122 | 105.0s | 8,424 | 76.7s |
| **平均值** | **19,926** | **115s** | **11,899** | **98s** |

该技能平均多使用约 68% 的令牌和约 17% 的时间。额外的成本在于结构化输出：创建 3 个文件而非 1-2 个，遵循阶段/状态规范，填充决策和错误表。这是预期的权衡 —— 该技能用速度换取结构。

#### 一个断言被细化（评估 4）

断言：`**Status:** pending on at least one future phase`
结果：未满足

代理在一次全面的规划会话中完成了所有 6 个迁移阶段，没有留下任何待处理阶段。技能被正确遵循 —— 该断言过于规定性。该技能不要求阶段保持待处理状态；它要求阶段具有状态字段。为未来评估修订为：`task_plan.md 包含 **Status:** 字段`（不指定值）。

---

## 测试 2：A/B 盲法比较

三个独立的比较代理评估了输出对，**不知道哪个是 with_skill 与 without_skill**。分配是随机的：

| 评估 | A | B | 胜者 | A 分数 | B 分数 |
|------|---|---|--------|---------|---------|
| 1 todo-cli | without_skill | with_skill | **B (with_skill)** | 6.0/10 | 10.0/10 |
| 3 debug-fastapi | with_skill | without_skill | **A (with_skill)** | 10.0/10 | 6.3/10 |
| 4 django-migration | without_skill | with_skill | **B (with_skill)** | 8.0/10 | 10.0/10 |

**with_skill 获胜：3/3 = 100%**

### 比较者引述

**评估 1 (todo-cli):** *"输出 B 精确地满足了所有四个结构化工作流期望... 输出 A 交付了真实、可运行的代码 (todo.py + 完整的测试套件)，这令人印象深刻，但它没有满足结构期望... 输出 A 的优势是真实的，但超出了评估范围。"*

**评估 3 (debug-fastapi):** *"输出 A 在每个评估的期望上都显著优于输出 B。输出 B 是一个称职的临时调试响应，但它不满足评估指定的结构化、多阶段规划格式。输出 A 通过了所有五个期望；输出 B 通过了一个，失败了四个。"*

**评估 4 (django-migration):** *"输出 B 在实质上也很强大：它涵盖了 pytz/zoneinfo 迁移（输出 A 完全忽略了这一点，这是 4.2 特有的项目），将 'django-upgrade' 作为自动化工具推荐包含在内... 18,727 个输出字符与输出 A 的 12,847 个相比，也反映了 B 中更高的信息密度。"*

---

## 测试 3：描述优化器

**状态：此周期未运行**

需要在评估环境中设置 `ANTHROPIC_API_KEY`。根据项目的评估标准，只有能够端到端运行并具有已验证指标的测试才会包含在结果中。已安排在下个评估周期进行。

---

## 总结

| 测试 | 状态 | 结果 |
|------|--------|--------|
| 评估 + 基准测试 | ✅ 完成 | 96.7% (with_skill) vs 6.7% (without_skill) |
| A/B 盲法比较 | ✅ 完成 | with_skill 3/3 获胜 (100%) |
| 描述优化器 | 待定 | 已安排在下个评估周期 |

该技能在不同类型的任务中明确地强制执行了 3 文件规划模式。没有该技能，代理会默认使用临时文件命名，并完全跳过结构化规划工作流。

---

## 复现这些结果

```bash
# 克隆评估框架
gh api repos/anthropics/skills/contents/skills/skill-creator ...

# 设置工作空间
mkdir -p eval-workspace/iteration-1/{eval-1,eval-2,...}/{with_skill,without_skill}/outputs

# 运行 with_skill 子代理
# 提示："读取路径 X 处的 SKILL.md。遵循它。执行：<任务>。保存到：<output_dir>"

# 运行 without_skill 子代理
# 提示："执行：<任务>。保存到：<output_dir>。不使用技能或模板。"

# 对断言评分，生成 benchmark.json
# 查看 eval-workspace/iteration-1/benchmark.json 获取完整数据
```

原始基准数据：[`eval-workspace/iteration-1/benchmark.json`](../planning-with-files-eval-test/eval-workspace/iteration-1/benchmark.json)（在 eval-test 副本中，主仓库未跟踪）
