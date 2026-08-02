# GitHub Copilot CLI — DeepSeek 配置集

> 本目录提供 GitHub Copilot CLI 的仓库级配置文件、可选 Skills，
> 以及通过 **BYOK（自带密钥）** 把 DeepSeek 作为推理模型接入的环境变量示例。

## 目录结构

```
copilot-cli/
├── .github/
│   ├── copilot-instructions.md          # 仓库级指令（自动加载）
│   └── skills/                          # 仓库级 Skills（自动发现）
│       ├── code-review/SKILL.md
│       ├── conventional-commits/SKILL.md
│       ├── security-review/SKILL.md
│       └── verification-before-completion/SKILL.md
├── .copilot/
│   └── settings.json                    # Copilot CLI 行为配置
├── .env.example                         # BYOK 接入 DeepSeek 的环境变量示例
└── README.md                            # 本文件
```

## 文件说明

### `.github/copilot-instructions.md`

仓库级自定义指令，Copilot CLI 在每次会话中自动加载。内容基于
[`shared/RULES.md`](../shared/RULES.md) 翻译为 Copilot CLI 可执行的约束,
涵盖：

- 核心原则（意图优先、最小改动、先读后写等）
- 反模式（禁止万能文件名、AI 填充词、空 catch、死代码）
- 代码风格（`const` 优先、提前返回、函数式数组方法）
- 注释与证据纪律
- 提交前自检流程

将此文件放入任意仓库的 `.github/` 目录即可生效。

### `.copilot/settings.json`

Copilot CLI 的行为配置。真实生效位置是用户级 `~/.copilot/settings.json`
（可用 `COPILOT_HOME` 覆盖）；本仓库放在项目级 `.copilot/settings.json` 便于分发，
使用时复制到 `~/.copilot/settings.json` 即可。当前示例设置：

- `effortLevel`：推理投入档位（`low` / `medium` / `high` / `xhigh`），此处为 `high`
- `askUser`：每次执行前询问用户确认
- `autoUpdate`：开启自动更新
- `renderMarkdown`：终端渲染 Markdown

> 说明：DeepSeek 模型不通过 settings.json 指定，而是通过 `.env.example` 里的
> `COPILOT_MODEL` 等 BYOK 环境变量注入（见下文）。

### Skills（`.github/skills/<name>/SKILL.md`）

Skills 用一句话描述 + 一段说明扩展 Copilot CLI 的能力。放在仓库
`.github/skills/<name>/SKILL.md` 会被**自动发现**；也可安装到用户级
`~/.copilot/skills/<name>/SKILL.md` 全局启用。每个 `SKILL.md` 的 frontmatter
只需 `name` 和 `description` 两个必填字段。

| Skill | 用途 |
| --- | --- |
| `code-review` | 五维度代码审查（正确性、安全、性能、架构、可维护性） |
| `conventional-commits` | Conventional Commits 格式的提交信息与 PR 标题 |
| `security-review` | 安全漏洞审查（注入、XSS、SSRF、秘钥泄露等） |
| `verification-before-completion` | 完成前强制验证纪律：证据先行，无验证不声称完成 |

## Skills 安装

仓库级 `.github/skills/` 在该仓库内自动生效，无需安装。若想全局启用，复制到用户
skills 目录：

```powershell
# Windows
Copy-Item -Recurse -Path "copilot-cli\.github\skills\*" -Destination "$env:USERPROFILE\.copilot\skills\"
```

```bash
# macOS / Linux
cp -r copilot-cli/.github/skills/* ~/.copilot/skills/
```

安装后重启 Copilot CLI 会话即可使用。在对话中引用 skill 名称触发（例如 "review 当前改动" 会触发 `code-review` skill）。

## 关于 DeepSeek 模型（BYOK）

GitHub Copilot CLI 支持 **BYOK（Bring Your Own Key，自带密钥）**，
可以把 DeepSeek 直接作为推理模型。DeepSeek 官方推荐使用其 **Anthropic 兼容端点**接入。

复制 `.env.example` 为 `.env`（或直接 `export` 到当前 shell），填入真实密钥：

```bash
COPILOT_PROVIDER_TYPE=anthropic
COPILOT_PROVIDER_BASE_URL=https://api.deepseek.com/anthropic
COPILOT_PROVIDER_API_KEY=sk-your-deepseek-key
COPILOT_MODEL=deepseek-v4-pro      # 强推理主模型；轻量任务可换 deepseek-v4-flash
```

设置后启动 `copilot`，即由 DeepSeek 承担推理。本配置集的整体定位：

1. 通过 BYOK 环境变量把 DeepSeek 接为后端模型；
2. 通过 `copilot-instructions.md` 注入 DeepSeek 的工作理念与纪律；
3. 通过 Skills 扩展审查、安全、提交等标准化工作流。

> 若不使用 BYOK，Copilot CLI 会回退到 GitHub 托管模型，此时以上指令与 Skills 依然生效。

## 关联资源

- [全局通用规则](../shared/RULES.md) — 所有工具共享的核心原则
- [GitHub Copilot CLI 官方文档](https://docs.github.com/copilot)
