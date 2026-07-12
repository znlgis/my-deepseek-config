# Qwen Code × DeepSeek 配置

用 [Qwen Code](https://github.com/QwenLM/qwen-code) 通过 **OpenAI 兼容端点** 接入 DeepSeek。
Qwen Code 是活跃维护的开源终端编码代理，多协议支持 OpenAI / Anthropic / Gemini / Qwen。

## 文件说明

| 文件            | 作用                                                  |
| --------------- | ----------------------------------------------------- |
| `.env.example`  | DeepSeek 端点 / Key / 模型的环境变量示例              |
| `settings.json` | Qwen Code 行为设置（模型、上下文文件名、隐私、工具）  |
| `QWEN.md`       | 项目级记忆（核心原则、模型分工、纪律）                |

## 安装

```bash
npm install -g @qwen-code/qwen-code@latest
```

## 配置步骤

1. **申请 DeepSeek API Key**：https://platform.deepseek.com 。

2. **放置环境变量**：把 `.env.example` 复制为 `.qwen/.env`（项目级）或
   `~/.qwen/.env`（全局），填入真实 Key。也可用交互式 `/auth` 选择 OpenAI 协议后填写。

3. **放置配置文件**：把 `settings.json` 复制到 `.qwen/settings.json`（项目级）或
   `~/.qwen/settings.json`（全局），把 `QWEN.md` 复制到项目根目录或 `~/.qwen/QWEN.md`。

4. **启动**：

   ```bash
   qwen
   ```

   进入会话后可用 `/model` 在 `deepseek-chat` 与 `deepseek-reasoner` 间切换。

## 关键环境变量

| 变量              | 值                            | 说明                     |
| ----------------- | ----------------------------- | ------------------------ |
| `OPENAI_BASE_URL` | `https://api.deepseek.com/v1` | DeepSeek OpenAI 兼容端点 |
| `OPENAI_API_KEY`  | `sk-...`                      | DeepSeek API Key         |
| `OPENAI_MODEL`    | `deepseek-chat`               | 默认模型（Flash）        |

> 模型分工：`deepseek-reasoner`（Pro / 强推理）+ `deepseek-chat`（Flash / 快速）。
> 待 DeepSeek V4（pro/flash）发布后替换模型 ID 即可。
