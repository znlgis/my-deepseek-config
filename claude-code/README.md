# Claude Code × DeepSeek 配置

用 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 直连 DeepSeek 的
**Anthropic 兼容端点**，实现「主模型 + 轻量模型」的双档分工。

## 文件说明

| 文件            | 作用                                                            |
| --------------- | --------------------------------------------------------------- |
| `.claude/settings.json` | Claude Code 设置：DeepSeek 端点、主/轻量模型、权限基线、遥测关闭 |
| `.claude/rules/`| 分层规则（代码风格、反模式）                                    |
| `.claude/skills/` | 4 个可复用 Skill（SKILL.md）                                   |
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

   - 项目级：把 `.claude/` 目录复制到项目根（已内含 `settings.json`、`rules/`、`skills/`），
     把 `CLAUDE.md` 复制到项目根目录。
   - 全局级：把 `.claude/` 复制到 `~/.claude/`，
     把 `CLAUDE.md` 复制到 `~/.claude/CLAUDE.md`。

4. **启动**：

   ```bash
   claude
   ```

## 关键配置（settings.json 中的 `env`）

| 变量                            | 值                                  | 说明                       |
| ------------------------------- | ----------------------------------- | -------------------------- |
| `ANTHROPIC_BASE_URL`            | `https://api.deepseek.com/anthropic`| DeepSeek 的 Anthropic 端点 |
| `ANTHROPIC_MODEL`               | `deepseek-v4-pro`                   | 主模型（Pro / 强推理）     |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | `deepseek-v4-flash`                 | 轻量模型（Flash / 快速）   |
| `ANTHROPIC_AUTH_TOKEN`          | 由环境变量注入                      | DeepSeek API Key           |

> 轻量模型也一并设置了旧变量 `ANTHROPIC_SMALL_FAST_MODEL`（已废弃，仅为兼容旧版本），
> 新版本请以 `ANTHROPIC_DEFAULT_HAIKU_MODEL` 为准。
> `ANTHROPIC_AUTH_TOKEN` 刻意不写进 `settings.json`，请通过环境变量提供，避免密钥入库。

## 权限基线

- 默认 `acceptEdits`，放行读写与安全的 git / 构建 / 测试命令。
- `rm`、`git push`、`git reset`、`sudo`、`docker` 等破坏性命令需确认（`ask`）。
- `.env`、`*.key`、`*.pem`、`secrets/**` 拒绝读取（`deny`）。
