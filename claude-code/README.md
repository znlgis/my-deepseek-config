# Claude Code × DeepSeek 配置

用 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 直连 DeepSeek 的
**Anthropic 兼容端点**，实现「主模型 + 轻量模型」的双档分工。

## 文件说明

| 文件            | 作用                                                            |
| --------------- | --------------------------------------------------------------- |
| `settings.json` | Claude Code 设置：DeepSeek 端点、主/轻量模型、权限基线、遥测关闭 |
| `CLAUDE.md`     | 项目级记忆（核心原则、模型分工、纪律）                           |

## 安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

## 配置步骤

1. **申请 DeepSeek API Key**：https://platform.deepseek.com 。

2. **设置认证令牌**（不要写入配置文件）：

   ```bash
   # macOS / Linux
   export ANTHROPIC_AUTH_TOKEN="sk-your-deepseek-key"
   ```

   ```powershell
   # Windows PowerShell
   $env:ANTHROPIC_AUTH_TOKEN="sk-your-deepseek-key"
   ```

   永久生效请将其写入系统环境变量或 shell 启动文件。

3. **放置配置文件**：

   - 项目级：把 `settings.json` 复制到项目的 `.claude/settings.json`，
     把 `CLAUDE.md` 复制到项目根目录。
   - 全局级：把 `settings.json` 复制到 `~/.claude/settings.json`，
     把 `CLAUDE.md` 复制到 `~/.claude/CLAUDE.md`。

4. **启动**：

   ```bash
   claude
   ```

## 关键配置（settings.json 中的 `env`）

| 变量                         | 值                                  | 说明                     |
| ---------------------------- | ----------------------------------- | ------------------------ |
| `ANTHROPIC_BASE_URL`         | `https://api.deepseek.com/anthropic`| DeepSeek 的 Anthropic 端点 |
| `ANTHROPIC_MODEL`            | `deepseek-reasoner`                 | 主模型（Pro / 强推理）   |
| `ANTHROPIC_SMALL_FAST_MODEL` | `deepseek-chat`                     | 轻量模型（Flash / 快速） |
| `ANTHROPIC_AUTH_TOKEN`       | 由环境变量注入                      | DeepSeek API Key         |

> `ANTHROPIC_AUTH_TOKEN` 刻意不写进 `settings.json`，请通过环境变量提供，避免密钥入库。

## 权限基线

- 默认 `acceptEdits`，放行读写与安全的 git / 构建 / 测试命令。
- `rm`、`git push`、`git reset`、`sudo`、`docker` 等破坏性命令需确认（`ask`）。
- `.env`、`*.key`、`*.pem`、`secrets/**` 拒绝读取（`deny`）。
