---
name: conventional-commits
description: 按 Conventional Commits 标准撰写提交信息和 PR 标题。用于提交变更、撰写提交信息、压缩提交或命名 PR 时选择正确的 type（feat/fix/docs/...）、scope、breaking-change 标记和祈使语气主题行。
license: MIT
compatibility: kimi-cli
metadata:
  audience: developers
  workflow: git
---

# Conventional Commits

按 Conventional Commits 格式生成提交信息和 PR 标题，保持历史机器可读、变更日志/版本更新可自动化。

规范：https://www.conventionalcommits.org

## 格式

```
<type>(<可选 scope>): <祈使语气主题>

<可选正文 — 变更内容及原因，约 72 字符折行>

<可选脚注 — BREAKING CHANGE:、Closes #123、Co-authored-by:>
```

## 类型

| 类型       | 用途                   | SemVer |
| ---------- | ---------------------- | ------ |
| `feat`     | 新的用户可见功能       | minor  |
| `fix`      | 修复 bug              | patch  |
| `docs`     | 仅文档变更             | —      |
| `style`    | 格式化/空白，无行为变更 | —      |
| `refactor` | 既不修 bug 也不加功能的代码变更 | — |
| `perf`     | 性能改进               | patch  |
| `test`     | 添加或修复测试         | —      |
| `build`    | 构建系统或依赖         | —      |
| `ci`       | CI 配置和脚本          | —      |
| `chore`    | 不涉及 src 或 test 的维护 | —    |
| `revert`   | 回退之前的提交         | —      |

## 规则

1. **主题用祈使语气**："添加"而非"添加了"/"添加的"。
2. **不以句号结尾**；主题尽量 ≤ 50 字符。
3. **scope 可选**，命名影响区域：`feat(parser): ...`。
4. **破坏性变更**在 type/scope 后标 `!` **和/或**用 `BREAKING CHANGE:` 脚注说明迁移方法。
5. **每次提交一个逻辑变更。** 不相关的工作分开提交。
6. 在脚注中引用 issue（`Closes #42`），不在主题中。

## 示例

```text
feat(auth): add OAuth2 device-code login flow

fix(api): guard against null user before serializing

docs: clarify skill auto-discovery paths in README

refactor(orchestrator)!: drop legacy qwen routing table

BREAKING CHANGE: the `qwen3.7-max` route is removed; callers must
use the deepseek-v4 tiers instead.
```

## 不过度设计的情况

对于极小的单文件调整，一行 `type: subject` 即可——正文仅在"原因"无法从 diff 明显看出时才需要。如果仓库已有不同但一致的约定，遵循现有风格。

## 检查现有约定

在提出格式前，检查仓库最近的提交历史：

```bash
git log --oneline -20
```
