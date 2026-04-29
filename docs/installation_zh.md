# 安装指南

planning-with-files 的完整安装说明。

## 快速安装（推荐）

```bash
/plugin marketplace add OthmanAdi/planning-with-files
/plugin install planning-with-files@planning-with-files
```

完成！技能现已激活。

---

## 安装方法

### 1. Claude Code 插件（推荐）

使用 Claude Code CLI 直接安装：

```bash
/plugin marketplace add OthmanAdi/planning-with-files
/plugin install planning-with-files@planning-with-files
```

**优势：**
- 自动更新
- 正确的钩子集成
- 完整功能支持

---

### 2. 手动安装

将此仓库克隆或复制到项目的 `.claude/plugins/` 目录：

#### 选项 A：克隆到插件目录

```bash
mkdir -p .claude/plugins
git clone https://github.com/OthmanAdi/planning-with-files.git .claude/plugins/planning-with-files
```

#### 选项 B：添加为 git 子模块

```bash
git submodule add https://github.com/OthmanAdi/planning-with-files.git .claude/plugins/planning-with-files
```

#### 选项 C：使用 --plugin-dir 标志

```bash
git clone https://github.com/OthmanAdi/planning-with-files.git
claude --plugin-dir ./planning-with-files
```

---

### 3. 传统安装（仅技能）

如果只需要技能而不需要完整的插件结构：

```bash
git clone https://github.com/OthmanAdi/planning-with-files.git
cp -r planning-with-files/skills/* ~/.claude/skills/
```

---

### 4. 单行安装器（仅技能）

直接将技能提取到当前目录：

```bash
curl -L https://github.com/OthmanAdi/planning-with-files/archive/master.tar.gz | tar -xzv --strip-components=2 "planning-with-files-master/skills/planning-with-files"
```

然后将 `planning-with-files/` 移动到 `~/.claude/skills/`。

---

## 验证安装

安装后，验证技能是否已加载：

1. 启动新的 Claude Code 会话
2. 您应该看到：`[planning-with-files] 就绪。复杂任务自动激活，或使用 /planning-with-files 手动调用`
3. 或输入 `/planning-with-files` 手动调用

---

## 更新

### 插件安装

```bash
/plugin update planning-with-files@planning-with-files
```

### 手动安装

```bash
cd .claude/plugins/planning-with-files
git pull origin master
```

### 仅技能

```bash
cd ~/.claude/skills/planning-with-files
git pull origin master
```

---

## 卸载

### 插件

```bash
/plugin uninstall planning-with-files@planning-with-files
```

### 手动安装

```bash
rm -rf .claude/plugins/planning-with-files
```

### 仅技能

```bash
rm -rf ~/.claude/skills/planning-with-files
```

---

## 要求

- **Claude Code：** v2.1.0 或更高版本（完整钩子支持）
- **旧版本：** 核心功能可用，但钩子可能不会触发

---

## 平台特定说明

### Windows

查看 [docs/windows.md](windows.md) 获取 Windows 特定安装说明。

### Cursor

查看 [docs/cursor.md](cursor.md) 获取 Cursor IDE 安装说明。

### Codex

查看 [docs/codex.md](codex.md) 获取 Codex IDE 安装说明。

### OpenCode

查看 [docs/opencode.md](opencode.md) 获取 OpenCode IDE 安装说明。

---

## 需要帮助？

如果安装失败，请查看 [docs/troubleshooting.md](troubleshooting.md) 或在 [github.com/OthmanAdi/planning-with-files/issues](https://github.com/OthmanAdi/planning-with-files/issues) 提交问题。
