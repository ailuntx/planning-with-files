# Kilo Code 支持

通过原生集成，Kilo Code 完全支持文件规划功能。

## 快速开始

1. 在 Kilo Code 中打开您的项目
2. 技能会自动从全局目录（`~/.kilocode/skills/`）或项目目录（`.kilocode/skills/`）加载
3. 开始一项复杂任务 — Kilo Code 将自动创建规划文件

## 安装

### 快速安装（项目级）

将技能克隆或复制到项目的 `.kilocode/skills/` 目录：

**Unix/Linux/macOS：**
```bash
# 选项 A：克隆仓库
git clone https://github.com/OthmanAdi/planning-with-files.git

# 将技能复制到 Kilo Code 的技能目录
mkdir -p .kilocode/skills
cp -r planning-with-files/.kilocode/skills/planning-with-files .kilocode/skills/planning-with-files
```

**Windows (PowerShell)：**
```powershell
# 选项 A：克隆仓库
git clone https://github.com/OthmanAdi/planning-with-files.git

# 将技能复制到 Kilo Code 的技能目录
New-Item -ItemType Directory -Force -Path .kilocode\skills
Copy-Item -Recurse -Force planning-with-files\.kilocode\skills\planning-with-files .kilocode\skills\planning-with-files
```

### 手动安装（项目级）

将技能目录复制到您的项目：

**Unix/Linux/macOS：**
```bash
# 从克隆的仓库中
mkdir -p .kilocode/skills
cp -r planning-with-files/.kilocode/skills/planning-with-files .kilocode/skills/planning-with-files
```

**Windows (PowerShell)：**
```powershell
# 从克隆的仓库中
New-Item -ItemType Directory -Force -Path .kilocode\skills
Copy-Item -Recurse -Force planning-with-files\.kilocode\skills\planning-with-files .kilocode\skills\planning-with-files
```

### 全局安装（用户级）

使技能在所有项目中可用：

**Unix/Linux/macOS：**
```bash
# 复制到全局技能目录
mkdir -p ~/.kilocode/skills
cp -r planning-with-files/.kilocode/skills/planning-with-files ~/.kilocode/skills/planning-with-files
```

**Windows (PowerShell)：**
```powershell
# 复制到全局技能目录（将 YourUsername 替换为您的实际用户名）
New-Item -ItemType Directory -Force -Path C:\Users\YourUsername\.kilocode\skills
Copy-Item -Recurse -Force planning-with-files\.kilocode\skills\planning-with-files C:\Users\YourUsername\.kilocode\skills\planning-with-files
```

### 验证安装

安装后，验证技能是否已加载：

1. **重启 Kilo Code**（如果需要）
2. 询问代理："Do you have access to the planning-with-files skill?"
3. 代理应确认技能已加载

**测试 PowerShell 脚本（Windows）：**

安装后，您可以测试 PowerShell 脚本：

```powershell
# 测试 init-session.ps1
.\.kilocode\skills\planning-with-files\scripts\init-session.ps1

# 测试 check-complete.ps1
.\.kilocode\skills\planning-with-files\scripts\check-complete.ps1
```

脚本应在您的项目根目录中创建 `task_plan.md`、`findings.md` 和 `progress.md` 文件。

### 文件结构

```
~/.kilocode/skills/planning-with-files/          (全局)
或
.kilocode/skills/planning-with-files/             (项目)
├── SKILL.md              # 技能定义
├── examples.md           # 实际示例
├── reference.md          # 高级参考
├── templates/            # 规划文件模板
│   ├── task_plan.md
│   ├── findings.md
│   └── progress.md
└── scripts/              # 实用脚本
    ├── init-session.sh    # Unix/Linux/macOS
    ├── check-complete.sh  # Unix/Linux/macOS
    ├── init-session.ps1  # Windows (PowerShell)
    └── check-complete.ps1 # Windows (PowerShell)
```

**重要**：`SKILL.md` 中的 `name` 字段必须与目录名（`planning-with-files`）匹配。

## 文件位置

| 类型 | 全局位置 | 项目位置 |
|------|-----------------|------------------|
| **技能** | `~/.kilocode/skills/planning-with-files/SKILL.md` | `.kilocode/skills/planning-with-files/SKILL.md` |
| **模板** | `~/.kilocode/skills/planning-with-files/templates/` | `.kilocode/skills/planning-with-files/templates/` |
| **脚本 (Unix/Linux/macOS)** | `~/.kilocode/skills/planning-with-files/scripts/*.sh` | `.kilocode/skills/planning-with-files/scripts/*.sh` |
| **脚本 (Windows PowerShell)** | `~/.kilocode/skills/planning-with-files/scripts/*.ps1` | `.kilocode/skills/planning-with-files/scripts/*.ps1` |
| **您的文件** | 项目根目录中的 `task_plan.md`、`findings.md`、`progress.md` |

## 快速命令

**对于全局安装：**

**Unix/Linux/macOS：**
```bash
# 初始化规划文件
~/.kilocode/skills/planning-with-files/scripts/init-session.sh

# 验证任务完成情况
~/.kilocode/skills/planning-with-files/scripts/check-complete.sh
```

**Windows (PowerShell)：**
```powershell
# 初始化规划文件
$env:USERPROFILE\.kilocode\skills\planning-with-files\scripts\init-session.ps1

# 验证任务完成情况
$env:USERPROFILE\.kilocode\skills\planning-with-files\scripts\check-complete.ps1
```

**对于项目安装：**

**Unix/Linux/macOS：**
```bash
# 初始化规划文件
./.kilocode/skills/planning-with-files/scripts/init-session.sh

# 验证任务完成情况
./.kilocode/skills/planning-with-files/scripts/check-complete.sh
```

**Windows (PowerShell)：**
```powershell
# 初始化规划文件
.\.kilocode\skills\planning-with-files\scripts\init-session.ps1

# 验证任务完成情况
.\.kilocode\skills\planning-with-files\scripts\check-complete.ps1
```

## 从 Cursor/Windsurf 迁移

规划文件完全兼容。只需将您的 `task_plan.md`、`findings.md` 和 `progress.md` 文件复制到新项目即可。

## 附加资源

**对于全局安装：**
- [示例](~/.kilocode/skills/planning-with-files/examples.md) - 实际示例
- [参考](~/.kilocode/skills/planning-with-files/reference.md) - 高级参考文档
- [PowerShell 脚本](~/.kilocode/skills/planning-with-files/scripts/) - Windows 实用脚本

**对于项目安装：**
- [示例](.kilocode/skills/planning-with-files/examples.md) - 实际示例
- [参考](.kilocode/skills/planning-with-files/reference.md) - 高级参考文档
- [PowerShell 脚本](.kilocode/skills/planning-with-files/scripts/) - Windows 实用脚本
