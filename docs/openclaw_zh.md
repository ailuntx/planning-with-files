# OpenClaw 设置

如何将 planning-with-files 与 [OpenClaw](https://openclaw.ai) 结合使用。

---

## 此集成添加了什么

- 工作区技能：项目根目录下的 `skills/planning-with-files/`
- 完整的模板、脚本和参考文档
- 跨平台支持（macOS、Linux、Windows）

OpenClaw 支持三个技能位置（优先级顺序）：
1. **工作区技能**（最高优先级）：`<workspace>/skills/`
2. **托管/本地技能**：`~/.openclaw/skills/`
3. **内置技能**（最低优先级）：随安装一起提供

---

## 安装（通过 ClawHub — 推荐）

直接从 ClawHub 市场安装：

```bash
claw install othmanadi/planning-with-files
```

或从 [clawhub.ai/othmanadi/planning-with-files](https://clawhub.ai/othmanadi/planning-with-files) 下载 zip 文件并解压到工作区的 `skills/` 文件夹。

---

## 安装（工作区，手动）

从 GitHub 仓库复制到您的项目：

```bash
# 克隆仓库
git clone https://github.com/OthmanAdi/planning-with-files.git

# 将技能文件复制到工作区
mkdir -p skills/planning-with-files
cp -r planning-with-files/clawhub-upload/* skills/planning-with-files/

# 清理
rm -rf planning-with-files
```

---

## 安装（全局）

安装到本地 OpenClaw 技能目录：

```bash
# 克隆仓库
git clone https://github.com/OthmanAdi/planning-with-files.git

# 复制到全局 OpenClaw 技能
mkdir -p ~/.openclaw/skills/planning-with-files
cp -r planning-with-files/clawhub-upload/* ~/.openclaw/skills/planning-with-files/

# 清理
rm -rf planning-with-files
```

---

## 验证安装

```bash
# 查看 OpenClaw 状态和已加载的技能
openclaw status
```

---

## 使用

1. 在项目目录中启动 OpenClaw 会话
2. 对于复杂任务，技能将引导您创建：
   - `task_plan.md` — 阶段跟踪和决策
   - `findings.md` — 研究和发现
   - `progress.md` — 会话日志和测试结果
3. 遵循工作流：先规划，每个阶段后更新

---

## 辅助脚本

从项目根目录：

```bash
# 初始化所有规划文件
bash skills/planning-with-files/scripts/init-session.sh

# 或在 Windows PowerShell 上
powershell -ExecutionPolicy Bypass -File skills/planning-with-files/scripts/init-session.ps1

# 验证所有阶段是否完成
bash skills/planning-with-files/scripts/check-complete.sh
```

---

## 配置（可选）

在 `~/.openclaw/openclaw.json` 中配置技能：

```json5
{
  skills: {
    entries: {
      "planning-with-files": {
        enabled: true
      }
    }
  }
}
```

---

## 注意事项

- OpenClaw 在会话开始时会对符合条件的技能进行快照
- 工作区技能优先于内置技能
- 该技能适用于所有平台：macOS、Linux 和 Windows
- 规划文件与工具无关，可在 Claude Code、Cursor 和其他 IDE 中使用
