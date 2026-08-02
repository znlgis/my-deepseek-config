# Gemini CLI × DeepSeek 配置

用 [Gemini CLI](https://github.com/google-gemini/gemini-cli) 接入 DeepSeek。

> ⚠️ **前提**：上游 [google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli)
> **只支持 Google 后端，无法直连 DeepSeek**（认证类型仅限 Google 系）。要在 Gemini CLI
> 风格的终端里用 DeepSeek，二选一：
>
> 1. 使用支持多 Provider 的分支（如 `llxprt-code`），或用
>    [LiteLLM](https://github.com/BerriAI/litellm) 等代理把 DeepSeek 暴露为 OpenAI 兼容端点，
>    再把 `OPENAI_BASE_URL` 指向该端点/分支；
> 2. 直接改用同为 Gemini CLI 家族、**原生支持 DeepSeek** 的
>    [Qwen Code](../qwen-code) 或 [iFlow CLI](../iflow-cli)。
>
> 本目录的 `GEMINI.md` 与 `.gemini/skills/` 是**与模型无关的工作纪律**，无论用哪种方式接入都适用。

## 文件说明

| 文件            | 作用                                                |
| --------------- | --------------------------------------------------- |
| `.env.example`  | DeepSeek 端点 / Key / 模型的环境变量示例（经代理/分支时使用） |
| `.gemini/settings.json` | Gemini CLI 行为设置（审批模式、上下文文件名、隐私）——**不含**私有认证 |
| `.gemini/skills/` | 4 个可复用 Skill（SKILL.md），与模型无关            |
| `GEMINI.md`     | 项目级记忆（核心原则、模型分工、纪律）              |

## 安装

```bash
npm install -g @google/gemini-cli
```

## 配置步骤

1. **申请 DeepSeek API Key**：https://platform.deepseek.com 。
2. **放置环境变量**：把 `.env.example` 复制为 `.gemini/.env` 或 `~/.gemini/.env`，填入真实 Key。
   （仅在使用多 Provider 分支或 LiteLLM 代理时生效；上游 Gemini CLI 会忽略 `OPENAI_*`。）
3. **放置配置文件**：把 `.gemini/` 目录复制到项目根（已内含 `settings.json`、`skills/`）或
   `~/.gemini/`，把 `GEMINI.md` 复制到项目根目录或 `~/.gemini/GEMINI.md`。
4. **启动**：

   ```bash
   gemini
   ```

## 关键环境变量

| 变量              | 值                            | 说明                     |
| ----------------- | ----------------------------- | ------------------------ |
| `OPENAI_BASE_URL` | `https://api.deepseek.com/v1` | DeepSeek OpenAI 兼容端点（或代理地址） |
| `OPENAI_API_KEY`  | `sk-...`                      | DeepSeek API Key         |
| `OPENAI_MODEL`    | `deepseek-v4-flash`           | 默认模型（Flash）        |

> 模型分工：`deepseek-v4-pro`（Pro / 强推理）+ `deepseek-v4-flash`（Flash / 快速）。
> 仅在多 Provider 分支或代理场景下适用；上游 Gemini CLI 仅支持 Google 模型。
