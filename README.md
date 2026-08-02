# my-deepseek-config

**多工具 AI 编码助手通用配置集** —— 借鉴
[my-opencode-deepseek-config](https://github.com/znlgis/my-opencode-deepseek-config)
的「意图优先、角色隔离、Skill 复用」架构思路，为 7 款主流终端 AI 编码工具分别提供完整的
**官方格式配置**（指令文件 + 设置 + Skill），每个工具独立成文件夹。

## 核心理念

- **意图优先**：先理解真正意图再行动，"看看 X"≠"改 X"，不确定时先确认。
- **最小改动**：只改完成任务必需的代码，不动无关代码。一个完整、正确的方案优于一个花哨的方案。
- **角色隔离**：只读 Agent（审查、分析）绝不写文件；执行 Agent 专注实现，不越界做研究。
- **Skill 复用**：跨工具提供 4 个可复用 Skill——code-review、conventional-commits、security-review、verification-before-completion——按各工具的 Skill/SubCommand 格式独立实现。
- **纯配置理念**：优先改提示词 / 配置，而非引入新工具；未经请求不创建文件。
- **统一纪律**：跨工具共享一套核心原则、反模式与质量基线，见各工具的主指令文件。

## 目录结构

| 工具 | 目录 | 配置格式 | 主指令文件 | Skill 体系 |
| --- | --- | --- | --- | --- |
| Claude Code | [`claude-code/`](claude-code) | CLAUDE.md + .claude/ | `CLAUDE.md` | 4× SKILL.md + rules/ 分层 |
| Gemini CLI | [`gemini-cli/`](gemini-cli) | GEMINI.md + .gemini/ | `GEMINI.md` | 4× SKILL.md |
| Qwen Code | [`qwen-code/`](qwen-code) | QWEN.md + .qwen/ | `QWEN.md`（层级加载） | 4× SKILL.md |
| GitHub Copilot CLI | [`copilot-cli/`](copilot-cli) | .github/ + .copilot/ | `copilot-instructions.md` | 4× SKILL.md |
| OpenAI Codex | [`codex-cli/`](codex-cli) | AGENTS.md + .codex/ | `AGENTS.md` | 4× SKILL.md |
| iFlow CLI | [`iflow-cli/`](iflow-cli) | IFLOW.md + .iflow/ | `IFLOW.md` | 4× SubCommand TOML |
| Kimi CLI | [`kimi-cli/`](kimi-cli) | AGENTS.md + config.toml | `AGENTS.md` | 4× SKILL.md |

每个工具目录内含至少以下内容（具体布局随工具原生约定略有差异）：

```
<tool>/
├── <主指令文件>          # 核心行为准则 + 代码风格 + 反模式 + 证据纪律
├── <原生配置目录/文件>    # .claude/ .gemini/ .qwen/ .iflow/ .codex/ 或 config.toml
│   ├── settings.json     # 权限 / 模型 / 上下文设置（部分工具为 config.toml）
│   ├── skills/           # 可复用 Skill（iFlow 为 commands/；Copilot 在 .github/skills/）
│   └── ...               # 工具特有配置（rules/、commands/ 等）
├── .env.example          # 需要密钥的工具附带（BYOK / OpenAI 兼容变量）
└── README.md             # 安装与使用说明
```

## 4 个通用 Skill

| Skill | 用途 | 各工具实现方式 |
| --- | --- | --- |
| **code-review** | 多维度代码审查，按 diff 大小缩放深度，严重度分级 | Claude/Gemini/Qwen/Codex/Kimi/Copilot: SKILL.md；iFlow: SubCommand TOML |
| **conventional-commits** | 按 Conventional Commits 规范生成 commit message | 同上 |
| **security-review** | 安全漏洞审计（注入/密钥/认证/数据暴露/依赖等 12 项检查） | 同上 |
| **verification-before-completion** | 完成前强制验证门禁（文件完整性/调用者/构建/测试/lint） | 同上 |

## 通用规则蓝本（注入所有指令文件）

以下规则在所有 7 个工具的主指令文件中均有体现：

**核心原则**：意图优先、最小改动、读前不猜、独立任务并行、不主动创建文件、停止条件明确、语言跟随 OS

**反模式（严格禁止）**：万能文件（utils/helpers/service）、emoji、AI 填充词、空 catch、无注释的 @ts-ignore、注释掉的代码

**代码风格**：const > let、早返回避免 else、函数式数组优先 for 循环、减少中间变量、不用不必要解构/别名/通配符导入

**注释纪律**：注释解释 WHY 不解释 WHAT；不写 AI 模板注释

**证据纪律**：完成前必须有可验证证据（测试通过 / build 成功 / lint 干净 / grep 验证）

## 快速开始

1. 选择你使用的工具，进入对应目录。
2. 将目录下的配置文件复制到工具期望的位置（详见各工具 README 或目录内说明）：
   - Claude Code：`CLAUDE.md` → 项目根，`.claude/` → 项目根（原生支持 DeepSeek，走 Anthropic 兼容端点）
   - Qwen Code / iFlow CLI：`.qwen/` / `.iflow/` → 项目根（原生支持 DeepSeek，OpenAI 兼容）
   - Gemini CLI：`.gemini/` → 项目根。注意上游仅支持 Google 模型，接 DeepSeek 需 LiteLLM 代理或 `llxprt-code` 分叉；纯 DeepSeek 场景建议改用 Qwen Code / iFlow CLI
   - Copilot CLI：`.github/` → 项目根（`copilot-instructions.md` + `skills/` 自动发现），`.copilot/settings.json` → `~/.copilot/`，再按 `.env.example` 配置 BYOK 环境变量把 DeepSeek 接为后端模型
   - Codex CLI：`.codex/` → 项目根。DeepSeek 无 Responses API，需经 LiteLLM 等 Responses 兼容代理接入，且 provider 定义须放全局 `~/.codex/config.toml`
   - Kimi CLI：`config.toml` → `~/.kimi/`，`skills/` → `~/.kimi/skills/`（原生支持 DeepSeek，OpenAI 兼容）
3. 按各工具要求配置 API Key（环境变量或配置文件），**切勿将 Key 写入仓库**。
4. 启动工具即可。

## 安全提示

- 所有配置示例中的 API Key 均为占位符，请替换为自己的 Key。
- 优先通过**环境变量**注入 Key；对会把 Key 落盘的工具，纳入版本管理时请用占位符或忽略该文件。
- 权限基线遵循「默认放行安全命令、破坏性命令征求同意、敏感文件拒绝读取」。

## 许可

见 [LICENSE](LICENSE)。
