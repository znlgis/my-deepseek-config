# Gemini CLI × DeepSeek 配置

用 [Gemini CLI](https://github.com/google-gemini/gemini-cli) 接入 DeepSeek。

> ⚠️ **前提**：Gemini CLI 原生面向 Google Gemini。使用 DeepSeek 需要**支持 OpenAI 兼容
> Provider 的 Gemini CLI 版本**（`selectedAuthType: "openai"`）。若你的版本不支持，
> 可用 [LiteLLM](https://github.com/BerriAI/litellm) 等代理把 DeepSeek 暴露为
> OpenAI/Gemini 兼容端点，再把 `OPENAI_BASE_URL` 指向该代理。
> 若只想开箱即用的 DeepSeek 终端体验，推荐直接用同为 Gemini CLI 家族的
> [Qwen Code](../qwen-code) 或 [iFlow CLI](../iflow-cli)。

## 文件说明

| 文件            | 作用                                                |
| --------------- | --------------------------------------------------- |
| `.env.example`  | DeepSeek 端点 / Key / 模型的环境变量示例            |
| `settings.json` | Gemini CLI 行为设置（认证类型、上下文文件名、隐私） |
| `GEMINI.md`     | 项目级记忆（核心原则、模型分工、纪律）              |

## 安装

```bash
npm install -g @google/gemini-cli
```

## 配置步骤

1. **申请 DeepSeek API Key**：https://platform.deepseek.com 。
2. **放置环境变量**：把 `.env.example` 复制为 `.gemini/.env` 或 `~/.gemini/.env`，填入真实 Key。
3. **放置配置文件**：把 `settings.json` 复制到 `.gemini/settings.json` 或 `~/.gemini/settings.json`，
   把 `GEMINI.md` 复制到项目根目录或 `~/.gemini/GEMINI.md`。
4. **启动**：

   ```bash
   gemini
   ```

## 关键环境变量

| 变量              | 值                            | 说明                     |
| ----------------- | ----------------------------- | ------------------------ |
| `OPENAI_BASE_URL` | `https://api.deepseek.com/v1` | DeepSeek OpenAI 兼容端点 |
| `OPENAI_API_KEY`  | `sk-...`                      | DeepSeek API Key         |
| `OPENAI_MODEL`    | `deepseek-chat`               | 默认模型（Flash）        |

> 模型分工：`deepseek-reasoner`（Pro / 强推理）+ `deepseek-chat`（Flash / 快速）。
