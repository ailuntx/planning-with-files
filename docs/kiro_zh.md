# Kiro

在 [Kiro](https://kiro.dev) 中使用 **planning-with-files**：**Agent Skills**、可选的 **Steering**（由 bootstrap 创建），以及位于 `.kiro/plan/` 下的磁盘 Markdown 文件。

官方参考：

- [Agent Skills](https://kiro.dev/docs/skills/)
- [Steering](https://kiro.dev/docs/steering/)（包含模式、`#[[file:path]]` 实时文件引用）

---

## 本仓库包含的内容

仅包含工作区技能文件夹：

```
.kiro/skills/planning-with-files/
├── SKILL.md
├── references/          # manus-principles, planning-rules, planning-templates
└── assets/
    ├── scripts/         # bootstrap, session-catchup, check-complete (.sh + .ps1 + .py)
    └── templates/       # task_plan, findings, progress, planning-context (steering)
```

运行 **bootstrap**（从项目根目录）会创建：

| 路径 | 作用 |
|------|------|
| `.kiro/plan/task_plan.md` | 目标、阶段、决策、错误 |
| `.kiro/plan/findings.md` | 研究和技术决策 |
| `.kiro/plan/progress.md` | 会话日志 |
| `.kiro/steering/planning-context.md` | `inclusion: auto` + `#[[file:.kiro/plan/…]]` |

设计说明：**默认不安装钩子。** 钩子是工作区范围的。此集成使用技能、生成的 steering 以及 `SKILL.md` 中的 `[Planning Active]` 提醒。

---

## 安装到你的项目

```bash
git clone https://github.com/OthmanAdi/planning-with-files.git
mkdir -p .kiro/skills
cp -r planning-with-files/.kiro/skills/planning-with-files ./.kiro/skills/
```

然后从你的**项目根目录**运行：

```bash
sh .kiro/skills/planning-with-files/assets/scripts/bootstrap.sh
```

Windows (PowerShell)：

```powershell
pwsh -ExecutionPolicy Bypass -File .kiro/skills/planning-with-files/assets/scripts/bootstrap.ps1
```

---

## 在 Kiro 中导入技能

1. 在 Kiro 面板中打开 **Agent Steering & Skills**。  
2. **Import a skill** → local folder → `.kiro/skills/planning-with-files`  
3. 或者将该文件夹复制到 `~/.kiro/skills/planning-with-files` 以作为**全局**技能（[作用域](https://kiro.dev/docs/skills/#skill-scope)）。

---

## 脚本（位于技能下）

所有路径均相对于**项目根目录**，前提是你已将 `.kiro/skills/planning-with-files/` 复制到项目中。

| 脚本 | 用途 |
|--------|---------|
| `assets/scripts/bootstrap.sh` / `bootstrap.ps1` | 创建 `.kiro/plan/*` 和 `planning-context.md`（幂等操作） |
| `assets/scripts/session-catchup.py` | 打印规划文件的修改时间 + 简短摘要 |
| `assets/scripts/check-complete.sh` / `check-complete.ps1` | 报告阶段完成情况与 `.kiro/plan/task_plan.md` 的对比 |

示例：

```bash
sh .kiro/skills/planning-with-files/assets/scripts/check-complete.sh
$(command -v python3 || command -v python) \
  .kiro/skills/planning-with-files/assets/scripts/session-catchup.py "$(pwd)"
```

```powershell
pwsh -File .kiro/skills/planning-with-files/assets/scripts/check-complete.ps1
python .kiro/skills/planning-with-files/assets/scripts/session-catchup.py (Get-Location)
```

---

## Manus 风格的上下文工程

技能描述和 [references/manus-principles.md](../.kiro/skills/planning-with-files/references/manus-principles.md) 记录了在 Manus 风格智能体上下文工程中讨论的**文件系统即内存**模式。

---

## 模板参考

查看 [references/planning-templates.md](../.kiro/skills/planning-with-files/references/planning-templates.md) 获取紧凑、便于粘贴的骨架模板。要在 Kiro 中将其用作**手动 steering**，请将该文件复制到 `.kiro/steering/` 并添加该文件顶部描述的 YAML 前置元数据。
