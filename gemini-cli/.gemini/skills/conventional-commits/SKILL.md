---
name: conventional-commits
description: 编写符合 Conventional Commits 规范的提交信息与 PR 标题。在提交代码、拟写 commit message、squash 或命名 PR 时使用。提供正确的 type、scope、breaking-change 标记和命令式主题行。
---

# Conventional Commits

生成符合 Conventional Commits 格式的提交信息与 PR 标题，使历史可机器读取、changelog 与版本号自动化。
规范：https://www.conventionalcommits.org

## 格式

```
<type>(<可选 scope>): <命令式主题>

<可选正文——改了什么以及为什么，约 72 列换行>

<可选脚注——BREAKING CHANGE:、Closes #123、Co-authored-by:>
```

## 类型

| Type       | 用途                          | SemVer |
|------------|-------------------------------|--------|
| `feat`     | 新的面向用户的功能            | minor  |
| `fix`      | 修复 bug                      | patch  |
| `docs`     | 仅文档变更                    | —      |
| `style`    | 格式/空白，无行为变更         | —      |
| `refactor` | 既不修 bug 也不加功能的代码变更 | —      |
| `perf`     | 性能优化                      | patch  |
| `test`     | 添加或修复测试                | —      |
| `build`    | 构建系统或依赖                | —      |
| `ci`       | CI 配置与脚本                 | —      |
| `chore`    | 不涉及 src 或 test 的维护     | —      |
| `revert`   | 回滚之前的提交                | —      |

## 规则

1. **主题使用命令式语气**："添加"而非"已添加"。
2. **主题末尾不加句号**；尽量控制在 ~50 字符内。
3. **scope 可选**，命名影响区域：`feat(parser): ...`
4. **破坏性变更**在 type/scope 后加 `!`，**且/或**在脚注加 `BREAKING CHANGE:` 解释迁移方式。
5. **一次提交仅包含一个逻辑变更。**拆分不相关的工作。
6. 在脚注引用 issue（`Closes #42`），不在主题中。

## 示例

```text
feat(auth): 添加 OAuth2 设备码登录流程

fix(api): 序列化前防护 null 用户对象

docs: 在 README 中说明 Skill 自动发现路径

refactor(orchestrator)!: 删除旧版 qwen 路由表

BREAKING CHANGE: `qwen3.7-max` 路由已移除；调用方必须改用 deepseek-v4 层级。
```

## 何时不过度设计

对微小单文件修改，单行 `type: 主题` 足够——仅当 *为什么* 从 diff 看不出来时才加正文。如果仓库已有不同但一致的提交风格，遵循既有风格而非规范。

## 检查既有约定

提议格式前先检查仓库近期历史：

```bash
git log --oneline -20   # 查看实际使用的提交风格
```

如果仓库使用不同约定（如 `[fix] 主题` 或模块前缀如 `auth: 主题`），遵循既有风格——不必强推 Conventional Commits 规范。
