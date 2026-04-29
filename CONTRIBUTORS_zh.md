# 贡献者

感谢所有为让 `planning-with-files` 变得更好做出贡献的人！

## 项目作者

- **[Ahmad Othman Ammar Adi](https://github.com/OthmanAdi)** - 最初的创建者和维护者

## 代码贡献者

这些杰出的人们为项目贡献了代码、文档或重大改进：

### 主要贡献

- **[@kaichen](https://github.com/kaichen)** - [PR #9](https://github.com/OthmanAdi/planning-with-files/pull/9)
  - 将仓库转换为 Claude Code 插件结构
  - 启用了市场安装
  - 遵循官方插件标准
  - **影响：** 使该技能能够被大众使用

- **[@fuahyo](https://github.com/fuahyo)** - [PR #12](https://github.com/OthmanAdi/planning-with-files/pull/12)
  - 添加了“构建一个待办事项应用”的演练，包含 4 个阶段
  - 为模板创建了内联注释（WHAT/WHY/WHEN/EXAMPLE）
  - 开发了带有 ASCII 参考表的快速入门指南
  - 创建了展示任务生命周期的工作流图表
  - **影响：** 极大地改善了新手上手体验

- **[@lasmarois](https://github.com/lasmarois)** - [PR #33](https://github.com/OthmanAdi/planning-with-files/pull/33)、[PR #37](https://github.com/OthmanAdi/planning-with-files/pull/37)
  - 创建了会话恢复功能，用于在 `/clear` 后保留上下文
  - 构建了 `session-catchup.py` 脚本，用于分析之前的会话 JSONL 文件
  - 增强了 PreToolUse 钩子，以包含 Read/Glob/Grep 操作
  - 重组了 SKILL.md，以提供更好的会话恢复工作流（PR #33）
  - 将回溯扫描扩展到所有会话，而不仅仅是最近的一个（PR #37）
  - **影响：** 解决了上下文丢失问题，支持跨任何会话无缝恢复工作

- **[@aimasteracc](https://github.com/aimasteracc)** - [PR #30](https://github.com/OthmanAdi/planning-with-files/pull/30)
  - 添加了 Kilocode IDE 支持及相关文档
  - 为 Windows 兼容性创建了 PowerShell 脚本
  - 添加了 `.kilocode/rules/` 配置
  - 更新了文档以支持多 IDE
  - **影响：** Windows 兼容性和 IDE 生态系统的扩展

- **[@SaladDay](https://github.com/SaladDay)** - [PR #57](https://github.com/OthmanAdi/planning-with-files/pull/57)
  - 修复了 Stop 钩子在 Debian/Ubuntu 上的 POSIX sh 兼容性
  - 将 bash 特有语法（`[[`、`&>`）替换为 POSIX 结构
  - 添加了使用 `uname -s` 的跨 Shell Windows 检测方案
  - **影响：** 修复了在使用 dash 作为 `/bin/sh` 的系统上钩子失败的问题

- **[@murphyXu](https://github.com/murphyXu)** - [PR #56](https://github.com/OthmanAdi/planning-with-files/pull/56)
  - 添加了 Continue IDE 集成（VS Code / JetBrains）
  - 创建了 `.continue/skills/` 和 `.continue/prompts/` 结构
  - 添加了中文斜杠命令提示
  - 创建了 `docs/continue.md` 安装指南
  - **影响：** 将 IDE 支持扩展到 Continue.dev 生态系统

- **[@ZWkang](https://github.com/ZWkang)** - [PR #60](https://github.com/OthmanAdi/planning-with-files/pull/60)
  - 添加了 CodeBuddy IDE 集成（腾讯云 AI 编程助手）
  - 创建了包含完整技能结构的 `.codebuddy/skills/` 文件夹
  - 为 CodeBuddy 添加了模板、脚本和参考资料
  - 创建了 `docs/codebuddy.md` 安装指南
  - **影响：** 将 IDE 支持扩展到 CodeBuddy 生态系统

- **[@EListenX](https://github.com/EListenX)** (Yi Chenxi) - [PR #112](https://github.com/OthmanAdi/planning-with-files/pull/112)
  - 在 `.kiro/skills/planning-with-files/` 下添加了完整的 Kiro Agent Skill 支持
  - 创建了引导脚本，通过 `#[[file:]]` 实时引用实现了引导集成
  - 将旧的 `.kiro/scripts/` 和 `.kiro/steering/` 替换为合适的 Agent Skill 布局
  - 更新了 Cursor 和 Mastra Code 钩子，改进了 docs/kiro.md
  - **影响：** 以原生的 Agent Skill 格式使 Kiro IDE 支持达到生产质量水平

- **[@lincolnwan](https://github.com/lincolnwan)** - [PR #80](https://github.com/OthmanAdi/planning-with-files/pull/80)
  - 使用 2026 年初的钩子系统添加了原生 GitHub Copilot 钩子集成
  - 创建了 `.github/hooks/planning-with-files.json`，并在 `.github/hooks/scripts/` 中附带了完整的钩子脚本
  - 全面的跨平台支持（bash + PowerShell）以及 `docs/copilot.md` 安装指南
  - **影响：** 使支持平台总数达到 15 个，将该技能扩展到 GitHub Copilot 生态系统

- **[@ciberponk](https://github.com/ciberponk)** - [PR #77](https://github.com/OthmanAdi/planning-with-files/pull/77)
  - 添加了带有 UUID 生成和 PLAN_ID 钉定的独立 `.planning/{uuid}/` 计划会话
  - 支持在不同终端中并行进行计划会话，避免状态冲突
  - 跨平台脚本（bash + PowerShell），为单会话用户保留了完整的向后兼容性
  - **影响：** 解锁了并行计划工作流，已发布到实验分支，即将合入主分支

- **[@ttttmr](https://github.com/ttttmr)** - [PR #67](https://github.com/OthmanAdi/planning-with-files/pull/67)
  - 添加了 Pi Agent 支持，实现完整的技能集成
  - **影响：** 将该技能扩展到 Pi Agent 生态系统

- **[@mvanhorn](https://github.com/mvanhorn)** (Matt Van Horn) - [PR #115](https://github.com/OthmanAdi/planning-with-files/pull/115)
  - 添加了分析工作流模板，在 `init-session.sh` 和 `init-session.ps1` 上提供了 `--template analytics` 标志
  - 创建了包含 4 个分析专属阶段（数据发现、探索性分析、假设检验、综合总结）的 `analytics_task_plan.md`
  - 创建了包含数据源表、假设日志、查询结果和统计发现等部分的 `analytics_findings.md`
  - 特定于分析的 `progress.md`，用查询日志替代了测试结果
  - **影响：** 将计划模式扩展到数据分析工作流（解决了 #103）

- **[@ebrevdo](https://github.com/ebrevdo)** (Eugene Brevdo) - [PR #124](https://github.com/OthmanAdi/planning-with-files/pull/124)
  - 重写了 `session-catchup.py` 以支持 Codex 的 rollout JSONL 会话格式
  - 添加了 `CODEX_THREAD_ID` 偏好设置、子代理/微型会话过滤以及结构化的 `patch_apply_end` 事件检测
  - 更新了测试和文档，以适应新的 Codex 回溯行为
  - **影响：** 为 Codex 用户带来了会话恢复对等的能力

- **[@bailob](https://github.com/bailob)** - [PR #136](https://github.com/OthmanAdi/planning-with-files/pull/136)
  - 添加了 Hermes 适配器，包含项目插件、面向 Hermes 的 `planning-with-files` 技能以及 `/plan` 和 `/plan-status` 命令包装器
  - 将 Hermes 技能模板和脚本打包在 `.hermes/skills/planning-with-files/` 中，并通过活动配置文件的 `HERMES_HOME` 进行解析
  - 添加了 20 个单元测试，覆盖状态解析、提醒行为、安装布局和完成检查
  - **影响：** 将 planning-with-files 作为第 17 个平台引入 Hermes 生态系统

### 其他贡献者

- **[@gavinlinasd](https://github.com/gavinlinasd)** - [PR #135](https://github.com/OthmanAdi/planning-with-files/pull/135)
  - 在 README 中添加了 ClawHub 下载历史图表，跟踪随时间推移的技能下载增长情况
  - **影响：** 访问者现在可以一目了然地看到下载势头

- **[@xiaolai](https://github.com/xiaolai)** - [PR #137](https://github.com/OthmanAdi/planning-with-files/pull/137)、[PR #138](https://github.com/OthmanAdi/planning-with-files/pull/138)、[PR #139](https://github.com/OthmanAdi/planning-with-files/pull/139)、[Issue #140](https://github.com/OthmanAdi/planning-with-files/issues/140)
  - 对插件运行了 NLPM（自然语言编程管理器）审计，并提交了 3 个针对性修复的 PR，外加一份完整的审计摘要 issue（总分 91/100）
  - PR #137：修复了 Pi 变体 PowerShell 会话回溯调用中缺少引号的问题，该错误导致命令在 Windows 上静默失败
  - PR #138：在注入模型上下文前将会话回溯输出限制为 100 行，并添加了带标签的前缀，关闭了一个来自存储会话内容的提示注入漏洞
  - PR #139：在 `session-start.sh`、`pre-tool-use.sh` 和 `error-occurred.sh` 中优先使用已知的系统 Python 路径，而非未限定的 PATH 解析
  - **影响：** 在一次性协调审计中强化了 Copilot 钩子脚本和 Pi 变体

- **[@Leon-Algo](https://github.com/Leon-Algo)** - [PR #119](https://github.com/OthmanAdi/planning-with-files/pull/119)、[PR #120](https://github.com/OthmanAdi/planning-with-files/pull/120)、[PR #122](https://github.com/OthmanAdi/planning-with-files/pull/122)
  - 使 `.codex` 技能安装中的计划脚本可执行，修复了 Codex 安装程序中断的问题（PR #119）
  - 添加了官方的 Codex hooks.json 集成，包含完整的生命周期钩子——SessionStart、UserPromptSubmit、PreToolUse、PostToolUse、Stop——使 Codex 与其他 IDE 达到完整的钩子对等（PR #120）
  - 修复了 `check-complete.sh` 和 `init-session.sh` 的规范脚本执行位问题，并进行了回归测试（PR #122）
  - **影响：** Codex 用户现在能获得与 Claude Code 和 Cursor 用户相同的自动上下文注入和生命周期自动化功能

- **[@YSAA1](https://github.com/YSAA1)** - [PR #109](https://github.com/OthmanAdi/planning-with-files/pull/109)
  - 修复了在会话路径更改后 Codex 会话回溯回退静默失败的问题

- **[@kevinaimonster](https://github.com/kevinaimonster)** - [PR #108](https://github.com/OthmanAdi/planning-with-files/pull/108)
  - 添加了简体中文本地化支持，将该技能扩展到中文用户群

- **[@wd041216-bit](https://github.com/wd041216-bit)** - [PR #107](https://github.com/OthmanAdi/planning-with-files/pull/107)
  - 将 openclaw-github-repo-commander 添加到“社区构建”部分，扩展了生态系统展示

- **[@popey](https://github.com/popey)** - [PR #83](https://github.com/OthmanAdi/planning-with-files/pull/83)
  - 修复了 `allowed-tools` YAML 列表（根据 Anthropic 技能规范无效，会静默破坏可发现性）
  - 修复了 `metadata.version` 的放置位置，并添加了触发术语以更好地匹配技能
  - 应用于规范的 SKILL.md 文件

- **[@jonthebeef](https://github.com/jonthebeef)** - [PR #75](https://github.com/OthmanAdi/planning-with-files/pull/75)
  - 添加了 `/plan:status` 命令，用于快速显示计划进度而无需通读所有计划文件

- **[@codelyc](https://github.com/codelyc)** - [PR #66](https://github.com/OthmanAdi/planning-with-files/pull/66)、[PR #70](https://github.com/OthmanAdi/planning-with-files/pull/70)、[PR #76](https://github.com/OthmanAdi/planning-with-files/pull/76)
  - 修复了 Codex 技能路径引用问题，并用正确的绝对路径替换了 CLAUDE_PLUGIN_ROOT（PR #66）
  - 修复了 CodeBuddy 技能路径引用和环境变量问题（PR #70）
  - 为 planning-with-files 技能添加了 OpenCode 脚本（PR #76）

- **[@Guozihong](https://github.com/Guozihong)** - [PR #51](https://github.com/OthmanAdi/planning-with-files/pull/51)
  - 添加了 `/planning-with-files:start` 命令，无需手动复制文件即可激活技能

- **[@fahmyelraie](https://github.com/fahmyelraie)** - [PR #49](https://github.com/OthmanAdi/planning-with-files/pull/49)
  - 修复了当环境中未设置 CLAUDE_PLUGIN_ROOT 时 Stop 钩子的路径解析问题

- **[@olgasafonova](https://github.com/olgasafonova)** - [PR #46](https://github.com/OthmanAdi/planning-with-files/pull/46)
  - 在通过规范验证运行该技能后，添加了 SkillCheck 验证徽章

- **[@AZLabsAI](https://github.com/AZLabsAI)** - [PR #65](https://github.com/OthmanAdi/planning-with-files/pull/65)
  - 更新了 OpenClaw 文档以反映产品从 Moltbot 重命名的情况，更正了所有路径和 CLI 命令

- **[@raykuo998](https://github.com/raykuo998)** - [PR #88](https://github.com/OthmanAdi/planning-with-files/pull/88)、[PR #86](https://github.com/OthmanAdi/planning-with-files/pull/86)
  - 修复了 `check-complete.ps1` 在 PowerShell 5.1 上因双引号字符串中的特殊字符解析错误而完全失败的问题；在所有 12 个平台副本中切换为单引号字符串并拼接（PR #88）
  - 修复了 Stop 钩子 YAML 多行命令块在 Windows 的 Git Bash 下失败的问题；在所有 7 个 SKILL.md 变体中将 25 行操作系统检测代码压缩为单行隐式平台回退链（PR #86）

- **[@gydx6](https://github.com/gydx6)** - [PR #79](https://github.com/OthmanAdi/planning-with-files/pull/79)
  - 修复了分布在 9 个技能副本中的会话回溯误报问题
  - 为非计划项目添加了提前返回检查
  - 提供了包含根本原因分析的详尽错误报告
  - **影响：** 消除了错误回溯报告带来的噪音

- **[@waynelee2048](https://github.com/waynelee2048)** - [PR #113](https://github.com/OthmanAdi/planning-with-files/pull/113)
  - 添加了繁体中文（zh-TW）技能变体，包含完全翻译的 SKILL.md、模板和脚本
  - 包括本地化的钩子、check-complete、init-session 和 session-catchup 脚本

- **[@tobrun](https://github.com/tobrun)** - [PR #3](https://github.com/OthmanAdi/planning-with-files/pull/3)
  - 早期的目录结构改进
  - 帮助确定了最佳仓库布局

- **[@markocupic024](https://github.com/markocupic024)** - [PR #4](https://github.com/OthmanAdi/planning-with-files/pull/4)
  - Cursor IDE 支持贡献
  - 帮助建立了多 IDE 模式

- **Copilot SWE Agent** - [PR #16](https://github.com/OthmanAdi/planning-with-files/pull/16)
  - 修复了 plugin.json 中的模板打包问题
  - 添加了 `assets` 字段，确保模板复制到缓存
  - **影响：** 解决了模板路径问题

- **[@tt-a1i](https://github.com/tt-a1i)** - [PR #92](https://github.com/OthmanAdi/planning-with-files/pull/92)、[PR #99](https://github.com/OthmanAdi/planning-with-files/pull/99)、[PR #100](https://github.com/OthmanAdi/planning-with-files/pull/100)
  - 修复了 Codex SKILL.md 中损坏的高级主题链接（PR #92）
  - 修复了跨文档的 5 个一致性问题：opencode.md 和 factory.md 中的损坏链接，在所有 16 个 IDE 副本中将过时的 `notes.md` 引用替换为 `findings.md`，在 README 中更正了 OpenCode 支持标签，`sync-ide-folders.py` 中的 `--help` 不再实际执行同步（PR #99）
  - 修复了 Codex 会话回溯静默扫描 Claude 会话路径的问题；现在当从 Codex 上下文运行时，会打印显式的回退消息（PR #100）
  - **影响：** 对整个多 IDE 表面进行了显著的文档和工具一致性检查

## 社区 Fork

这些开发者创建了扩展功能的 fork：

- **[@RioTheGreat-ai](https://github.com/RioTheGreat-ai)** - [agentfund-skill](https://github.com/RioTheGreat-ai/agentfund-skill)
  - 基于 Base 上使用里程碑式托管的 AI 代理众筹平台，使用 planning-with-files 构建

- **[@kmichels](https://github.com/kmichels)** - [multi-manus-planning](https://github.com/kmichels/multi-manus-planning)
  - 多项目支持
  - SessionStart Git 同步集成

## 问题报告者和测试者

感谢所有报告问题、提供反馈并帮助测试修复的人：

- [@nazeshinjite](https://github.com/nazeshinjite) - Issue #133（Windows Git Bash 上的 Stop 钩子可移植性故障——包含完整 Claude 输出的双根源诊断，已在 v2.34.1 中修复）
- [@msuadOf](https://github.com/msuadOf) - Issue #93（插件安装的 TMPDIR 环境修复）
- [@DorianZheng](https://github.com/DorianZheng) - Issue #84（BoxLite 沙盒集成提案）
- [@mtuwei](https://github.com/mtuwei) - Issue #32（Windows 钩子错误）
- [@JianweiWangs](https://github.com/JianweiWangs) - Issue #31（技能激活）
- [@tingles2233](https://github.com/tingles2233) - Issue #29（插件更新问题）
- [@st01cs](https://github.com/st01cs) - Issue #28（Devis fork 讨论）
- [@wqh17101](https://github.com/wqh17101) - Issue #11 测试与确认

以及许多为本项目加星、fork 和分享的人！

## 如何贡献

我们欢迎贡献！您可以通过以下方式提供帮助：

1. **报告问题** - 发现 Bug？请提交包含详细信息的 issue
2. **建议功能** - 有想法？在讨论中分享
3. **提交 PR** - 代码改进、文档、示例
4. **分享** - 向他人介绍 planning-with-files
5. **创建 Fork** - 在此工作的基础上进行构建（需注明归属）

更多详情请查看我们的[仓库](https://github.com/OthmanAdi/planning-with-files)。

## 致谢

如果您做出了贡献但在此处未看到自己的名字，请提交 issue！我们希望认可每一位帮助完善此项目的人。

---

**贡献者总数：** 36+ 且不断增长！

*最后更新：2026 年 4 月 21 日*
