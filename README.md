# my-deepseek-config

**多工具 × DeepSeek 最优配置** —— 借鉴
[my-opencode-deepseek-config](https://github.com/znlgis/my-opencode-deepseek-config)
的「Token 效率优先、双模型分工」理念，为主流终端 AI 编码工具分别提供 DeepSeek 专用配置，
每个工具独立成文件夹，参考各工具最新版本的推荐配置。

## 核心理念

- **DeepSeek 双模型分工**：用 `deepseek-reasoner`（Pro / 强推理）+ `deepseek-chat`（Flash / 快速）
  做成本感知的分工——搜索 / 简单编辑走 Flash，规划 / 审查 / 重型实现走 Pro。
- **Token 效率优先**：引用路径而非粘贴整文件、并行独立读取、积极压缩上下文、快变库先检索。
- **纯配置理念**：优先改提示词 / 配置，而非引入新工具；未经请求不创建文件。
- **统一纪律**：跨工具共享一套核心原则、反模式与质量基线，见 [`shared/RULES.md`](shared/RULES.md)。

> **模型命名**：统一使用 DeepSeek 官方当前可用的 `deepseek-chat` / `deepseek-reasoner`。
> 参考仓库中的 `deepseek-v4-pro` / `deepseek-v4-flash` 为其双模型分工命名思路，
> 待官方发布后只需在各工具配置里替换模型 ID。

## 目录结构

| 工具 | 目录 | 接入方式 | 记忆/指令文件 |
| --- | --- | --- | --- |
| 共享规则 | [`shared/`](shared) | 跨工具通用操作规则 | `RULES.md` |
| Claude Code | [`claude-code/`](claude-code) | Anthropic 兼容端点 | `CLAUDE.md` |
| Qwen Code | [`qwen-code/`](qwen-code) | OpenAI 兼容端点 | `QWEN.md` |
| Gemini CLI | [`gemini-cli/`](gemini-cli) | OpenAI 兼容端点 / 代理 | `GEMINI.md` |
| OpenAI Codex | [`codex/`](codex) | 自定义 model_providers | `AGENTS.md` |
| iFlow CLI | [`iflow-cli/`](iflow-cli) | OpenAI 兼容端点 | `IFLOW.md` |
| GitHub Copilot CLI | [`github-copilot-cli/`](github-copilot-cli) | GitHub 托管模型 + MCP 桥接 | `AGENTS.md` |
| Kimi CLI | [`kimi-cli/`](kimi-cli) | OpenAI 兼容 provider | `AGENTS.md` |

每个工具目录内都含一个 `README.md`，说明安装、配置步骤与关键参数。

## DeepSeek 接入端点

- **OpenAI 兼容**：`https://api.deepseek.com/v1`（Qwen Code / Gemini CLI / Codex / iFlow / Kimi CLI）
- **Anthropic 兼容**：`https://api.deepseek.com/anthropic`（Claude Code）
- **API Key**：在 https://platform.deepseek.com 申请，通过环境变量注入，**切勿写入仓库**。

## 快速开始

1. 选择你使用的工具，进入对应目录并阅读其 `README.md`。
2. 在 https://platform.deepseek.com 申请 API Key。
3. 按该目录说明放置配置文件、注入 Key、启动工具即可。

## 关于模型选择的说明

- Claude Code / Qwen Code / Codex / iFlow / Kimi CLI 均可将 DeepSeek 作为**主推理模型**。
- **GitHub Copilot CLI** 的后端模型由 GitHub 托管，目前不支持把 DeepSeek 直接作为推理模型；
  该目录提供统一指令（`AGENTS.md`）与通过 MCP 桥接 DeepSeek 的可选方案。
- **Gemini CLI** 原生面向 Google Gemini，使用 DeepSeek 需支持 OpenAI 兼容 Provider 的版本，
  或配合 LiteLLM 等代理，详见该目录 `README.md`。

## 安全提示

- 所有配置示例中的 API Key 均为占位符，请替换为自己的 Key。
- 优先通过**环境变量**注入 Key；对会把 Key 落盘的工具（iFlow / Kimi），纳入版本管理时请用占位符或忽略该文件。
- 权限基线遵循「默认放行、破坏性命令征求同意、敏感文件拒绝读取」。

## 许可

见 [LICENSE](LICENSE)。
