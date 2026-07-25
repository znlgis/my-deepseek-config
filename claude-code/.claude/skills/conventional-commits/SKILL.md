---
name: conventional-commits
description: 按 Conventional Commits 规范撰写 Git 提交信息。触发词：「commit」「提交」「提交信息」「commit message」。
allowed-tools:
  - Bash(git *)
---

# Conventional Commits

按 [Conventional Commits v1.0.0](https://www.conventionalcommits.org/) 规范撰写提交信息。

## 提交信息格式

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### type（类型）

| 类型 | 含义 | 示例 |
|------|------|------|
| `feat` | 新功能 | `feat: 添加用户登录功能` |
| `fix` | Bug 修复 | `fix: 修复列表页分页计数错误` |
| `docs` | 仅文档变更 | `docs: 更新 API 认证说明` |
| `style` | 格式调整（不影响代码逻辑） | `style: 统一缩进为 2 空格` |
| `refactor` | 重构（不改变行为） | `refactor: 提取用户校验逻辑` |
| `perf` | 性能优化 | `perf: 用 Map 替代 Object 减少查找开销` |
| `test` | 添加或修改测试 | `test: 补充订单模块单元测试` |
| `build` | 构建系统或外部依赖变更 | `build: 升级 webpack 到 v5` |
| `ci` | CI/CD 配置变更 | `ci: 添加 PR 自动部署 workflow` |
| `chore` | 其他杂项 | `chore: 更新 .gitignore` |

### scope（范围，可选）

标明改动的影响范围，使用项目中的模块名或目录名：

```
feat(auth): 支持 OAuth2 登录
fix(api): 修复分页参数丢失
refactor(db): 提取数据库连接池逻辑
```

### description（描述）

- 使用祈使句（"添加"而非"添加了"）
- 首字母小写（中文无此约束）
- 结尾不加句号
- 长度控制在 72 字符以内（中文约 50 字）

### body（正文，可选）

- 解释 WHAT 和 WHY，而非 HOW
- 每行不超过 72 字符
- 与 description 之间空一行

### footer（脚注，可选）

- **BREAKING CHANGE:** 破坏性变更说明
- **Closes / Fixes / Refs:** 关联的 Issue 号

## 完整示例

```
feat(auth): 支持 OAuth2 第三方登录

新增 Google 和 GitHub OAuth2 登录方式。用户可选择使用第三方账号
注册和登录，无需额外设置密码。

BREAKING CHANGE: 用户表中新增 `oauth_provider` 和 `oauth_id` 字段，
旧版 API 的 `/login` 端点返回格式有变化。

Closes #128
```

## 多文件提交策略

- **一个提交只做一件事。** 不要把无关的改动混在一个 commit 里。
- **逻辑上独立的改动分成多个 commit。** 重构和功能变更分别提交。
- **WIP 提交在 push 前 squash。** 保持主干历史干净。

## 检查清单

在输出提交信息前确认：

1. type 是否正确？
2. scope 是否准确（如果使用了）？
3. description 是否用祈使句、无句号？
4. 破坏性变更是否在 footer 中标明？
5. 关联的 Issue 是否引用？
