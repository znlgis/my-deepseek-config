# GitHub Copilot CLI — DeepSeek 配置集

> 本目录提供 GitHub Copilot CLI 的仓库级配置文件和可选 Skills，
> 将 DeepSeek 双模型分工理念注入 Copilot CLI 工作流。

## 目录结构

```
copilot-cli/
├── .github/
│   ├── copilot-instructions.md   # 仓库级指令（自动加载）
│   └── settings.json             # Copilot CLI 配置
├── skills/                       # 可选 Agent Skills 源文件
│   ├── code-review.agent.md
│   ├── conventional-commits.agent.md
│   ├── security-review.agent.md
│   └── verification-before-completion.agent.md
└── README.md                     # 本文件
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

### `.github/settings.json`

Copilot CLI 的默认配置：
- 模型：`claude-sonnet-4.5`
- 交互模式：每次操作前询问用户确认
- 自动更新：开启（stable 通道）
- Footer 显示：模型/effort、目录、分支、配额

### Skills 源文件

Skills 是用户级配置，安装在 `~/.copilot/skills/` 下。
本仓库提供源文件方便分发和版本管理。

| Skill | 用途 |
| --- | --- |
| `code-review.agent.md` | 五维度代码审查（正确性、安全、性能、架构、可维护性） |
| `conventional-commits.agent.md` | Conventional Commits 格式的提交信息与 PR 标题 |
| `security-review.agent.md` | 安全漏洞审查（注入、XSS、SSRF、秘钥泄露等） |
| `verification-before-completion.agent.md` | 完成前强制验证纪律：证据先行，无验证不声称完成 |

## Skills 安装

将 skills 源文件复制到 Copilot CLI 的用户 skills 目录：

```powershell
# Windows
Copy-Item -Path "copilot-cli\skills\*.agent.md" -Destination "$env:USERPROFILE\.copilot\skills\"
```

```bash
# macOS / Linux
cp copilot-cli/skills/*.agent.md ~/.copilot/skills/
```

安装后重启 Copilot CLI 会话即可使用。在对话中引用 skill 名称触发（例如 "review 当前改动" 会触发 `code-review` skill）。

## 关于 DeepSeek 模型

GitHub Copilot CLI 的后端模型由 GitHub 托管，**无法直接把 DeepSeek 作为推理模型**。
本配置集的定位是：

1. 通过 `copilot-instructions.md` 注入 DeepSeek 的工作理念与纪律；
2. 通过 Skills 扩展审查、安全、提交等标准化工作流。

选择内置模型时，建议选用具备强推理能力的模型承担复杂任务。

## 关联资源

- [全局通用规则](../shared/RULES.md) — 所有工具共享的核心原则
- [GitHub Copilot CLI 官方文档](https://docs.github.com/copilot)
