# Qwen Code × DeepSeek 配置

用 [Qwen Code](https://github.com/QwenLM/qwen-code) 通过 **OpenAI 兼容端点** 接入 DeepSeek。
Qwen Code 是活跃维护的开源终端编码代理，多协议支持 OpenAI / Anthropic / Gemini / Qwen。

## 文件说明

| 文件            | 作用                                                  |
| --------------- | ----------------------------------------------------- |
| `.env.example`  | DeepSeek 端点 / Key / 模型的环境变量示例              |
| `.qwen/settings.json` | Qwen Code 行为设置（`selectedType: openai`、模型、上下文文件名、权限、隐私） |
| `.qwen/skills/` | 4 个可复用 Skill（SKILL.md）                          |
| `QWEN.md`       | 项目级记忆（核心原则、模型分工、纪律）                |

## 安装

```bash
npm install -g @qwen-code/qwen-code@latest
```

## 配置步骤

1. **申请 DeepSeek API Key**：https://platform.deepseek.com 。

2. **放置环境变量**：把 `.env.example` 复制为 `.qwen/.env`（项目级）或
   `~/.qwen/.env`（全局），填入真实 Key。也可用交互式 `/auth` 选择 OpenAI 协议后填写。

3. **放置配置文件**：把 `.qwen/` 目录复制到项目根（已内含 `settings.json`、`skills/`）或
   `~/.qwen/`，把 `QWEN.md` 复制到项目根目录或 `~/.qwen/QWEN.md`。
   `.qwen/settings.json` 已设 `security.auth.selectedType: "openai"`，配合上面的 `.env` 即用 DeepSeek。

4. **启动**：

   ```bash
   qwen
   ```

   进入会话后可用 `/model` 在 `deepseek-v4-flash` 与 `deepseek-v4-pro` 间切换。

## 关键环境变量

| 变量              | 值                            | 说明                     |
| ----------------- | ----------------------------- | ------------------------ |
| `OPENAI_BASE_URL` | `https://api.deepseek.com/v1` | DeepSeek OpenAI 兼容端点 |
| `OPENAI_API_KEY`  | `sk-...`                      | DeepSeek API Key         |
| `OPENAI_MODEL`    | `deepseek-v4-flash`               | 默认模型（Flash）        |

> 模型分工：`deepseek-v4-pro`（Pro / 强推理）+ `deepseek-v4-flash`（Flash / 快速）。
> `deepseek-v4-pro` / `deepseek-v4-flash` 为 DeepSeek 当前官方模型；旧别名 `deepseek-reasoner` / `deepseek-chat` 已并入并停用。
