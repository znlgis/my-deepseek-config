---
name: conventional-commits
description: 按 Conventional Commits 规范编写提交信息和 PR 标题。提交代码、撰写提交信息、squash 或命名 PR 时使用，需要正确的 type(feat/fix/docs/...)、scope、breaking-change 标记及祈使语气的主题行。
paths: []
priority: low
---

# Conventional Commits

生成符合 Conventional Commits 格式的提交信息和 PR 标题，使历史保持机器可读，
变更日志和版本号升级可自动化。规范：https://www.conventionalcommits.org

## 格式

```
<type>(<可选 scope>): <祈使语气主题>

<可选正文 — 改了什么和为什么，约 72 列换行>

<可选脚注 — BREAKING CHANGE:、Closes #123、Co-authored-by:>
```

## 类型

| Type       | 用途                                | SemVer |
| ---------- | ----------------------------------- | ------ |
| `feat`     | 新的用户可见功能                     | minor  |
| `fix`      | bug 修复                            | patch  |
| `docs`     | 仅文档变更                          | —      |
| `style`    | 格式化/空白，无行为变更              | —      |
| `refactor` | 既不修 bug 也不加功能的代码变更      | —      |
| `perf`     | 性能优化                            | patch  |
| `test`     | 添加或修复测试                       | —      |
| `build`    | 构建系统或依赖                       | —      |
| `ci`       | CI 配置和脚本                       | —      |
| `chore`    | 不涉及 src 或 test 的维护           | —      |
| `revert`   | 回退一个提交                        | —      |

## 规则

1. **主题用祈使语气**："add"，不是 "added"/"adds"。
2. **主题行无末尾句号**；尽量保持 ≤ ~50 字符。
3. **scope 可选**，命名受影响区域：`feat(parser): ...`。
4. **破坏性变更**在类型/scope 后加 `!`，**和/或**加 `BREAKING CHANGE:` 脚注说明迁移方式。
5. **一个提交一个逻辑变更。** 不相关的工作分开提交。
6. 引用 issue 放脚注（`Closes #42`），不放主题行。

## 示例

```text
feat(auth): add OAuth2 device-code login flow

fix(api): guard against null user before serializing

docs: clarify skill auto-discovery paths in README

refactor(orchestrator)!: drop legacy qwen routing table

BREAKING CHANGE: the `qwen3.7-max` route is removed; callers must
use the deepseek-v4 tiers instead.
```

## 何时不过度工程化

对单文件的微小改动，一行 `type: subject` 足够——只有 diff 看不出"为什么"时才需要正文。
如果仓库已有不同但一致的提交风格，匹配既有风格。

## 检查现有约定

提议格式前检查仓库最近历史：

```bash
git log --oneline -20   # 看实际使用的提交风格
```

如果仓库用不同约定（如 `[fix] subject` 或模块前缀 `auth: subject`），遵循既有风格——
不遵循规范。
