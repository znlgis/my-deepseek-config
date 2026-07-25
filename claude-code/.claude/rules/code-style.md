---
paths:
  - "**/*.{ts,tsx,js,jsx,mjs,cjs}"
  - "**/*.{py,rs,go,java}"
---

# 代码风格规则

本文件定义跨语言的代码风格约定。规则按优先级排列——越靠前越重要。

## 变量声明

- **优先 `const` 而非 `let`。** 如果变量不需要重新赋值，用 `const`。
  这既是文档（告诉读者「此值不变」），也是安全措施。
- **用三元表达式替代简单的 if-else 赋值：**

  ```typescript
  // 好
  const label = isEmpty ? '空' : data.name;

  // 避免
  let label;
  if (isEmpty) {
    label = '空';
  } else {
    label = data.name;
  }
  ```

- **如果三元表达式嵌套超过一层或表达式过长，改用提前返回。**

## 控制流

- **尽可能避免 `else`。** 用提前返回（early return）或卫语句（guard clause）
  来压平代码结构。扁平的代码更容易阅读和推理：

  ```typescript
  // 好：提前返回
  function process(data: Data | null): Result {
    if (!data) return { error: '无数据' };
    if (!data.valid) return { error: '数据无效' };
    return { value: transform(data) };
  }

  // 避免：嵌套 else
  function process(data: Data | null): Result {
    if (data) {
      if (data.valid) {
        return { value: transform(data) };
      } else {
        return { error: '数据无效' };
      }
    } else {
      return { error: '无数据' };
    }
  }
  ```

- **避免深层嵌套。** 如果缩进超过 3 层，考虑提取函数或使用提前返回。

## 数据变换

- **优先函数式数组方法**（`map`、`filter`、`flatMap`、`reduce`）而非命令式 `for` 循环：

  ```typescript
  // 好
  const names = users.filter(u => u.active).map(u => u.name);

  // 避免
  const names = [];
  for (let i = 0; i < users.length; i++) {
    if (users[i].active) {
      names.push(users[i].name);
    }
  }
  ```

- **`reduce` 仅用于真正需要累积的场合。**
  如果现有的专用方法（`map` + `filter`、`some`、`every`、`find`）能表达意图，就用它们。

## 变量与内联

- **减少变量数量。** 如果一个值只用一次且表达式足够简洁，直接内联到使用点：

  ```typescript
  // 好
  return computeResult(await fetchData(id));

  // 避免（无意义命名）
  const data = await fetchData(id);
  const result = computeResult(data);
  return result;
  ```

- **但不要牺牲可读性。** 如果表达式过长或含义不直观，保留命名变量以增强可读性。

## 解构

- **避免不必要的解构。** 当解构名不能增加清晰度时，直接用点号访问：

  ```typescript
  // 好
  console.log(config.timeout);

  // 无意义解构
  const { timeout } = config;
  console.log(timeout);
  ```

- **解构在以下场景是好的：**
  - 需要提取多个属性时（减少 `obj.` 重复）
  - 函数参数声明中需要提供默认值
  - 重命名以解决命名冲突

## 导入

- **不用通配符导入：**

  ```typescript
  // 好的
  import { useState, useEffect } from 'react';

  // 禁止
  import * as React from 'react';
  ```

- **不用导入别名**，除非解决真正的命名冲突：

  ```typescript
  // 仅在命名冲突时需要
  import { Button as AntButton } from 'antd';
  import { Button as MyButton } from './components';
  ```

## 函数提取

- **不要过早提取只用一次的函数。** 如果一块逻辑只在一个地方使用且不长，
  保持内联。过早提取分散逻辑，增加跳转成本：

  ```typescript
  // 好：逻辑内聚
  function submit(form: FormData) {
    const validated = { ...form, email: form.email.trim().toLowerCase() };
    return api.post('/submit', validated);
  }

  // 避免：无意义提取
  function normalizeEmail(email: string) { return email.trim().toLowerCase(); }
  function validate(form: FormData) { return { ...form, email: normalizeEmail(form.email) }; }
  function submit(form: FormData) { return api.post('/submit', validate(form)); }
  ```

- **提取函数的标准：**
  - 被多处调用（≥2 个调用点）
  - 逻辑复杂到需要独立测试
  - 有明确的语义边界（「这是一个独立的概念」）

## 错误处理

- **错误处理要明确。** 不要吞掉异常而不处理：

  ```typescript
  // 禁止：空 catch
  try {
    riskyOperation();
  } catch (e) {
    // 如果错误确实可以忽略，必须注释说明原因
    // 例如：非关键日志写入失败不影响主流程
  }
  ```

- **优先用返回错误值**（Result 类型 / null / ErrorCode）而非 `try/catch` 处理可预期的失败路径。
