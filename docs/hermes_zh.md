# Hermes 设置

本仓库提供了一个用于 planning-with-files 的 Hermes 适配器。

适配器包含两个部分：

- `.hermes/skills/planning-with-files/` 包含面向 Hermes 的工作流技能及其捆绑的模板和脚本
- `.hermes/plugins/planning-with-files/` 包含提供规划工具和上下文注入的项目插件

## 适配器提供的功能

- `planning_with_files_init` 在目标项目中创建 `task_plan.md`、`findings.md` 和 `progress.md`
- `planning_with_files_status` 总结当前的规划状态
- `planning_with_files_check_complete` 运行完成检查辅助工具
- 项目插件在后续轮次中注入活跃的规划上下文，并提醒代理在写操作后更新规划文件

## 安装

### 1. 启用项目插件

```bash
export HERMES_ENABLE_PROJECT_PLUGINS=1
```

### 2. 安装 Hermes 技能包

将技能目录添加到您的 Hermes 配置文件中。技能包包含 `SKILL.md`、`templates/` 和 `scripts/`。

```yaml
skills:
  external_dirs:
    - /absolute/path/to/planning-with-files/.hermes/skills
```

### 3. 安装 Hermes 项目插件

将 `.hermes/plugins/planning-with-files/` 复制到目标配置文件或仓库中，以便 Hermes 能够加载 Python 适配器。

### 4. 从目标项目目录启动 Hermes

项目插件位于 `.hermes/plugins/planning-with-files/`。当项目插件启用且工作目录为此仓库时，Hermes 会自动发现它。

## 用法

- 运行 `/plan` 在当前项目中启动规划工作流
- 运行 `/plan-status` 查看当前规划状态
- 直接加载 `planning-with-files` 以获取工作流指令，而不使用命令包装器

## 验证

```bash
python3 -m unittest tests/test_hermes_adapter.py
```
