# OpenAI Codex CLI × DeepSeek 配置

用 [Codex CLI](https://github.com/openai/codex) 通过自定义 `model_providers` 接入 DeepSeek 的
OpenAI 兼容端点，并预置 `pro` / `flash` 两个 profile 对应双模型分工。

## 文件说明

| 文件          | 作用                                                       |
| ------------- | ---------------------------------------------------------- |
| `config.toml` | Codex 配置：DeepSeek provider、默认模型、审批/沙箱、profile |
| `AGENTS.md`   | 项目级指令（核心原则、模型分工、纪律）                     |

## 安装

```bash
npm install -g @openai/codex
# 或 brew install codex
```

## 配置步骤

1. **申请 DeepSeek API Key**：https://platform.deepseek.com 。

2. **设置环境变量**（不要写入配置文件）：

   ```bash
   export DEEPSEEK_API_KEY="sk-your-deepseek-key"      # macOS / Linux
   ```

   ```powershell
   $env:DEEPSEEK_API_KEY="sk-your-deepseek-key"        # Windows PowerShell
   ```

3. **放置配置**：把 `config.toml` 复制到 `~/.codex/config.toml`；
   把 `AGENTS.md` 复制到项目根目录（或 `~/.codex/AGENTS.md` 作为全局指令）。

4. **启动**：

   ```bash
   codex                       # 默认 deepseek-chat（Flash）
   codex --profile pro         # deepseek-reasoner（Pro / 强推理）
   codex -m deepseek-reasoner  # 临时切换模型
   ```

## 关键配置（config.toml）

| 键                              | 值                            | 说明                         |
| ------------------------------- | ----------------------------- | ---------------------------- |
| `model_providers.deepseek.base_url` | `https://api.deepseek.com/v1` | DeepSeek OpenAI 兼容端点 |
| `model_providers.deepseek.env_key`  | `DEEPSEEK_API_KEY`        | 读取 Key 的环境变量名        |
| `model_providers.deepseek.wire_api` | `chat`                    | 使用 chat completions 协议   |
| `model`                         | `deepseek-chat`               | 默认模型（Flash）            |
| `approval_policy` / `sandbox_mode` | `on-request` / `workspace-write` | 审批与沙箱基线        |

> 模型分工：`deepseek-reasoner`（Pro）+ `deepseek-chat`（Flash），已分别做成 `pro` / `flash` profile。
