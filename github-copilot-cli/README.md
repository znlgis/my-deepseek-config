# GitHub Copilot CLI × DeepSeek 配置

用 [GitHub Copilot CLI](https://docs.github.com/en/copilot/concepts/agents/about-copilot-cli)
落地本仓库统一的「DeepSeek 双模型」工作理念。

> ⚠️ **重要说明**：GitHub Copilot CLI 的后端模型由 GitHub 托管（Claude / GPT / Gemini 等），
> **目前不支持把 DeepSeek 直接作为其推理模型（无 BYOK/自定义端点）**。
> 因此本目录提供两件事：
> 1. **统一指令**：`AGENTS.md` 注入与其它工具一致的原则与纪律；
> 2. **DeepSeek 桥接（可选）**：通过 MCP 把 DeepSeek 作为可调用工具接入。
> 若你需要 DeepSeek 作为**主推理模型**，请改用本仓库的
> [Claude Code](../claude-code) / [Codex](../codex) / [Qwen Code](../qwen-code) 等目录。

## 文件说明

| 文件          | 作用                                                    |
| ------------- | ------------------------------------------------------- |
| `AGENTS.md`   | 仓库级自定义指令（Copilot CLI 自动读取）                |
| `config.json` | Copilot CLI 用户配置示例（横幅、主题、信任目录、MCP）   |

## 安装

```bash
npm install -g @github/copilot
```

## 配置步骤

1. **登录**：首次运行 `copilot`，用 `/login` 完成 GitHub 认证（需有 Copilot 订阅）。
2. **放置指令**：把 `AGENTS.md` 复制到你的项目根目录，Copilot CLI 会自动应用。
3. **放置用户配置（可选）**：把 `config.json` 内容合并到 `~/.copilot/config.json`。
4. **选择模型**：进入会话后用 `/model` 选择一个强推理模型承担「Pro」角色的规划 / 审查工作。

## DeepSeek 桥接（可选，通过 MCP）

若希望在 Copilot CLI 中真正调用到 DeepSeek，可把 DeepSeek 包装为一个 MCP 服务，
在 `config.json` 的 `mcp_servers` 中注册后，Copilot 即可将其作为工具调用：

```jsonc
{
  "mcp_servers": {
    "deepseek": {
      "command": "npx",
      "args": ["-y", "some-openai-compatible-mcp-server"],
      "env": {
        "OPENAI_BASE_URL": "https://api.deepseek.com/v1",
        "OPENAI_API_KEY": "sk-your-deepseek-key",
        "OPENAI_MODEL": "deepseek-reasoner"
      }
    }
  }
}
```

> 具体 MCP 服务实现可自选；这里仅示例如何把 DeepSeek 端点 / Key / 模型注入。
> 切勿把真实 API Key 提交进仓库。
