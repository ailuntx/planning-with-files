<div align="center">
<img src="media/banner.png" alt="文件规划" width="100%">
</div>

# 文件规划

> **像 Manus 一样工作** —— Meta 以 **20 亿美元**收购的 AI 智能体公司。

[![基准测试](https://img.shields.io/badge/基准测试-96.7%25_通过率-brightgreen)](docs/evals.md)
[![A/B 验证](https://img.shields.io/badge/A%2FB_盲测-3%2F3_胜出-brightgreen)](docs/evals.md)
[![Loaditout 安全等级](https://loaditout.ai/badge/OthmanAdi/planning-with-files)](https://loaditout.ai/skills/OthmanAdi/planning-with-files)
[![SkillCheck 验证](https://img.shields.io/badge/SkillCheck-已验证-4c1)](https://getskillcheck.com)
[![安全验证](https://img.shields.io/badge/安全-已审计并修复_v2.21.0-blue)](docs/evals.md)

[![Skills Playground](https://skillsplayground.com/badges/installs/othmanadi-planning-with-files-planning-with-files.svg)](https://skillsplayground.com/skills/othmanadi-planning-with-files-planning-with-files/)
[![下载量](https://skill-history.com/badge/othmanadi/planning-with-files.svg)](https://skill-history.com/othmanadi/planning-with-files)
[![版本](https://img.shields.io/badge/版本-2.34.1-brightgreen)](https://github.com/OthmanAdi/planning-with-files/releases)
[![许可：MIT](https://img.shields.io/badge/许可-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![已关闭 Issues](https://img.shields.io/github/issues-closed/OthmanAdi/planning-with-files?color=success)](https://github.com/OthmanAdi/planning-with-files/issues?q=is%3Aissue+is%3Aclosed)
[![已关闭 PRs](https://img.shields.io/github/issues-pr-closed/OthmanAdi/planning-with-files?color=success)](https://github.com/OthmanAdi/planning-with-files/pulls?q=is%3Apr+is%3Aclosed)

<details>
<summary><strong>💬 来自作者的话</strong></summary>

致所有为此技能加星、复刻和分享的人——谢谢你们。这个项目在不到 24 小时内就爆火了，社区的支持令人难以置信。

如果这个技能能帮助你更聪明地工作，那就是我全部的愿望。

</details>

<details>
<summary><strong>🌍 社区贡献了什么</strong></summary>

### 复刻与扩展

| 分支 | 作者 | 他们构建了什么 |
|------|--------|-----------------|
| [devis](https://github.com/st01cs/devis) | [@st01cs](https://github.com/st01cs) | 面试优先工作流，`/devis:intv` 和 `/devis:impl` 命令，保证激活 |
| [multi-manus-planning](https://github.com/kmichels/multi-manus-planning) | [@kmichels](https://github.com/kmichels) | 多项目支持，SessionStart git 同步 |
| [plan-cascade](https://github.com/Taoidle/plan-cascade) | [@Taoidle](https://github.com/Taoidle) | 多层级任务编排，并行执行，多智能体协作 |
| [agentfund-skill](https://github.com/RioTheGreat-ai/agentfund-skill) | [@RioTheGreat-ai](https://github.com/RioTheGreat-ai) | 基于 Base 的里程碑式托管，为 AI 智能体众筹 |
| [openclaw-github-repo-commander](https://github.com/wd041216-bit/openclaw-github-repo-commander) | [@wd041216-bit](https://github.com/wd041216-bit) | 面向 OpenClaw 的 7 阶段 GitHub 仓库审计、优化和清理工作流 |

### 实际项目中的应用

| 项目 | 是什么 |
|---------|-----------|
| [lincolnwan/Planning-with-files-copilot-agent](https://github.com/lincolnwan/Planning-with-files-copilot-agent) | 围绕 planning-with-files 技能构建的完整 Copilot 智能体仓库 |
| [cooragent/ClarityFinance](https://github.com/cooragent/ClarityFinance) | AI 金融智能体框架——直接借鉴了 Planning-with-Files 方法 |
| [oeftimie/vv-claude-harness](https://github.com/oeftimie/vv-claude-harness) | 基于 Manus 风格持久化 markdown 规划构建的 Claude Code 工具 |
| [jessepwj/CCteam-creator](https://github.com/jessepwj/CCteam-creator) | 使用基于文件的规划的多智能体团队编排技能 |

*有构建了什么吗？ [提交 issue](https://github.com/OthmanAdi/planning-with-files/issues) 以列入列表！*

</details>

<details>
<summary><strong>🤝 贡献者</strong></summary>

查看 [CONTRIBUTORS.md](./CONTRIBUTORS.md) 中所有为此项目做出贡献的完整列表。

</details>

<details>
<summary><strong>📦 发布版本 & 会话恢复</strong></summary>

### 当前版本：v2.34.1

| 版本 | 亮点 |
|---------|------------|
| **v2.34.1** | **停止钩子 Windows 可移植性修复**（关闭 #133）：在 Windows Git Bash 钩子上下文中 `export SD=` 失败；插件缓存结构的回退路径有误。在所有 13 个 SKILL.md 变体中修复。（感谢 @nazeshinjite!） |
| **v2.34.0** | **Codex 钩子完全恢复**（关闭 #132）：`.codex/hooks.json` + 生命周期脚本回归 —— SessionStart、UserPromptSubmit、PreToolUse、PostToolUse、Stop。针对 SKILL.md 质量审查的 Tessl CI。可执行位修复。添加了 4 位缺失的贡献者。（感谢 @Leon-Algo、@popey!） |
| **v2.33.0** | **多语言扩展**：新增阿拉伯语、德语、西班牙语技能变体（感谢社区贡献者！） |
| **v2.32.0** | Codex 会话追赶重写（感谢 @ebrevdo!），Loaditout A 级安全徽章，停止钩子 Git Bash 修复 |
| **v2.31.0** | Codex hooks.json 集成及完整生命周期钩子（感谢 @Leon-Algo!） |
| **v2.30.1** | 修复：恢复 Codex 脚本可执行位（感谢 @Leon-Algo!） |
| **v2.30.0** | `CLAUDE_SKILL_DIR` 变量，IDE 配置移至每个 IDE 分支，plugin.json 从 2.23.0 升级 |
| **v2.29.0** | 分析工作流模板：`--template analytics` 标志用于数据探索会话（感谢 @mvanhorn!） |
| **v2.28.0** | 正体中文 (zh-TW) 技能变体（感谢 @waynelee2048!） |
| **v2.26.2** | 修复：钩子命令中的 `---` 破坏了 YAML 前置内容解析，钩子现在可以正确注册 |
| **v2.26.1** | 修复：`/clear` 后的会话追赶，Windows 上的路径清理 + 内容注入（感谢 @tony-stark-eth!） |
| **v2.26.0** | IDE 审计：Factory 钩子，Copilot errorOccurred 钩子，Gemini 钩子，bug 修复 |
| **v2.18.2** | Mastra Code 钩子修复（hooks.json + 文档准确性） |
| **v2.18.1** | Copilot 乱码的完整修复 |
| **v2.18.0** | BoxLite 沙盒运行时集成 |
| **v2.17.0** | Mastra Code 支持 + 所有 IDE SKILL.md 规范修复 |
| **v2.16.1** | Copilot 乱码修复：PS1 UTF-8 编码 + bash ensure_ascii（感谢 @Hexiaopi!） |
| **v2.16.0** | GitHub Copilot 钩子支持（感谢 @lincolnwan!） |
| **v2.27.0** | Kiro Agent Skill 布局（感谢 @EListenX!） |
| **v2.15.1** | 会话追赶误报修复（感谢 @gydx6!） |
| **v2.15.0** | `/plan:status` 命令，OpenCode 兼容性修复 |
| **v2.14.0** | Pi Agent 支持，OpenClaw 文档更新，Codex 路径修复 |
| **v2.11.0** | `/plan` 命令便于自动补全 |
| **v2.10.0** | Kiro 引导文件支持 |
| **v2.7.0** | Gemini CLI 支持 |
| **v2.2.0** | 会话恢复，Windows PowerShell，操作系统感知钩子 |

[查看所有发布版本](https://github.com/OthmanAdi/planning-with-files/releases) · [更新日志](CHANGELOG.md)

> 🧪 **实验性：** 独立并行规划（`.planning/{uuid}/` 文件夹）正在 [`experimental/isolated-planning`](https://github.com/OthmanAdi/planning-with-files/tree/experimental/isolated-planning) 分支上进行测试。欢迎试用并分享反馈！

---

### 会话恢复

当你的上下文已满，运行 `/clear` 时，此技能会自动恢复你之前的会话。

**如何工作：**
1. 检查活跃 IDE 的会话存储中是否有上一会话的数据（Claude Code 用 `~/.claude/projects/`，Codex 用 `~/.codex/sessions/`）
2. 找到规划文件上次更新的时间
3. 提取之后发生的对话（可能是丢失的上下文）
4. 显示追赶报告以便同步

**专业提示：** 在清除之前禁用自动压缩以最大化上下文：
```json
{ "autoCompact": false }
```

</details>

<details>
<summary><strong>🛠️ 支持的 IDE（17+ 平台）</strong></summary>

#### 增强支持（钩子 + 生命周期自动化）

以下 IDE 有专用的钩子配置，能在使用工具前自动重新读取你的计划，提醒你更新进度，并在停止前验证完成：

| IDE | 安装指南 | 集成方式 |
|-----|-------------------|-------------|
| Claude Code | [安装](docs/installation.md) | 插件 + SKILL.md + 钩子 |
| Cursor | [Cursor 设置](docs/cursor.md) | 技能 + [hooks.json](https://cursor.com/docs/hooks) |
| GitHub Copilot | [Copilot 设置](docs/copilot.md) | [钩子](https://docs.github.com/en/copilot/reference/hooks-configuration)（含 errorOccurred） |
| Mastra Code | [Mastra 设置](docs/mastra.md) | 技能 + [钩子](https://mastra.ai/docs/mastra-code/configuration) |
| Gemini CLI | [Gemini 设置](docs/gemini.md) | 技能 + [钩子](https://geminicli.com/docs/hooks/) |
| Kiro | [Kiro 设置](docs/kiro.md) | [智能体技能](https://kiro.dev/docs/skills/) |
| Codex | [Codex 设置](docs/codex.md) | [技能 + 钩子](https://developers.openai.com/codex/skills) |
| Hermes Agent | [Hermes 设置](docs/hermes.md) | 技能 + 项目插件 |
| CodeBuddy | [CodeBuddy 设置](docs/codebuddy.md) | [技能 + 钩子](https://www.codebuddy.ai/docs/cli/skills) |
| FactoryAI Droid | [Factory 设置](docs/factory.md) | [技能 + 钩子](https://docs.factory.ai/cli/configuration/skills) |
| OpenCode | [OpenCode 设置](docs/opencode.md) | 技能 + 自定义会话存储 |

#### 标准智能体技能支持

以下 IDE 实现了 [Agent Skills](https://agentskills.io) 开放规范。使用 `npx skills add` 安装——安装器会将技能自动放入每个 IDE 的发现路径中：

| IDE | 安装指南 | 技能发现路径 |
|-----|-------------------|---------------------|
| Continue | [Continue 设置](docs/continue.md) | `.continue/skills/` + [.prompt 文件](https://docs.continue.dev/customize/deep-dives/prompts) |
| Pi Agent | [Pi Agent 设置](docs/pi-agent.md) | `.pi/skills/` ([npm 包](https://www.npmjs.com/package/@mariozechner/pi-coding-agent)) |
| OpenClaw | [OpenClaw 设置](docs/openclaw.md) | `.openclaw/skills/` ([文档](https://docs.openclaw.ai/tools/skills)) |
| Antigravity | [Antigravity 设置](docs/antigravity.md) | `.agent/skills/` ([文档](https://codelabs.developers.google.com/getting-started-with-antigravity-skills)) |
| Kilocode | [Kilocode 设置](docs/kilocode.md) | `.kilocode/skills/` ([文档](https://kilo.ai/docs/agent-behavior/skills)) |
| AdaL CLI (Sylph AI) | [AdaL 设置](docs/adal.md) | `.adal/skills/` ([文档](https://docs.sylph.ai/features/plugins-and-skills)) |

> **注意：** 如果你的 IDE 使用的是旧版 Rules 系统而非 Skills，请参见 [`legacy-rules-support`](https://github.com/OthmanAdi/planning-with-files/tree/legacy-rules-support) 分支。

</details>

<details>
<summary><strong>🧱 沙盒运行时（1 平台）</strong></summary>

| 运行时 | 状态 | 指南 | 备注 |
|---------|--------|-------|-------|
| BoxLite | ✅ 已文档化 | [BoxLite 设置](docs/boxlite.md) | 在硬件隔离的微虚拟机中运行 Claude Code + planning-with-files |

> **注意：** BoxLite 是一个沙盒运行时，而非 IDE。技能通过 [ClaudeBox](https://github.com/boxlite-ai/claudebox) 加载——BoxLite 的官方 Claude Code 集成层。

</details>

---

一个用于改造你的工作流的 Claude Code 插件，使用持久化的 markdown 文件进行规划、进度追踪和知识存储——这正是让 Manus 价值数十亿美元的核心模式。

[![Claude Code 插件](https://img.shields.io/badge/Claude%20Code-插件-blue)](https://code.claude.com/docs/en/plugins)
[![Claude Code 技能](https://img.shields.io/badge/Claude%20Code-技能-green)](https://code.claude.com/docs/en/skills)
[![Cursor 技能](https://img.shields.io/badge/Cursor-技能-purple)](https://docs.cursor.com/context/skills)
[![Kilocode 技能](https://img.shields.io/badge/Kilocode-技能-orange)](https://kilo.ai/docs/agent-behavior/skills)
[![Gemini CLI](https://img.shields.io/badge/Gemini%20CLI-技能-4285F4)](https://geminicli.com/docs/cli/skills/)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-技能-FF6B6B)](https://openclaw.ai)
[![Kiro](https://img.shields.io/badge/Kiro-智能体技能-00D4AA)](https://kiro.dev/docs/skills/)
[![AdaL CLI](https://img.shields.io/badge/AdaL%20CLI-技能-9B59B6)](https://docs.sylph.ai/features/plugins-and-skills)
[![Pi Agent](https://img.shields.io/badge/Pi%20Agent-技能-FF4081)](https://pi.dev)
[![GitHub Copilot](https://img.shields.io/badge/GitHub%20Copilot-钩子-000000)](https://docs.github.com/en/copilot/reference/hooks-configuration)
[![Mastra Code](https://img.shields.io/badge/Mastra%20Code-技能-00BCD4)](https://code.mastra.ai)
[![Hermes](https://img.shields.io/badge/Hermes-智能体技能-5C7CFA)](docs/hermes.md)
[![BoxLite](https://img.shields.io/badge/BoxLite-沙盒-6C3483)](https://boxlite.ai)

## 快速安装

```bash
npx skills add OthmanAdi/planning-with-files --skill planning-with-files -g
```

<details>
<summary><strong>🌐 提供其他 5 种语言</strong></summary>

**🇸🇦 العربية / Arabic**
```bash
npx skills add OthmanAdi/planning-with-files --skill planning-with-files-ar -g
```

**🇩🇪 Deutsch / German**
```bash
npx skills add OthmanAdi/planning-with-files --skill planning-with-files-de -g
```

**🇪🇸 Español / Spanish**
```bash
npx skills add OthmanAdi/planning-with-files --skill planning-with-files-es -g
```

**🇨🇳 中文版 / Chinese (Simplified)**
```bash
npx skills add OthmanAdi/planning-with-files --skill planning-with-files-zh -g
```

**🇹🇼 正體中文版 / Chinese (Traditional)**
```bash
npx skills add OthmanAdi/planning-with-files --skill planning-with-files-zht -g
```

</details>

适用于 Claude Code、Cursor、Codex、Gemini CLI，以及 40+ 个支持 [Agent Skills](https://agentskills.io) 规范的智能体。

<details>
<summary><strong>🔧 Claude Code 插件（高级功能）</strong></summary>

针对 Claude Code 特有的功能，如 `/plan` 自动补全命令：

```
/plugin marketplace add OthmanAdi/planning-with-files
/plugin install planning-with-files@planning-with-files
```

</details>

就这样！现在在 Claude Code 中使用以下命令之一：

| 命令 | 自动补全 | 描述 |
|---------|--------------|-------------|
| `/planning-with-files:plan` | 输入 `/plan` | 开始规划会话（v2.11.0+） |
| `/planning-with-files:status` | 输入 `/plan:status` | 一目了然地显示规划进度（v2.15.0+） |
| `/planning-with-files:start` | 输入 `/planning` | 原始启动命令 |

**替代方案：** 如果你想要 `/planning-with-files`（不带前缀），将技能复制到你的本地文件夹：

**macOS/Linux:**
```bash
cp -r ~/.claude/plugins/cache/planning-with-files/planning-with-files/*/skills/planning-with-files ~/.claude/skills/
```

**Windows (PowerShell):**
```powershell
Copy-Item -Recurse -Path "$env:USERPROFILE\.claude\plugins\cache\planning-with-files\planning-with-files\*\skills\planning-with-files" -Destination "$env:USERPROFILE\.claude\skills\"
```

所有安装方式请参见 [docs/installation.md](docs/installation.md)。

## 为什么需要这个技能？

在 2025 年 12 月 29 日，[Meta 以 20 亿美元收购了 Manus](https://techcrunch.com/2025/12/29/meta-just-bought-manus-an-ai-startup-everyone-has-been-talking-about/)。短短 8 个月内，Manus 从上线到营收超 1 亿美元。他们的秘密？**上下文工程**。

> “Markdown 是我在磁盘上的‘工作记忆’。由于我迭代地处理信息，而有限的活跃上下文有限，Markdown 文件充当了笔记草稿、进度检查点以及最终交付成果的构建模块。”
> — Manus AI

## 问题所在

Claude Code（以及大多数 AI 智能体）都面临以下问题：

- **易失性记忆** —— 上下文重置时，TodoWrite 工具就会消失
- **目标漂移** —— 50 多次工具调用后，最初的目标被遗忘
- **隐藏的错误** —— 失败不被追踪，同样的错误重复出现
- **上下文填塞** —— 所有内容都被塞进上下文，而不是存储起来

## 解决方案：3 文件模式

对于每一项复杂任务，创建三个文件：

```
task_plan.md      → 追踪阶段和进度
findings.md       → 存储研究和发现
progress.md       → 会话日志和测试结果
```

### 核心原则

```
上下文窗口 = RAM（易失，有限）
文件系统 = 磁盘（持久，无限）

→ 任何重要的内容都写入磁盘。
```

## Manus 原则

| 原则 | 实现方式 |
|-----------|----------------|
| 文件系统作为记忆 | 存储在文件中，而非上下文 |
| 注意力操控 | 决策前重新阅读计划（钩子） |
| 错误持久化 | 在计划文件中记录失败 |
| 目标追踪 | 复选框中显示进度 |
| 完成验证 | 停止钩子检查所有阶段 |

## 使用方式

安装后，AI 智能体将：

1. 如果未提供描述，**询问你的任务**
2. 在你的项目目录中**创建 `task_plan.md`、`findings.md` 和 `progress.md`**
3. 在重要决策前**重新阅读计划**（通过 PreToolUse 钩子）
4. 写入文件后**提醒你更新状态**（通过 PostToolUse 钩子）
5. 将发现**存储在 `findings.md`** 中，而不是填充上下文
6. 为未来参考**记录错误**
7. 在停止前**验证完成**（通过停止钩子）

使用方式：
- `/planning-with-files:plan` —— 输入 `/plan` 可在自动补全中找到（v2.11.0+）
- `/planning-with-files:start` —— 输入 `/planning` 可在自动补全中找到
- `/planning-with-files` —— 仅当你已将技能复制到 `~/.claude/skills/`

完整 5 步指南请参见 [docs/quickstart.md](docs/quickstart.md)。

## 基准测试结果

使用 Anthropic 的 [skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) 框架（v2.22.0）进行了正式评估。10 个并行子智能体，5 种任务类型，30 项可客观验证的断言，3 次盲测 A/B 比较。

| 测试 | with_skill | without_skill |
|------|-----------|---------------|
| 通过率（30 项断言） | **96.7%** (29/30) | 6.7% (2/30) |
| 遵循 3 文件模式 | 5/5 评估 | 0/5 评估 |
| 盲测 A/B 胜出 | **3/3 (100%)** | 0/3 |
| 平均评分标准得分 | **10.0/10** | 6.8/10 |

[完整方法和结果](docs/evals.md) · [技术文章](docs/article.md)

## 关键规则

1. **首先创建计划** —— 无 `task_plan.md` 不开始
2. **2 操作规则** —— 每两次查看/浏览操作后保存发现
3. **记录所有错误** —— 它们有助于避免重复
4. **永不重复失败** —— 追踪尝试，改变方法

## 何时使用

**适合以下场景使用此模式：**
- 多步骤任务（3 步以上）
- 研究任务
- 构建/创建项目
- 跨越大量工具调用的任务

**以下情况可跳过：**
- 简单问题
- 单一文件编辑
- 快速查询

## 文件结构

```
planning-with-files/
├── commands/                # 插件命令
│   ├── plan.md              # /planning-with-files:plan 命令（v2.11.0+）
│   ├── plan-ar.md           # 阿拉伯语 /plan 命令（v2.33.0+）
│   ├── plan-de.md           # 德语 /plan 命令（v2.33.0+）
│   ├── plan-es.md           # 西班牙语 /plan 命令（v2.33.0+）
│   └── start.md             # /planning-with-files:start 命令
├── templates/               # 根级模板（用于 CLAUDE_PLUGIN_ROOT）
├── scripts/                 # 根级脚本（用于 CLAUDE_PLUGIN_ROOT）
├── docs/                    # 文档
│   ├── installation.md
│   ├── quickstart.md
│   ├── workflow.md
│   ├── troubleshooting.md
│   ├── gemini.md            # Gemini CLI 设置
│   ├── cursor.md
│   ├── windows.md
│   ├── kilocode.md
│   ├── codex.md
│   ├── opencode.md
│   ├── mastra.md             # Mastra Code 设置
│   └── boxlite.md            # BoxLite 沙盒设置
├── examples/                # 集成示例
│   └── boxlite/             # BoxLite 快速开始
│       ├── README.md
│       └── quickstart.py
├── planning-with-files/     # 插件技能文件夹
│   ├── SKILL.md
│   ├── templates/
│   └── scripts/
├── skills/                  # 技能变体
│   ├── planning-with-files/     # 英文（默认）
│   │   ├── SKILL.md
│   │   ├── examples.md
│   │   ├── reference.md
│   │   ├── templates/
│   │   └── scripts/
│   │       ├── init-session.sh
│   │       ├── check-complete.sh
│   │       ├── init-session.ps1   # Windows PowerShell
│   │       └── check-complete.ps1 # Windows PowerShell
│   ├── planning-with-files-ar/   # 阿拉伯语（v2.33.0+）
│   │   ├── SKILL.md
│   │   ├── templates/
│   │   └── scripts/
│   ├── planning-with-files-de/   # 德语（v2.33.0+）
│   │   ├── SKILL.md
│   │   ├── templates/
│   │   └── scripts/
│   ├── planning-with-files-es/   # 西班牙语（v2.33.0+）
│   │   ├── SKILL.md
│   │   ├── templates/
│   │   └── scripts/
│   ├── planning-with-files-zh/   # 简体中文（v2.25.0+）
│   └── planning-with-files-zht/  # 正体中文（v2.28.0+）
├── .gemini/                 # Gemini CLI 技能 + 钩子
│   ├── settings.json        # 钩子配置（v2.26.0）
│   ├── hooks/               # 钩子脚本（SessionStart、BeforeTool、AfterTool、BeforeModel、SessionEnd）
│   └── skills/
│       └── planning-with-files/
├── .codex/                  # Codex CLI 技能 + 钩子
│   └── skills/
├── .opencode/               # OpenCode 技能（自定义会话存储）
│   └── skills/
├── .claude-plugin/          # 插件清单
├── .cursor/                 # Cursor 技能 + 钩子
│   ├── hooks.json           # 钩子配置
│   ├── hooks/               # 钩子脚本（bash + PowerShell）
│   └── skills/
├── .codebuddy/              # CodeBuddy 技能 + 钩子
│   └── skills/
├── .factory/                # FactoryAI Droid 技能 + 钩子（v2.26.0）
│   └── skills/
├── .pi/                     # Pi Agent 技能（npm 包）
│   └── skills/
│       └── planning-with-files/
├── .continue/               # Continue.dev 技能 + prompt 文件
│   ├── prompts/             # .prompt 文件用于斜杠命令
│   └── skills/
├── .github/                 # GitHub Copilot 钩子（含 errorOccurred）
│   └── hooks/
│       ├── planning-with-files.json  # 钩子配置
│       └── scripts/         # 钩子脚本（bash + PowerShell）
├── .mastracode/             # Mastra Code 技能 + 钩子
│   └── skills/
├── .kiro/                   # Kiro 智能体技能（v2.27.0+）
│   └── skills/
├── CHANGELOG.md
├── CITATION.cff
├── LICENSE
└── README.md
```

## 文档

所有平台设置指南和文档位于 [docs/](./docs/) 文件夹中。


## 致谢

- **Manus AI** —— 因其开创性的上下文工程模式
- **Anthropic** —— 为 Claude Code、Agent Skills 和插件系统
- **Lance Martin** —— 详细的 Manus 架构分析
- 基于 [AI 智能体的上下文工程：构建 Manus 的教训](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)

## 贡献

欢迎贡献！请：
1. 复刻仓库
2. 创建功能分支
3. 提交拉取请求

## 许可

MIT 许可 —— 可自由使用、修改和分发。

---

**作者：** [Ahmad Othman Ammar Adi](https://github.com/OthmanAdi)

## Star 历史

<a href="https://repostars.dev/?repos=OthmanAdi%2Fplanning-with-files&theme=copper"><img src="https://repostars.dev/api/embed?repo=OthmanAdi%2Fplanning-with-files&theme=copper" width="100%" alt="Star 历史图表" /></a>
