# 更新日志

本项目所有显著变更都将记录在此文件中。

## [2.35.0] - 2026-04-21

### 新增

- **Hermes 适配器** (PR #136，作者 @bailob)：新增 `.hermes/skills/planning-with-files/` 捆绑包、`.hermes/plugins/planning-with-files/` Python 适配器、`/plan` 和 `/plan-status` 命令包装器以及 `docs/hermes.md` 安装指南。该适配器注册了三个工具（`planning_with_files_init`、`planning_with_files_status`、`planning_with_files_check_complete`）以及模仿 Claude Code 钩子行为的 `pre_llm_call` 和 `post_tool_call` 钩子。Hermes 现成为第 17 个平台。此 PR 包含 `tests/test_hermes_adapter.py` 中的 20 个单元测试，覆盖状态解析、提醒行为、安装布局及完成检查等场景。
- **NLPM 审计覆盖** (Issue #140，作者 @xiaolai)：对所有 25 个自然语言制品进行了静态审计，总分 91/100，无严重或高风险发现。三个已验证的 bug 已分别提交 PR，并在下方合并。

### 修复

- **Pi PowerShell 会话追赶语法错误** (PR #137，作者 @xiaolai，关闭 #140 部分内容)：`.pi/skills/planning-with-files/SKILL.md` 中 Windows PowerShell 调用的脚本路径前缺少了起始双引号 `"`，导致的解析错误会静默终止 Windows 上 Pi 用户的会话追赶。现已恢复引号以平衡末尾的 `"`。
- **会话追赶上下文注入现已限制** (PR #138，作者 @xiaolai，关闭 #140 部分内容)：`.github/hooks/scripts/session-start.sh` 将无限制的 `session-catchup.py` 输出通过管道传入 `additionalContext`，意味着来自先前会话的内容（网络结果、工具输出）可能无标签且无大小限制地进入当前模型上下文。现在输出经过 `head -100` 截断，并添加 `[planning-with-files] Previous session context (truncated to 100 lines):` 前缀，以便模型知晓这些内容是历史信息。
- **钩子脚本优先使用已知 Python 路径** (PR #139，作者 @xiaolai，关闭 #140 部分内容)：`session-start.sh`、`pre-tool-use.sh` 和 `error-occurred.sh` 在解析 Python 解释器时完全依赖用户的 PATH。这三个脚本现在会依次尝试 `/usr/bin/python3`、`/usr/local/bin/python3` 和 `/opt/homebrew/bin/python3`，最后才回退到 `command -v python3`，从而关闭了一个 PATH 劫持向量，同时不影响在那些规范路径下提供 Python 的系统上的行为。

### 变更

- 在全部 14 个 SKILL.md 变体、plugin.json、marketplace.json 和 CITATION.cff 中将版本号提升至 2.35.0
- 更新 `CONTRIBUTORS.md`：添加了 @bailob（PR #136，主要贡献）和 @xiaolai（PR #137、#138、#139、Issue #140）；总人数现为 36+ 人
- plugin.json 的描述现在写为 “17+ AI coding assistants”，关键词中包含 `hermes`

### 致谢

- @bailob 贡献了 Hermes 适配器、完整测试覆盖以及 `/plan` 和 `/plan-status` 命令包装器（PR #136）
- @xiaolai 进行了 NLPM 审计扫查和三项协调性的强化修复 PR（PR #137、PR #138、PR #139、Issue #140）

## [2.34.1] - 2026-04-17

### 修复

- **Stop 钩子在 Windows Git Bash 上的可移植性故障**（关闭 #133，报告者 @nazeshinjite）—— 两个独立的 bug 导致 Stop 钩子在 Command Prompt 内运行 Git Bash 的 Windows 11 上静默失败：(1) 在某些 Windows Git Bash 调用上下文中，`export SD=` 被当作外部命令而非内建命令处理，产生 `bash: export: No such file or directory` 错误；(2) 回退路径 `$HOME/.claude/plugins/planning-with-files` 从未存在——实际安装位置是 `~/.claude/plugins/cache/planning-with-files/planning-with-files/VERSION/`。已修复所有 13 个 SKILL.md 变体（Claude Code、Codex、CodeBuddy、Cursor、Factory、Mastra Code、OpenCode 及所有语言变体）。Claude Code 变体现在使用 PowerShell 自发现机制，通过 `Get-ChildItem -Recurse` 配合 `~` 主目录展开（无需 bash 变量），以及一个基于 glob 的 sh 回退路径指向正确的缓存路径。所有其他 IDE 变体已将 `export SD=` 替换为 `SD=`。

## [2.34.0] - 2026-04-15

### 新增

- **Codex 钩子已恢复**（关闭 #132）—— `.codex/hooks.json` 和 `.codex/hooks/` 下的脚本已恢复。Codex 用户现在可获得与 Claude Code、Cursor 和 Copilot 用户相同的全生命周期钩子自动化：SessionStart 运行会话追赶并注入计划上下文；UserPromptSubmit 在每一条消息上重新注入；PreToolUse 在执行 Bash 前重新读取 task_plan.md；PostToolUse 提醒智能体更新 progress.md；当各阶段尚未完成时 Stop 会阻止并重新提示。这些文件存在于 v2.31.0 版本中（PR #120，作者 @Leon-Algo），但在 v2.32.0 版本中因主分支重写而不慎被清除——现已完全恢复。
- **Codex 钩子回归测试**（`tests/test_codex_hooks.py`）—— 4 个测试用例，涵盖 hooks.json 结构、SessionStart 上下文注入、PreToolUse systemMessage 发送、PostToolUse 进度提醒以及 Stop 的阻止后允许行为。
- **Tessl 技能评审与优化 CI**（PR #131，作者 @popey）—— `.github/workflows/skill-review.yml` 对每个触及 SKILL.md 的 PR 执行，将评分和 AI 建议的改进点作为 PR 评论发布；`.github/workflows/skill-optimize-apply.yml` 允许贡献者输入 `/apply-optimize` 直接提交建议。默认为非阻塞。

### 修复

- **规范 Shell 脚本不可执行**（PR #122，作者 @Leon-Algo）—— `skills/planning-with-files/scripts/check-complete.sh` 和 `init-session.sh` 的文件模式为 `100644` 而非 `100755`，破坏了 Codex 及任何依赖可执行位的 Unix 安装程序。已修正为 `100755`，并添加了回归测试。
- **Codex SKILL.md 中重复的 `version:` 键**—— `.codex/skills/planning-with-files/SKILL.md` 的元数据块中存在两个 `version: "2.33.0"` 条目（在先前提交中已修复 zh/zht 的相同 bug，但此处遗漏）。已去重。
- **更新 Codex 文档**——重写了 `docs/codex.md`，涵盖了技能和钩子的安装、钩子协议说明、工作区安装与个人安装的区别，以及重复钩子消息和 Windows 限制的故障排除。

### 变更

- **更新 CONTRIBUTORS.md**——添加了 @Leon-Algo（PR #119、#120、#122）、@YSAA1（PR #109）、@kevinaimonster（PR #108）、@wd041216-bit（PR #107）；更新 @lasmarois 条目以包含 PR #37；总人数增至 32+ 人。

### 致谢

- @Leon-Algo 为 Codex 钩子设计、三项独立修复 PR 以及在主分支重写擦除其工作时表现出的耐心（PR #119、#120、#122）
- @popey（Alan Pope）为 Tessl CI 工作流（PR #131）

## [2.33.0] - 2026-04-09

### 新增

- **多语言扩展**—— 为国际用户新增技能变体：
  - 阿拉伯语 (`planning-with-files-ar`) - 完整的阿拉伯语本地化，支持正确的 RTL 方向
  - 德语 (`planning-with-files-de`) - 完整的德语本地化
  - 西班牙语 (`planning-with-files-es`) - 全面的西班牙语本地化
  - 增强简体中文 (`planning-with-files-zh`) - 完全本地化的脚本和模板
  - 增强繁体中文 (`planning-with-files-zht`) - 精细化的本地化
- 所有语言新增**命令文件**：`plan-ar.md`、`plan-de.md`、`plan-es.md`
- README 中添加了**国际化安装命令**，并附有各语言示例
- 插件元数据中增加**全局关键词支持**，提高可发现性

### 修复

- **简体中文脚本本地化**—— 所有脚本现在能正确显示中文消息，而非英文
- **阿拉伯语模板一致性**—— 模板和脚本现在使用一致的阿拉伯语阶段标题 (`### المرحلة`) 和状态标签 (`**الحالة:**`)
- **西班牙语模板一致性**—— 模板和脚本现在使用一致的西班牙语状态标签 (`**Estado:**`)
- **Stop 钩子路径更正**—— 所有语言变体现在在 Stop 钩子中使用正确的路径

## [2.32.0] - 2026-04-08

### 新增

- **Codex 会话追赶**（PR #124，作者 @ebrevdo）—— `session-catchup.py` 现在可从 `~/.codex/sessions` 读取 Codex 的 rollout JSONL 文件，在跳过当前线程时优先使用 `CODEX_THREAD_ID`，过滤子代理和微会话，并通过结构化的 Codex `patch_apply_end` 事件检测计划文件的更新。
- **Loaditout 安全徽章**（PR #126，关闭 #123）—— 将 A 级安全徽章添加到 README（在已扫描的 20,000+ 个 MCP 服务器中位列前 20.5%）

### 修复

- **Stop 钩子在 Windows Git Bash (MSYS2) 上失败**（PR #126，关闭 #125）
  - 根本原因：MSYS2 将裸露的 `SD="/c/Users/..."` 视为要执行的命令而非变量赋值
  - 修复方案：在所有 9 个 SKILL.md 变体（Claude Code、Codex、CodeBuddy、Cursor、Factory、Gemini、Mastra Code、OpenCode 以及 zh/zht）中将 `SD="..."` 改为 `export SD="..."`

### 变更

- 在所有 12 个 SKILL.md 文件、plugin.json、marketplace.json 和 CITATION.cff 中将版本提升至 2.32.0

### 致谢

- @ebrevdo（Eugene Brevdo）为 Codex 会话追赶重写（PR #124）

## [2.29.0] - 2026-03-24

### 新增

- **分析工作流模板**（PR #115，作者 @mvanhorn，针对 #103）
  - 在 `init-session.sh` 和 `init-session.ps1` 上新增 `--template analytics` 标志
  - `templates/analytics_task_plan.md` 包含 4 个分析特定阶段：数据发现、探索性分析、假设检验、综合
  - `templates/analytics_findings.md` 包含数据源表、假设日志、查询结果和统计发现等部分
  - 分析特定的 `progress.md` 生成查询日志表而非测试结果
  - 默认行为不变；现有用户不受影响

### 使用示例

```bash
./scripts/init-session.sh --template analytics my-project
```

### 致谢

- @mvanhorn（Matt Van Horn）实现了 @sedlukha 在 #103 中请求的分析模板

---

## [2.28.0] - 2026-03-22

### 新增

- **繁体中文 (zh-TW) 技能变体**（PR #113，作者 @waynelee2048）
  - 完全翻译的 SKILL.md、模板及脚本，位于 `skills/planning-with-files-zht/` 下
  - 本地化了钩子、check-complete、init-session 和 session-catchup 脚本

### 致谢

- @waynelee2048 贡献了繁体中文翻译

---

## [2.27.0] - 2026-03-20

### 新增

- **Kiro Agent Skill 支持**（PR #112，作者 @EListenX）
  - 完整的 `.kiro/skills/planning-with-files/` 布局，包含 SKILL.md、引导脚本、模板和参考资料
  - 引导程序创建 `.kiro/plan/` 用于存放计划文件，以及带有 `#[[file:]]` 实时引用的 `.kiro/steering/planning-context.md`
  - 包含适配 Kiro `.kiro/plan/` 路径的 session-catchup.py 和 check-complete 脚本
  - 用适当的 Agent Skill 格式替换了旧的 `.kiro/scripts/` 和 `.kiro/steering/` 方案

### 变更

- 更新 `scripts/sync-ide-folders.py`，跳过 `.kiro`（Kiro 使用自己的技能布局）
- 重写 `docs/kiro.md` 以反映新的 Agent Skill 方案

### 致谢

- @EListenX（Yi Chenxi）为遵循适当 Agent Skill 格式进行的 Kiro 集成提供了全面支持

---

## [2.23.0] - 2026-03-16

### 修复

- **执行 `/clear` 后会话追赶失效**（Issue #106，报告者 @tony-stark-eth）
  - 根本原因：会话启动时没有钩子提醒智能体已有计划文件。执行 `/clear` 后，智能体开始全新会话，完全不知晓已有的活动计划。
  - 在所有 7 个 IDE 的 SKILL.md 文件中添加了 `UserPromptSubmit` 钩子。当 `task_plan.md` 存在时，该钩子注入一条指令，要求智能体在处理之前先读取所有三个计划文件。该钩子在每一条用户消息上触发，确保智能体即使在 `/clear` 或上下文压缩后也能始终知晓活动计划。
  - 加强了 SKILL.md 中“FIRST”部分的描述：现在明确要求立即读取所有三个文件，而不仅仅是运行会话追赶。

- **进度更新不连续**（Issue #106）
  - 根本原因：`PostToolUse` 钩子消息仅提及 `task_plan.md`，从未提 `progress.md`。智能体从未被提醒记录其所作所为。
  - 在所有 7 个 IDE 的 SKILL.md 文件和两个 Copilot 钩子脚本中，将 PostToolUse 消息改为首句即为“Update progress.md with what you just did.”。
  - 添加了 `if [ -f task_plan.md ]` 守卫，使得提醒仅在存在活动计划时触发。

- **计划后增补未追踪**（Issue #106）
  - 根本原因：当所有阶段完成时，`check-complete` 脚本报告“ALL PHASES COMPLETE”，但未提供后续指导。智能体没有理由向计划添加新工作。
  - 更新 `check-complete.sh` 和 `check-complete.ps1`：完成消息现在指出“If the user has additional work, add new phases to task_plan.md before starting.”。
  - 更新 Copilot `agent-stop` 脚本，即使所有阶段已完成也输出继续上下文（之前返回空 `{}`）。
  - 向标准 SKILL.md 正文添加了关键规则 #7（“Continue After Completion”）。

### 变更

- 在所有 7 个 IDE 的 SKILL.md 文件、plugin.json 和 marketplace.json 中将版本提升至 2.23.0

### 致谢

- @tony-stark-eth 提供了涵盖三种症状的详细 bug 报告（Issue #106）

---

## [2.22.0] - 2026-03-06

### 新增

- **正式基准测试结果**—— 使用 Anthropic 的技能创建者框架对技能进行了评估
  - 10 个并行子代理，5 种不同任务类型，30 个客观可验证的断言
  - 有技能时：**96.7% 通过率**（29/30）；无技能时：6.7%（2/30）—— 差距：+90 个百分点
  - 3 次盲测 A/B 比较：有技能获胜 3/3（100%），平均分 10.0/10 vs 6.8/10
  - 完整方法参见 [docs/evals.md](docs/evals.md)
- **技术文章**—— [docs/article.md](docs/article.md)：安全分析、修复和评估方法的完整撰写
- **README 徽章**—— 基准测试（96.7% 通过率）、A/B 验证（3/3 获胜）、安全验证
- **README 基准测试结果部分**—— 关键数字一目了然

### 变更

- 将 `marketplace.json` 版本修正为跟踪当前发布（此前卡在 2.0.0）

## [2.21.0] - 2026-03-05

### 安全

- **从 `allowed-tools` 中移除 `WebFetch` 和 `WebSearch`**—— 修复 Gen Agent Trust Hub 的 FAIL 并降低 Snyk W011 风险评分
  - planning-with-files 技能是文件管理与规划技能，网络访问不属于其核心范围
  - PreToolUse 钩子在每次工具调用前都会重新读取 `task_plan.md`，当网络来源的内容被写入计划文件时会产生放大效应。从技能声明的范围中移除这些工具可以切断这一有害流程
  - 该变更已应用于声明了 `allowed-tools` 的所有 7 个 IDE 变体：Claude Code、Cursor、Kilocode、CodeBuddy、Codex、OpenCode、Mastra Code
- **向 SKILL.md 添加安全边界章节**—— 明确指示网络/搜索结果只能写入 `findings.md`（不可写入 `task_plan.md`），且所有外部内容必须被视为不可信
- **向 examples.md 添加安全说明**—— 网络研究示例现在包含一条内联注释，强调了信任边界

## [2.20.0] - 2026-03-04

### 修复

- **Codex 会话追赶静默失败**（PR #100，作者 @tt-a1i，修复 #94）
  - Codex 变体中的 `session-catchup.py` 即使在 Codex 环境下运行时也静默扫描 `~/.claude/projects`，而 Codex 的会话实际位于 `~/.codex/sessions`，且格式不同
  - 现在从 `__file__` 路径检测 Codex 运行时，并输出清晰的回退消息，而非静默空操作

- **文档中的死链**（PR #99，作者 @tt-a1i，修复 #95）
  - `docs/opencode.md` 链接到不存在的 `.opencode/INSTALL.md` —— 已更正为 `docs/installation.md`
  - `docs/factory.md` 的“另见”链接使用了 `../skills/planning-with-files/` 路径 —— 已更正为 `../.factory/skills/planning-with-files/`

- **示例使用了已过时的 `notes.md` 文件名**（PR #99，作者 @tt-a1i，修复 #96）
  - 14 个 IDE 副本的所有 `examples.md` 文件都引用了 `notes.md`，该文件名已更名为 `findings.md` —— 已在所有位置一致更新

- **`sync-ide-folders.py --help` 执行了同步而非显示用法**（PR #99，作者 @tt-a1i，修复 #98）
  - 将手动的 `sys.argv` 解析替换为 `argparse` —— `--help` 现在能正常退出并显示用法信息

### 变更

- **更正了 OpenCode README 支持标签**（PR #99，作者 @tt-a1i，修复 #97）
  - 将“完全支持”改为“部分支持”，并附带了关于会话追赶限制的说明 —— 使 README 与 `docs/opencode.md` 的实际说法一致

### 致谢

- @tt-a1i 进行了全面的一致性扫查（PR #99、PR #100）

---

## [2.19.0] - 2026-03-04

### 修复

- **Codex 高级主题部分存在死链**（PR #92，作者 @tt-a1i，修复 #91）
  - 更正了 `.codex/skills/planning-with-files/SKILL.md` 中的两个死链
  - `reference.md` → `references/reference.md`
  - `examples.md` → `references/examples.md`

### 致谢

- @tt-a1i 发现并修复了 Codex 的死链（PR #92）

---

## [2.18.3] - 2026-02-28

### 修复

- **Stop 钩子的多行 YAML 命令在 Windows Git Bash 下失败**（PR #86，作者 @raykuo998）
  - 根本原因：YAML 的 `command: |` 多行块在 Windows 的 Git Bash 中无法被可靠解析。Shell 将第一行（`SCRIPT_DIR=...`）当作命令名而非变量赋值，导致钩子在执行之前就崩溃了。
  - 将 25 行的操作系统检测脚本替换为一套单行隐式平台回退链：先尝试 `powershell.exe`，不回退 `sh`。已应用到所有带有 Stop 钩子的 7 个 SKILL.md 变体。
  - 为加快启动速度，向 PowerShell 调用添加了 `-NoProfile` 参数。

- **`check-complete.ps1` 在 PowerShell 5.1 上完全失败**（PR #88，作者 @raykuo998）
  - 根本原因：双引号内的 `Write-Host` 字符串中包含特殊字符（`[`、`(`、破折号）导致 Windows PowerShell 5.1 解析错误
  - 将双引号字符串替换为单引号字符串，并在变量插值时使用显式拼接。已应用到所有 12 个平台副本。

### 致谢

- @raykuo998 协助修复了两个 Windows 兼容性问题（PR #86、PR #88）

---

## [2.18.2] - 2026-02-26

### 修复

- **Mastra Code 的钩子完全未生效**
  - 根本原因：Mastra Code 从 `.mastracode/hooks.json` 读取钩子，而非从 SKILL.md 的前置元数据读取。已有的集成仅在 SKILL.md 中定义了钩子（Claude Code 格式），Mastra Code 会完全忽略。所有三个钩子（PreToolUse、PostToolUse、Stop）均未生效。
  - 添加了 `.mastracode/hooks.json`，采用正确的 Mastra Code 格式，包含 `matcher`、`timeout` 和 `description` 字段
  - 修正了 SKILL.md Stop 钩子中的 `MASTRACODE_SKILL_ROOT` 环境变量（该变量在 Mastra Code 中不存在，已替换为回退到本地路径的 `$HOME` 方案）
  - 将 `.mastracode/skills/planning-with-files/SKILL.md` 的元数据版本从 2.16.1 提升至 2.18.1
  - 更正了 `docs/mastra.md` 以准确描述 hooks.json（移除了声称 Mastra Code 使用与 Claude Code 相同钩子系统的错误说法）
  - 修复了个人安装说明，包含了 hooks.json 的复制步骤

---

## [2.18.1] - 2026-02-26

### 修复

- **Copilot 钩子出现乱码字符 —— v2.16.1 后仍未修复**（Issue #82，确认者 @Hexiaopi）
  - 根本原因：所有 PS1 脚本中的 `Get-Content` 未指定 `-Encoding` 参数 —— PowerShell 5.x 默认使用系统 ANSI 代码页（Windows-1252）读取文件，导致 `task_plan.md` 或 `SKILL.md` 中的任何非 ASCII 字符在到达输出管道前就已损坏。v2.16.1 的修复是正确的，但仅修复了输出侧，未修复读取侧。
  - 次要修复：`[System.Text.Encoding]::UTF8` 返回带 BOM 的 UTF-8 —— 在所有四个 PS1 脚本中将其替换为 `[System.Text.UTF8Encoding]::new($false)`（无 BOM 的 UTF-8），以防止 JSON 解析器收到多余的 `0xEF 0xBB 0xBF` 序文
  - 修复的文件：`pre-tool-use.ps1`、`session-start.ps1`、`agent-stop.ps1`、`post-tool-use.ps1`
  - Bash 脚本自 v2.16.1 起已是正确的

### 致谢

- @Hexiaopi 确认 v2.16.1 后问题仍然存在（Issue #82）

---

## [2.18.0] - 2026-02-26

### 新增

- **BoxLite 沙箱运行时集成**（Issue #84，作者 @DorianZheng）
  - 新指南 `docs/boxlite.md`，用于通过 ClaudeBox 在 BoxLite 微型虚拟机沙箱内运行 planning-with-files
  - 新示例 `examples/boxlite/quickstart.py` —— 一个使用 ClaudeBox 的 Skill API 将 planning-with-files 注入虚拟机的可用 Python 示例
  - 新文件 `examples/boxlite/README.md` —— 示例上下文和要求
  - README：新增“沙箱运行时”部分（BoxLite 属于基础设施，而非 IDE —— 与 16 平台的 IDE 表格分开）
  - README：添加了 BoxLite 徽章和文档表中的条目
  - BoxLite 通过 ClaudeBox（`pip install claudebox`）使用其 Python Skill 对象加载 —— 无需 `.boxlite/` 文件夹

### 致谢

- @DorianZheng 提出了 BoxLite 集成提案（Issue #84）

---

## [2.17.0] - 2026-02-25

### 新增

- **Mastra Code 支持** —— 新增 `.mastracode/skills/planning-with-files/` 集成，具有原生钩子（PreToolUse、PostToolUse、Stop）、完整脚本、模板和安装指南（第 16 个平台）

### 修复

- **技能元数据规范合规性** —— 将 PR #83 的修复应用到所有 12 个 IDE 特定 SKILL.md 文件：
  - `allowed-tools` 从 YAML 列表改为逗号分隔的字符串（Codex、Cursor、Kilocode、CodeBuddy、OpenCode）
  - `version` 从顶层移至 `metadata.version`，涉及所有适用文件
  - 描述中添加了触发词（如“plan out”、“break down”、“organize”、“track progress”）
  - 版本在所有位置提升至 2.16.1，包括标准 `skills/planning-with-files/SKILL.md`
  - OpenClaw 的内联 JSON 元数据扩展为适当的块状 YAML

### 致谢

- @popey 为 PR #83 规范修复，识别了相关问题

---

## [2.16.1] - 2026-02-25

### 修复

- **Copilot 钩子在 Windows 上出现乱码字符**（Issue #82，报告者 @Hexiaopi）
  - PowerShell 脚本现在在任何输出之前将 `$OutputEncoding` 和 `[Console]::OutputEncoding` 设置为 UTF-8 —— 修复了因 PowerShell 5.x 默认使用 UTF-16LE 标准输出而导致的乱码菱形字符（◆）问题
  - Bash 脚本现在使用 `json.dumps(..., ensure_ascii=False)` —— 保留了 `task_plan.md` 中的 UTF-8 字符（表情符号、重音字母、中日韩文字），而不是将其转换为原始的 `XXXX` 转义序列

### 致谢

- @Hexiaopi 报告了乱码字符问题（Issue #82）

---

## [2.16.0] - 2026-02-22

### 新增

- **GitHub Copilot 支持**（PR #80，作者 @lincolnwan）
  - 原生 GitHub Copilot 钩子集成（2026 年初的钩子功能）
  - 创建了 `.github/hooks/planning-with-files.json` 配置文件
  - 在 `.github/hooks/scripts/` 中添加了完整的钩子脚本
  - 跨平台支持（bash + PowerShell）
  - 添加了 `docs/copilot.md` 安装指南
  - 在 README 中添加了 GitHub Copilot 徽章
  - 支持的平台总数增至 15 个

### 致谢

- @lincolnwan 为 GitHub Copilot 钩子支持（PR #80）

---

## [2.14.0] - 2026-02-04

### 新增

- **Pi Agent 支持**（PR #67，作者 @ttttmr）
  - 完整的 Pi Agent (pi.dev) 集成
  - 创建了 `.pi/skills/planning-with-files/` 技能捆绑包
  - 添加了 `package.json` 以支持 NPM 安装 (`pi install npm:pi-planning-with-files`)
  - 包含完整模板、脚本和参考资料
  - 跨平台支持（macOS、Linux、Windows）
  - 添加了 `docs/pi-agent.md` 安装指南
  - 在 README 中添加了 Pi Agent 徽章
  - 注意：钩子是 Claude Code 专用的，Pi Agent 不支持

### 修复

- **Codex 技能路径引用**（PR #66，作者 @codelyc）
  - 将已损坏的 `CLAUDE_PLUGIN_ROOT` 引用替换为正确的 Codex 路径（`~/.codex/skills/planning-with-files/`）
  - 向 `.codex/skills/planning-with-files/templates/` 添加了缺失的模板文件

### 变更

- **OpenClaw 文档更新**（PR #65，作者 @AZLabsAI，修复 #64）
  - 将 `docs/moltbot.md` 重命名为 `docs/openclaw.md`
  - 将所有路径从 `~/.clawdbot/` 更新为 `~/.openclaw/`
  - 将 CLI 命令从 `moltbot` 更新为 `openclaw`
  - 将网站链接从 `molt.bot` 更新为 `openclaw.ai`
- 更新 README：将 Moltbot 徽章和引用更新为 OpenClaw
- 版本徽章更新至 v2.14.0

### 致谢

- @ttttmr 为 Pi Agent 集成（PR #67）
- @codelyc 为 Codex 路径修复（PR #66）
- @AZLabsAI 为 OpenClaw 文档更新（PR #65）

---

## [2.11.0] - 2026-01-26

### 新增

- **`/plan` 命令，便于自动补全**（Issue #39）
  - 添加了 `commands/plan.md`，创建了 `/planning-with-files:plan` 命令
  - 用户现在可以输入 `/plan` 并在自动补全中看到该命令
  - 比 `/planning-with-files:start` 更短的替代命令
  - 安装插件后立即可用 —— 无需额外设置

### 使用方式

安装插件后，你有两种命令选项：

| 命令 | 如何找到 | 支持版本 |
|---------|-------------|-------------|
| `/planning-with-files:plan` | 输入 `/plan` | v2.11.0 |
| `/planning-with-files:start` | 输入 `/planning` | v2.6.0 |

### 致谢

- @wqh17101 在讨论 #36 中一再提醒
- @dalisoft、@zoffyzhang、@yyuziyu 在 Issue #39 中提供反馈和变通方案
- 社区在寻找正确解决方案过程中表现出的耐心

---

## [2.10.0] - 2026-01-26

### 新增

- **Kiro 支持**（Issue #55，作者 @453783374）
  - 原生 Kiro 引导文件集成
  - 创建了 `.kiro/steering/`，内含规划工作流、规则和模板
  - 在 `.kiro/scripts/` 中添加了辅助脚本
  - 添加了 `docs/kiro.md` 安装指南
  - 在 README 中添加了 Kiro 徽章

### 注意

Kiro 使用**引导文件**（`.kiro/steering/*.md`）而非标准的 `SKILL.md` 格式。引导文件在每次交互中由 Kiro 自动加载。

---

## [2.9.0] - 2026-01-26

### 新增

- **Moltbot 支持**（原 Clawd CLI）
  - 为工作区和本地技能添加了 Moltbot 集成
  - 创建了 `.moltbot/skills/planning-with-files/` 技能捆绑包
  - 包含完整模板、脚本和参考资料
  - 跨平台支持（macOS、Linux、Windows）
  - 添加了 `docs/moltbot.md` 安装指南
  - 在 README 中添加了 Moltbot 徽章

### 变更

- 更新 plugin.json 描述，突出多 IDE 支持
- 添加了新关键词：moltbot、gemini、cursor、continue、multi-ide、agent-skills
- 现在支持 10+ 个 AI 编程助手

---

## [2.8.0] - 2026-01-26

### 新增

- **Continue IDE 支持**（PR #56，作者 @murphyXu）
  - 为 VS Code 和 JetBrains IDE 添加了 Continue.dev 集成
  - 创建了 `.continue/skills/planning-with-files/` 技能捆绑包
  - 创建了 `.continue/prompts/planning-with-files.prompt` 斜杠命令（中文）
  - 添加了 `docs/continue.md` 安装指南
  - 添加了 `scripts/check-continue.sh` 校验器
  - 包含完整模板、脚本和参考资料

### 修复

- **POSIX sh 兼容性**（PR #57，作者 @SaladDay）
  - 修复了在使用 dash 作为 `/bin/sh` 的 Debian/Ubuntu 系统上 Stop 钩子失败的问题
  - 将仅 bash 支持的语法（`[[`、`&>`）替换为 POSIX 兼容的构造
  - 添加了使用 `uname -s` 和 `$OS` 的不依赖具体 shell 的 Windows 检测方法
  - 修复已应用到所有 5 个 IDE 特定的 SKILL.md 文件
  - 解决了 @aqlkzf 在 #32 中报告的问题

### 致谢

- @murphyXu 为 Continue IDE 集成（PR #56）
- @SaladDay 为 POSIX sh 兼容性修复（PR #57）

---

## [2.7.1] - 2026-01-22

### 修复

- **动态 Python 命令检测**（Issue #41，作者 @wqh17101）
  - 将硬编码的 `python3` 替换为动态检测：`$(command -v python3 || command -v python)`
  - 为 Windows PowerShell 添加了直接使用 `python` 的命令
  - 已在所有 5 个 IDE 特定的 SKILL.md 文件中修复（Claude Code、Codex、Cursor、Kilocode、OpenCode）
  - 解决了 Windows/Anaconda 上仅存在 `python` 时的兼容性问题

### 致谢

- @wqh17101 报告并提出修复建议（Issue #41）

---

## [2.7.0] - 2026-01-22

### 新增

- **Gemini CLI 支持**（Issue #52）
  - 为 Google Gemini CLI v0.23+ 提供原生 Agent Skills 支持
  - 创建了 `.gemini/skills/planning-with-files/` 目录结构
  - 为 Gemini CLI 兼容性格式化了 SKILL.md
  - 包含完整模板、脚本和参考资料
  - 添加了 `docs/gemini.md` 安装指南
  - 在 README 中添加了 Gemini CLI 徽章

### 文档

- 在支持的 IDE 表格中添加了 Gemini CLI
- 更新了文件结构图
- 在文档表中添加了 Gemini CLI

### 致谢

- @airclear 请求 Gemini CLI 支持（Issue #52）

---

## [2.6.0] - 2026-01-22

### 新增

- **启动命令**（PR #51，作者 @Guozihong）
  - 新增 `/planning-with-files:start` 命令，便于激活
  - 不再需要将技能复制到 `~/.claude/skills/` 文件夹
  - 安装插件后直接可用
  - 添加了 `commands/start.md` 文件

### 修复

- **Stop 钩子路径解析**（PR #49，作者 @fahmyelraie）
  - 修复了当 `CLAUDE_PLUGIN_ROOT` 未设置时出现的“No such file or directory”错误
  - 添加了回退路径：`$HOME/.claude/plugins/planning-with-files/scripts`
  - 使 `check-complete.sh` 变为可执行（chmod +x）
  - 修复已应用到所有 IDE 特定的 SKILL.md 文件（Codex、Cursor、Kilocode、OpenCode）

### 致谢

- @fahmyelraie 为路径解析修复（PR #49）
- @Guozihong 为启动命令功能（PR #51）

---

## [2.4.0] - 2026-01-20

### 修复

- **严重：修复 SKILL.md 前置元数据以符合官方 Agent Skills 规范**（Issue #39）
  - 从 SKILL.md 前置元数据中移除无效的 `hooks:` 字段（规范不支持）
  - 移除无效的顶层 `version:` 字段（移至 `metadata.version`）
  - 移除 `user-invocable:` 字段（不在官方规范内）
  - 将 `allowed-tools:` 从 YAML 列表改为空格分隔的字符串（根据规范）
  - 这修复了 `/planning-with-files` 斜杠命令未显示的问题

### 变更

- SKILL.md 前置元数据现在遵循 [Agent Skills 规范](https://agentskills.io/specification)
- 版本现存储在 `metadata.version` 字段中
- 从 SKILL.md 中移除了 `${CLAUDE_PLUGIN_ROOT}` 变量引用（使用相对路径）
- 更新 plugin.json 至 v2.4.0

### 技术细节

之前的 SKILL.md 使用了非标准的前置元数据字段：
```yaml
# 旧（有问题）
version: "2.3.0"           # 顶层不支持
user-invocable: true       # 不在官方规范内
hooks:                     # SKILL.md 中不支持
  PreToolUse: ...
```

现在使用符合规范的格式：
```yaml
# 新（已修复）
name: planning-with-files
description: ...
license: MIT
metadata:
  version: "2.4.0"
  author: OthmanAdi
allowed-tools: Read Write Edit Bash Glob Grep WebFetch WebSearch
```

### 致谢

- @wqh17101 在 #39 中识别了该问题
- @dalisoft 和 @zoffyzhang 报告了该问题

## [2.3.0] - 2026-01-17

### 新增

- **Codex IDE 支持**
  - 创建了 `.codex/INSTALL.md` 安装说明
  - 技能安装至 `~/.codex/skills/planning-with-files/`
  - 可与 obra/superpowers 配合使用或独立使用
  - 添加了 `docs/codex.md` 用户文档
  - 基于对 obra/superpowers Codex 实现的分析

- **OpenCode IDE 支持**（Issue #27）
  - 创建了 `.opencode/INSTALL.md` 安装说明
  - 全局安装：`~/.config/opencode/skills/planning-with-files/`
  - 项目安装：`.opencode/skills/planning-with-files/`
  - 可与 obra/superpowers 插件配合使用或独立使用
  - 记录了 oh-my-opencode 兼容性
  - 添加了 `docs/opencode.md` 用户文档
  - 基于对 obra/superpowers OpenCode 插件的分析

### 变更

- 更新了 README.md 中的“支持的 IDE”表格
- 更新了 README.md 的文件结构图
- 更新了 docs/installation.md，增加了 Codex 和 OpenCode 部分
- 版本提升至 2.3.0

### 文档

- 在 README 的 IDE 支持表格中添加了 Codex 和 OpenCode
- 为两个 IDE 创建了全面的安装指南
- 记录了 OpenCode 的技能优先级系统
- 记录了与 superpowers 生态系统的集成

### 调研

本实现基于对以下内容的实际分析：
- [obra/superpowers](https://github.com/obra/superpowers) 仓库
- Codex 技能系统和 CLI 架构
- OpenCode 插件系统和技能解析
- 技能优先级和覆盖机制

### 致谢

- @Realtyxxx 针对 Issue #27 提供了关于 OpenCode 支持的反馈
- obra 为 superpowers 参考实现

---

## [2.2.2] - 2026-01-17

### 修复

- **恢复技能激活语言**（PR #34）
  - 恢复了 SKILL.md 描述中的激活触发器
  - 描述现在包含：“Use when starting complex multi-step tasks, research projects, or any task requiring >5 tool calls”
  - 这些语言在 v2.2.1 合并过程中被意外移除
  - 有助于 Claude 在检测到合适任务时自动激活该技能

### 变更

- 在所有 SKILL.md 文件和 plugin.json 中将版本更新至 2.2.2

### 致谢

- 社区成员发现了此问题

---

## [2.2.1] - 2026-01-17

### 新增

- **会话恢复功能**（PR #33，作者 @lasmarois）
  - 自动检测并恢复 `/clear` 后来自先前会话的未同步工作
  - 新增 `scripts/session-catchup.py`，分析先前会话的 JSONL 文件
  - 查找最后一次计划文件的更新，并提取之后发生的对话
  - 当调用 `/planning-with-files` 时自动触发恢复
  - 纯 Python 标准库实现，无外部依赖

- **PreToolUse 钩子增强**
  - 现在除了 Write/Edit/Bash 外，在 Read/Glob/Grep 时也触发
  - 在调研/探索阶段保持 task_plan.md 始终在关注范围内
  - 更好的跨工作流上下文管理

### 变更

- 重构 SKILL.md，将会话恢复作为第一条指令
- 更新描述，提及会话恢复功能
- 更新 README，增加会话恢复工作流和说明

### 文档

- 在 README 中新增“会话恢复”一节
- 记录了上下文窗口管理的最佳工作流
- 提供了在 Claude Code 设置中禁用自动压缩的说明

### 致谢

特别感谢：
- @lasmarois 为会话恢复实现（PR #33）
- 社区成员进行测试和反馈

---

## [2.2.0] - 2026-01-17

### 新增

- **Kilo Code 支持**（PR #30，作者 @aimasteracc）
  - 为 planning-with-files 技能添加了 Kilo Code IDE 兼容性
  - 创建了 `.kilocode/rules/planning-with-files.md`，包含 IDE 特定规则
  - 添加了 `docs/kilocode.md`，为 Kilo Code 用户提供全面文档
  - 实现与 Kilo Code 规划工作流的无缝集成

- **Windows PowerShell 支持**（修复 #32、#25）
  - 创建了 `check-complete.ps1` - PowerShell 等效脚本
  - 创建了 `init-session.ps1` - PowerShell 会话初始化
  - 脚本在三个位置均可用（仓库根目录、插件目录、技能目录）
  - 带自动回退的操作系统感知钩子执行
  - 通过原生 PowerShell 支持改善 Windows 用户体验

- **CONTRIBUTORS.md**
  - 认可所有社区贡献者
  - 列出代码贡献者及其影响
  - 感谢问题报告者和测试者
  - 记录社区分支

### 修复

- **Stop 钩子 Windows 兼容性**（修复 #32）
  - 钩子现在自动检测 Windows 环境
  - 在 Windows 上使用 PowerShell 脚本，在 Unix/Linux/Mac 上使用 bash
  - 若 PowerShell 不可用，则优雅回退
  - 已在 Windows 11 PowerShell 和 Git Bash 上测试

- **脚本路径解析**（修复 #25）
  - 改进 `${CLAUDE_PLUGIN_ROOT}` 在各平台上的处理
  - 无论安装方式如何，脚本现在均可正常工作
  - 为缺失脚本添加了错误处理

### 变更

- **SKILL.md 钩子配置**
  - Stop 钩子现使用带操作系统检测的多行命令
  - 支持 pwsh（PowerShell Core）、powershell（Windows PowerShell）和 bash
  - 自动回退链以实现最大兼容性

- **文档更新**
  - 更新以支持 Claude Code 和 Kilo Code 环境
  - 增强模板在不同 AI 编程助手间的兼容性
  - 更新 `.gitignore` 以包含 `findings.md` 和 `progress.md`

### 新增文件

- `.kilocode/rules/planning-with-files.md` - Kilo Code IDE 规则
- `docs/kilocode.md` - Kilo Code 特定文档
- `scripts/check-complete.ps1` - PowerShell 完成检查（根级别）
- `scripts/init-session.ps1` - PowerShell 会话初始化（根级别）
- `planning-with-files/scripts/check-complete.ps1` - PowerShell（插件级别）
- `planning-with-files/scripts/init-session.ps1` - PowerShell（插件级别）
- `skills/planning-with-files/scripts/check-complete.ps1` - PowerShell（技能级别）
- `skills/planning-with-files/scripts/init-session.ps1` - PowerShell（技能级别）
- `CONTRIBUTORS.md` - 社区贡献者认可
- `COMPREHENSIVE_ISSUE_ANALYSIS.md` - 详细问题研究与解决方案

### 文档

- 添加了 Windows 故障排除指南
- 在 CONTRIBUTORS.md 中认可了社区贡献者
- 更新 README 以反映 Windows 和 Kilo Code 支持

### 致谢

特别感谢：
- @aimasteracc 为 Kilo Code 支持和 PowerShell 脚本贡献（PR #30）
- @mtuwei 报告 Windows 兼容性问题（#32）
- 所有进行测试和提供反馈的社区成员

  - 根本原因：`${CLAUDE_PLUGIN_ROOT}` 解析为仓库根目录，但模板仅在子文件夹中
  - 在仓库根级别添加了 `templates/` 和 `scripts/` 目录
  - 现在无论 `CLAUDE_PLUGIN_ROOT` 如何解析，均可访问模板
  - 适用于插件安装和手动安装

### 结构

在此修复之后，模板存在于三个位置以实现最大兼容：
- `templates/` - 仓库根目录（用于 `${CLAUDE_PLUGIN_ROOT}/templates/`）
- `planning-with-files/templates/` - 用于插件市场安装
- `skills/planning-with-files/templates/` - 用于传统的 `~/.claude/skills/` 安装

### 现有用户的变通方案

如果更新后仍遇到问题：
1. 卸载：`/plugin uninstall planning-with-files@planning-with-files`
2. 重新安装：`/plugin marketplace add OthmanAdi/planning-with-files`
3. 安装：`/plugin install planning-with-files@planning-with-files`

---

## [2.1.1] - 2026-01-10

### 修复

- **插件模板路径问题**（修复 #15）
  - 通过插件市场安装时找不到模板
  - 插件缓存期望仓库根目录存在 `planning-with-files/templates/`
  - 在根级别添加了 `planning-with-files/` 文件夹用于插件安装
  - 保留 `skills/planning-with-files/` 用于传统的 `~/.claude/skills/` 安装

### 结构

- `planning-with-files/` - 用于插件市场安装
- `skills/planning-with-files/` - 用于手动 `~/.claude/skills/` 安装

---

## [2.1.0] - 2026-01-10

### 新增

- **Claude Code v2.1 兼容性**
  - 更新技能以利用所有新的 Claude Code v2.1 功能
  - 需要 Claude Code v2.1.0 或更高版本

- **`user-invocable: true` 前置元数据**
  - 技能现在出现在斜杠命令菜单中
  - 用户可通过 `/planning-with-files` 手动调用
  - 自动检测仍然工作如常

- **`SessionStart` 钩子**
  - 在技能加载并准备就绪时通知用户
  - 会话开始时显示消息，确认技能可用

- **`PostToolUse` 钩子**
  - 在每次 Write/Edit 操作后运行
  - 提醒 Claude 如果某个阶段已完成，应更新 `task_plan.md`
  - 有助于防止状态更新被遗忘

- **`allowed-tools` 的 YAML 列表格式**
  - 从逗号分隔的字符串迁移至 YAML 列表语法
  - 更清晰、更易维护的前置元数据
  - 遵循 Claude Code v2.1 最佳实践

### 变更

- 在 SKILL.md、plugin.json 和 README.md 中将版本提升至 2.1.0
- 更新 README.md，添加 v2.1.0 功能部分
- 更新版本表以反映新版本

### 兼容性

- **最低 Claude Code 版本：** v2.1.0
- **向后兼容：** 是（仍可在旧版 Claude Code 中运行，但新钩子可能不会触发）

## [2.0.1] - 2026-01-09

### 修复

- 计划文件现在正确创建在项目目录中，而非技能安装文件夹
- 在 SKILL.md 中添加了“Important: Where Files Go”部分
- 在 README.md 中添加了“故障排除”部分

### 致谢

- @wqh17101 报告并确认了修复

## [2.0.0] - 2026-01-08

### 新增

- **钩子集成**（Claude Code 2.1.0+）
  - `PreToolUse` 钩子：在 Write/Edit/Bash 操作前自动读取 `task_plan.md`
  - `Stop` 钩子：在停止前验证所有阶段是否已完成
  - 自动实现 Manus “注意力操控”原则

- **模板目录**
  - `templates/task_plan.md` - 结构化的阶段跟踪模板
  - `templates/findings.md` - 研究与发现存储模板
  - `templates/progress.md` - 包含测试结果的会话日志模板

- **脚本目录**
  - `scripts/init-session.sh` - 一次性初始化所有计划文件
  - `scripts/check-complete.sh` - 验证所有阶段是否已完成

- **新文档**
  - `CHANGELOG.md` - 本文件

- **增强的 SKILL.md**
  - 2-Action Rule（每 2 次视图/浏览器操作后保存发现结果）
  - 3-Strike 错误协议（结构化的错误恢复）
  - 读取与写入决策矩阵
  - 5 问题重启测试

- **扩展的 reference.md**
  - 3 种上下文工程策略（缩减、隔离、卸载）
  - 7 步 Agent 循环图
  - 关键约束部分
  - 更新了 Manus 统计数据

### 变更

- 重构 SKILL.md，实现渐进式披露（<500 行）
- 在所有清单中将版本提升至 2.0.0
- 重新组织 README.md（致谢部分移至顶部）
- 更新描述，提及 >5 次工具调用阈值

### 保留

- 所有 v1.0.0 内容均可在 `legacy` 分支中获取
- 保留原始的 examples.md（经过验证的模式）
- 核心的 3 文件模式未变
- MIT 许可证未变

## [1.0.0] - 2026-01-07

### 新增

- 初始版本
- 包含核心工作流的 SKILL.md
- 包含 6 条 Manus 原则的 reference.md
- 包含 4 个真实世界示例的 examples.md
- 面向 Claude Code 市场的插件结构
- 包含安装说明的 README.md

---

## 版本控制

本项目遵循 [语义化版本控制](https://semver.org/)：
- 主版本号：对技能行为的重大更改
- 次版本号：向后兼容的新功能
- 修订号：错误修复、文档更新
