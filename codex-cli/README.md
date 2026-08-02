# OpenAI Codex CLI × DeepSeek 配置

用 [OpenAI Codex CLI](https://github.com/openai/codex) 接入 DeepSeek，并注入 DeepSeek 双模型工作纪律。

> ⚠️ **前提**：新版 Codex CLI **只支持 OpenAI Responses API**（`wire_api = "responses"`），
> 已移除 Chat Completions（`wire_api = "chat"` 会直接报错）。DeepSeek 只提供 Chat Completions /
> Anthropic 接口，**不提供 Responses API**，因此无法直连，需要经 **Responses 兼容代理**
> （如 [LiteLLM](https://github.com/BerriAI/litellm)）转换后再接入。参考
> [openai/codex#7782](https://github.com/openai/codex/discussions/7782)。
>
> 若想开箱即用的 DeepSeek 终端体验，也可改用 [Qwen Code](../qwen-code) / [iFlow CLI](../iflow-cli)
> / [Kimi CLI](../kimi-cli)，它们原生支持 DeepSeek 的 Chat Completions 端点。

## 文件说明

| 文件                 | 作用                                                        |
| -------------------- | ----------------------------------------------------------- |
| `.codex/config.toml` | Codex 配置：DeepSeek provider（经 Responses 代理）、审批与沙箱 |
| `.codex/skills/`     | 4 个可复用 Skill（SKILL.md）                                |
| `AGENTS.md`          | 项目级记忆（核心原则、模型分工、纪律）                       |

## 安装

```bash
npm install -g @openai/codex
```

## 配置步骤

1. **申请 DeepSeek API Key**：https://platform.deepseek.com ，并设置环境变量（勿写入配置文件）：

   ```bash
   export DEEPSEEK_API_KEY="sk-your-deepseek-key"
   ```

2. **架设 Responses 兼容代理**（DeepSeek 直连不可用）。以 LiteLLM 为例，让它把 DeepSeek
   暴露为 `/v1/responses` 兼容端点（默认监听 `http://localhost:4000`）。

3. **放置配置**：把 `.codex/config.toml` 复制到 `~/.codex/config.toml`
   （provider 定义只在全局配置生效），把其中 `base_url` 指向你的代理地址；
   把 `AGENTS.md` 复制到项目根目录，`.codex/skills/` 复制到项目根的 `.codex/skills/`。

4. **启动**：

   ```bash
   codex
   ```

## 关键配置（config.toml）

| 键                                | 值                          | 说明                                   |
| --------------------------------- | --------------------------- | -------------------------------------- |
| `model`                           | `deepseek-v4-pro`           | 主模型（Pro / 强推理）                 |
| `model_provider`                  | `deepseek`                  | 指向下面的 `[model_providers.deepseek]` |
| `model_providers.deepseek.base_url` | 你的代理地址（示例 `http://localhost:4000/v1`） | Responses 兼容代理，而非 DeepSeek 官方地址 |
| `model_providers.deepseek.env_key`  | `DEEPSEEK_API_KEY`        | 存放 DeepSeek Key 的环境变量名          |
| `model_providers.deepseek.wire_api` | `responses`              | 新版 Codex 唯一支持的协议               |
| `approval_policy`                 | `on-request`                | 越权操作按需征求同意                   |
| `sandbox_mode`                    | `workspace-write`           | 可写工作区，其余受限                   |

> 轻量任务可在会话中用 `/model` 切换到 `deepseek-v4-flash`（需代理侧同样已注册该模型）。
