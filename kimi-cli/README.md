# Kimi CLI × DeepSeek 配置

用 [Kimi CLI](https://github.com/MoonshotAI/kimi-cli)（正演进为 Kimi Code CLI）通过
`openai_legacy` provider 接入 DeepSeek 的 OpenAI 兼容端点，并配置 Pro / Flash 双模型。

## 文件说明

| 文件          | 作用                                                   |
| ------------- | ------------------------------------------------------ |
| `config.toml` | Kimi CLI 配置：DeepSeek provider + `deepseek-chat` / `deepseek-reasoner` 两个模型 |
| `AGENTS.md`   | 项目级记忆（核心原则、模型分工、纪律）                 |

## 安装

```bash
# Linux / macOS
curl -LsSf https://code.kimi.com/install.sh | bash
# 或： uv tool install --python 3.13 kimi-cli
```

## 配置步骤

1. **申请 DeepSeek API Key**：https://platform.deepseek.com 。

2. **放置配置**：把 `config.toml` 复制到 `~/.kimi/config.toml`，把 `api_key` 替换为真实 Key；
   把 `AGENTS.md` 复制到项目根目录（或在项目内运行 `/init` 生成后并入本内容）。

   > ⚠️ `config.toml` 会存放 `api_key`。若纳入版本管理，请改用占位符或忽略该文件，切勿提交真实 Key。

3. **启动并选择模型**：

   ```bash
   cd your-project
   kimi
   ```

   进入后用 `/model` 选择 `deepseek-chat`（Flash）或 `deepseek-reasoner`（Pro，思考模式）。

## 关键配置（config.toml）

| 键                                | 值                            | 说明                         |
| --------------------------------- | ----------------------------- | ---------------------------- |
| `providers.deepseek.type`         | `openai_legacy`               | OpenAI Chat Completions 协议 |
| `providers.deepseek.base_url`     | `https://api.deepseek.com/v1` | DeepSeek 端点                |
| `models.deepseek-chat`            | —                             | Flash（快速，默认）          |
| `models.deepseek-reasoner`        | `capabilities=["always_thinking"]` | Pro（强推理，思考）    |

> 待 DeepSeek V4（pro/flash）发布后，替换 `[models.*]` 里的 `model` ID 即可。
