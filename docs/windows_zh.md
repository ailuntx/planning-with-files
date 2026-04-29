# Windows 设置

Windows 系统特有的安装与使用说明。

---

## Windows 安装指南

### 通过 winget 安装（推荐）

Claude Code 支持 Windows 包管理器：

```powershell
winget install Anthropic.ClaudeCode
```

然后安装技能：

```
/plugin marketplace add OthmanAdi/planning-with-files
/plugin install planning-with-files@planning-with-files
```

### 手动安装

```powershell
# 创建插件目录
mkdir -p $env:USERPROFILE\.claude\plugins

# 克隆仓库
git clone https://github.com/OthmanAdi/planning-with-files.git $env:USERPROFILE\.claude\plugins\planning-with-files
```

### 仅安装技能

```powershell
git clone https://github.com/OthmanAdi/planning-with-files.git
Copy-Item -Recurse planning-with-files\skills\* $env:USERPROFILE\.claude\skills\
```

---

## 路径差异

| Unix/macOS | Windows |
|------------|---------|
| `~/.claude/skills/` | `%USERPROFILE%\.claude\skills\` |
| `~/.claude/plugins/` | `%USERPROFILE%\.claude\plugins\` |
| `.claude/plugins/` | `.claude\plugins\` |

---

## Shell 脚本兼容性

辅助脚本（`init-session.sh`、`check-complete.sh`）是 bash 脚本。

### 选项 1：使用 Git Bash

如果已安装 Git for Windows，请在 Git Bash 中运行脚本：

```bash
./scripts/init-session.sh
```

### 选项 2：使用 WSL

```bash
wsl ./scripts/init-session.sh
```

### 选项 3：手动替代方案

不运行脚本，手动创建文件：

```powershell
# 将模板复制到当前目录
Copy-Item templates\task_plan.md .
Copy-Item templates\findings.md .
Copy-Item templates\progress.md .
```

---

## 钩子命令

钩子使用 Unix 风格命令。在 Windows 上的 Claude Code 中：

- 钩子在 Unix 兼容的 shell 环境中运行
- `cat`、`head`、`echo` 等命令可自动工作
- 无需修改技能配置

---

## 常见 Windows 问题

### 路径分隔符

如果出现路径错误，请确保使用正确的分隔符：

```powershell
# Windows 格式
$env:USERPROFILE\.claude\skills\

# 不要使用 Unix 格式
~/.claude/skills/
```

### 行尾符

如果模板显示异常，请检查行尾符：

```powershell
# 如需转换为 Windows 行尾符
(Get-Content template.md) | Set-Content -Encoding UTF8 template.md
```

### 权限错误

如果遇到权限错误，请以管理员身份运行 PowerShell：

```powershell
# 右键点击 PowerShell → 以管理员身份运行
```

---

## 终端推荐

为获得最佳 Windows 使用体验：

1. **Windows Terminal** - 现代终端，支持良好的 Unicode
2. **Git Bash** - Windows 上的类 Unix 环境
3. **WSL** - 完整的 Linux 环境

---

## 需要帮助？

请在 [github.com/OthmanAdi/planning-with-files/issues](https://github.com/OthmanAdi/planning-with-files/issues) 提交问题。
