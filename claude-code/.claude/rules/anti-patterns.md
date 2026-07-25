---
paths:
  - "**/*"
---

# 反模式清单

以下行为在任何情况下都被严格禁止。违反任何一条都会在代码审查中被阻断。

## 1. 万能文件名

禁止创建无意义的通用文件名：

- `utils.ts` / `utils.js` / `utils.py`
- `helpers.ts` / `helpers.js` / `helpers.py`
- `service.ts` / `service.js`
- `common.ts` / `common.js`
- `misc.ts` / `misc.js`

这些文件名不传达任何信息——什么都可以往里塞。使用描述性文件名：

- `string-formatter.ts` 而非 `utils.ts`
- `date-parser.ts` 而非 `helpers.ts`
- `user-repository.ts` 而非 `service.ts`

## 2. Emoji

禁止在以下位置使用 emoji：

- 代码中的变量名、函数名、类名
- 代码注释（`// 🔧 修复了 bug`）
- 文档字符串
- Git commit message（标题行）

唯一的例外：用户明确要求使用 emoji。

## 3. AI 填充词

禁止在注释、解释或回复中使用以下无信息量的词语：

- simply / simply put
- obviously / clearly / apparently
- moreover / furthermore
- interestingly / notably
- "just"（作副词用时："just do X"）
- "basically" / "essentially"

这些词不增加信息量，反而暗示读者应该已经知道——这是糟糕的写作习惯。

## 4. 空 catch 块

```typescript
// 禁止
try {
  doSomething();
} catch (e) {}

// 也禁止
try {
  doSomething();
} catch (e) {
  // silently ignore
}
```

如果异常确实可以忽略，必须注释说明**为什么**可以安全忽略：

```typescript
try {
  analytics.track(event);
} catch (e) {
  // 遥测失败不影响用户主流程，且重试机制在 analytics 模块内部已实现
}
```

更好的做法是使用显式的错误处理而非 `try/catch`：

```typescript
const result = safeExecute(() => analytics.track(event));
if (!result.ok) logWarning('遥测发送失败', result.error);
```

## 5. 无注释的类型抑制

```typescript
// 禁止
// @ts-ignore
const value = getUntypedData();

// 禁止
// @ts-expect-error
callLegacyApi(args);
```

每次使用 `@ts-ignore` 或 `@ts-expect-error` 必须附带解释：

```typescript
// @ts-expect-error — legacyApi 的类型定义未覆盖第三个参数（backwardCompat），
// 等到 v3 中该 API 重构后可移除此抑制
callLegacyApi(args, backwardCompat);
```

## 6. 注释掉的代码

```typescript
// 禁止
// function oldImplementation() {
//   return fetch('/api/v1/users');
// }

// 也禁止
/*
function oldImplementation() {
  return fetch('/api/v1/users');
}
*/
```

死代码属于 git 历史。如果需要恢复旧代码，用 `git revert` 或 `git checkout`。
提交前确保没有遗留的注释代码块。

## 7. 未经请求创建文件

禁止在用户没有明确要求时创建以下文件：

- README.md / README 文件
- 任何 Markdown 文档（除非 task 明确要求）
- CHANGELOG.md / CONTRIBUTING.md
- .gitignore（除非新建仓库）
- 占位文件或 TODO 文件
- 任何新文件

**唯一例外：** 用户明确要求「创建 X 文件」或任务描述中明确包含文件创建步骤。

## 8. 未经请求的依赖变更

禁止在用户没有明确要求时：

- 安装新的 npm/pip/cargo/go 包
- 升级现有依赖版本
- 修改 `package.json`、`requirements.txt`、`Cargo.toml`、`go.mod` 中的依赖声明
