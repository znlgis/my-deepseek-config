---
name: security-review
description: 合并前对代码变更进行安全漏洞审计。在审查 diff / PR、加固代码，或任务提到安全、注入、XSS、SSRF、密钥、认证、反序列化、路径遍历时使用。提供具体清单与汇报格式；永不静默自动修复。
---

# 安全审查 (Security Review)

针对变更集捕获真实漏洞的聚焦检查清单。先审查 **diff**（实际变更行及其调用的代码），仅在发现指向别处时才扩大范围。只汇报发现；不修改代码，除非明确要求。

## 威胁清单（12 项）

### 1. 注入 (Injection)
不可信输入流入 SQL、Shell、OS 命令、eval、模板引擎或 LDAP。寻找字符串拼接而非参数化查询 / 参数数组。
- **检查点**：`exec()`、`eval()`、`spawn()`、模板字面量拼接 SQL / 命令、`shell: true`

### 2. XSS（跨站脚本）
用户数据显示到 HTML / JS / DOM 中未转义。检查 `innerHTML`、`dangerouslySetInnerHTML`、未转义的模板插值。
- **检查点**：`v-html`、`innerHTML`、`insertAdjacentHTML`、`document.write`、直接操作 DOM

### 3. 认证与授权 (AuthN / AuthZ)
缺少认证、缺少所有权/权限检查、IDOR（按 id 操作对象而不验证调用者持有它）、权限提升、信任客户端提供的角色。
- **检查点**：路由未加 middleware、参数中传递用户 ID 而未校验、信任请求体中的 `role` 字段

### 4. 密钥泄露 (Secrets)
硬编码的 API Key、Token、密码、私钥或连接串；密钥被日志记录或提交。应从 env / 密钥管理器获取，绝不写入源码。
- **检查点**：`sk-`、`x-api-key`、`password`、`secret`、`token`、JWT 密钥、数据库连接串

### 5. 路径遍历 / 文件访问 (Path Traversal)
用户可控路径未经规范化 / 白名单直接拼接（`../` 逃逸、符号链接跟踪）。
- **检查点**：`path.join(userInput, ...)`、`fs.readFile(req.query.file)`、文件下载未限定目录

### 6. SSRF（服务端请求伪造）
向用户提供的 URL / 主机发起服务端请求未做白名单校验；可能访问内部元数据端点。
- **检查点**：`fetch(userUrl)`、`axios.get(userInput)`、Webhook URL 未校验

### 7. 不安全的反序列化 (Deserialization)
不可信数据喂给不安全反序列化器（pickle、原生 YAML loader、Java/PHP 对象反序列化）。
- **检查点**：`JSON.parse` 本身安全，但 `eval()` 解析 JSON、`yaml.load`（非 `safeLoad`）、`pickle.loads`

### 8. 加密缺陷 (Crypto)
弱/自动选择算法（密码用 MD5/SHA1）、硬编码 IV、ECB 模式、缺少 TLS 验证、Token 用可预测随机数。
- **检查点**：`Math.random()` 用于 Token、`createHash('md5')` 用于密码、`rejectUnauthorized: false`

### 9. 敏感数据暴露
PII / 密钥出现在日志、错误消息或 API 响应中；详细堆栈跟踪泄露给客户端。
- **检查点**：`console.log(user)`、错误响应中直接返回 `err.message`、`res.status(500).json({ error: err.stack })`

### 10. 依赖安全 (Dependencies)
新增包：是否可信、是否锁定版本、是否有已知 CVE？警惕拼写欺骗。
- **检查点**：`npm install` 陌生包、`pip install` 未锁定版本、Dockerfile 中 `latest` 标签

### 11. 资源与拒绝服务 (Resource & DoS)
无界循环、无界请求/响应体大小、缺少超时、正则灾难性回溯 (ReDoS)。
- **检查点**：`while(true)`、正则 `/(a+)+b/` 模式、`timeout: 0`、无 `maxBodySize` 限制

### 12. 竞态条件 / TOCTOU
对文件、余额或认证状态的检查-然后-操作，未加锁。
- **检查点**：`fs.existsSync` 后 `fs.writeFile`、数据库"读取余额 → 判断 → 扣款"非原子

## 审查方法

1. **识别信任边界**：不可信输入从哪里进入？到达了哪些汇聚点（DB、Shell、文件系统、网络、HTML）？
2. **追踪污染路径**：从源头到汇聚点追踪每个污点值。当它在缺少验证/编码/参数化的情况下到达危险汇聚点时，漏洞成立。
3. **偏好白名单**而非黑名单；偏好库提供的转义与参数化而非手动消毒。
4. **仅标记能用来证明漏洞确实存在的发现**——不做推测性、风格化干扰。

## 输出格式

每项发现按以下格式：

```
[severity: high|medium|low] <标题>
location: path/to/file.ext:LINE
issue: <出了什么问题，以及触发它的输入>
impact: <攻击者获得了什么>
fix: <最小化的具体修复方案>
```

按严重度降序排列。**如果审查后没有可行动的发现，直说没有**——不填充低价值条目。

## 严重度参考

- **high**：可直接利用的漏洞，导致数据泄露、服务器接管或认证绕过。必须修复。
- **medium**：利用有前提条件（需认证、特定配置），但仍构成实际风险。
- **low**：安全硬化的最佳实践偏差点，无直接利用路径。

## 规则

- 只审查这次变更引入或修改的代码行，不标记既有问题（可备注但不算本次审查发现）。
- 每条发现必须有具体位置与利用路径——不凭空猜测。
- 永远不静默自动修复安全问题。汇报发现并建议修复，执行留给用户。
