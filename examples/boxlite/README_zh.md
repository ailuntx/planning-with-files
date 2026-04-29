# BoxLite 示例

在 [BoxLite](https://boxlite.ai) 微虚拟机沙箱中通过 [ClaudeBox](https://github.com/boxlite-ai/claudebox) 运行 planning-with-files 的工作示例。

## 文件

| 文件 | 描述 |
|------|-------------|
| `quickstart.py` | 完整的 Python 示例 — 将 planning-with-files 作为 ClaudeBox 技能加载，在 BoxLite 虚拟机内运行规划会话 |

## 环境要求

```bash
pip install claudebox
export CLAUDE_CODE_OAUTH_TOKEN=sk-ant-oat01-...
```

## 运行

```bash
python quickstart.py
```

## 文档

完整集成指南请参阅 [docs/boxlite.md](../../docs/boxlite.md)。
