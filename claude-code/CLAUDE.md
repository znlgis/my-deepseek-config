# CLAUDE.md — Claude Code × DeepSeek 项目记忆

> 本文件是 Claude Code 的项目级记忆（Memory）。放在项目根目录或 `~/.claude/CLAUDE.md`
> 作为全局记忆。核心通用规则见仓库根的 [`shared/RULES.md`](../shared/RULES.md)，
> 本文件只保留 Claude Code 专属要点与内嵌的关键纪律。

## 模型分工（DeepSeek 双模型）

Claude Code 通过 `ANTHROPIC_MODEL` / `ANTHROPIC_SMALL_FAST_MODEL` 支持主模型 + 轻量模型：

- **主模型（Pro）**：`deepseek-reasoner` —— 规划、架构、根因分析、代码审查、重型实现。
- **轻量模型（Flash）**：`deepseek-chat` —— 后台快速任务、标题生成、简单编辑、探索。

> 待 DeepSeek V4（pro/flash）发布，只需在 `settings.json` 的 `env` 中替换这两个模型 ID。

## 核心原则（摘要，完整见 shared/RULES.md）

1. 先辨意图再动手：「解释 X」不是「改 X」。
2. 做能完整解决问题的最小改动，不碰无关代码。
3. 先读后写，用真实文件验证假设。
4. 并行处理独立的读取 / 搜索。
5. 未经请求不创建文件（README / 文档 / 脚手架）。
6. 用操作系统语言回复用户。

## 多步任务纪律

≥2 步的任务先用 TodoWrite 写清单，同一时刻一个「进行中」，完成即标记，不批量。

## 反模式（阻断）

- 不建万能文件（`utils.ts` / `helpers.ts`）。
- 代码 / 注释不用 emoji、不用 AI 填充词、不留死代码与空 catch。
- 未经请求不创建文件。

## 提交前自检

复读每个改动文件；grep 调用方确认未破坏接口；有测试则运行测试；清理未用的导入 / 变量。

## 权限基线（见 settings.json）

- 默认放行读写与安全 git / 构建 / 测试命令。
- `rm` / `git push` / `git reset` / `sudo` / `docker` 等破坏性命令设为 `ask`。
- `.env`、密钥、证书文件 `deny`，禁止读取。
