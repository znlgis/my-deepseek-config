# iFlow CLI × DeepSeek 配置

用 [iFlow CLI（心流）](https://platform.iflow.cn/cli) 通过 OpenAI 兼容配置接入 DeepSeek。
iFlow CLI 属于 Gemini CLI 家族，支持自定义第三方模型。

## 文件说明

| 文件            | 作用                                                     |
| --------------- | -------------------------------------------------------- |
| `settings.json.example` | iFlow 设置示例：认证类型、DeepSeek 端点 / Key / 模型、上下文文件名 |
| `IFLOW.md`      | 项目级记忆（核心原则、模型分工、纪律）                   |

## 安装

```bash
npm install -g @iflow-ai/iflow-cli
# 或： curl -fsSL https://cloud.iflow.cn/iflow-cli/install.sh | bash
```

## 配置步骤

1. **申请 DeepSeek API Key**：https://platform.deepseek.com 。

2. **放置配置**：把 `settings.json.example` 复制为 `~/.iflow/settings.json`（或项目级 `.iflow/settings.json`），
   把其中的 `apiKey` 替换为真实 Key；把 `IFLOW.md` 复制到项目根目录或 `~/.iflow/IFLOW.md`。

   > ⚠️ `settings.json` 会存放 `apiKey`。真实配置请放在 iFlow 配置目录，切勿把含真实 Key 的文件提交进仓库。

3. **启动**：

   ```bash
   iflow
   ```

   进入会话后可用 `/model` 在 `deepseek-chat` 与 `deepseek-reasoner` 间切换。

## 关键配置（settings.json）

| 键                 | 值                            | 说明                     |
| ------------------ | ----------------------------- | ------------------------ |
| `selectedAuthType` | `openai-compatible`           | 使用 OpenAI 兼容自定义模型 |
| `baseUrl`          | `https://api.deepseek.com/v1` | DeepSeek 端点            |
| `apiKey`           | `sk-...`                      | DeepSeek API Key         |
| `modelName`        | `deepseek-chat`               | 默认模型（Flash）        |

> 模型分工：`deepseek-reasoner`（Pro / 强推理）+ `deepseek-chat`（Flash / 快速）。
