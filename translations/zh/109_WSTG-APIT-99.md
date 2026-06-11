## WSTG-APIT-99 — GraphQL 测试

GraphQL 是一种用于 API 的查询语言 (Query Language)，它允许客户端请求特定的数据结构，但配置错误通常会导致严重的安全性风险，包括信息泄露 (Information Disclosure) 和资源耗尽 (Resource Exhaustion)。漏洞通常源于启用了内省 (Introspection)、缺乏查询深度/复杂度限制，以及解析器函数 (Resolver functions) 中的失效的对象级授权 (Broken Object-Level Authorization, BOLA)。攻击者利用这些端点 (Endpoints)，通过内省来映射整个模式 (Schema)，利用循环片段 (Circular fragments) 触发拒绝服务 (Denial of Service, DoS)，或者通过嵌套查询访问未经授权的字段来绕过授权。测试重点关注 `/graphql`、`/v1/graphql` 或 `/graphiql` 端点，以确保实现过程中强制执行了严格的访问控制和资源管理。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重性** | 高 / 严重* |

> *如果失效的对象级授权 (BOLA) 允许未经授权访问个人身份信息 (PII) 或管理变异 (Mutations)，严重性将变为“严重”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**工具：** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### GraphQL 内省系统是否已启用？
- [ ] 否 — 内省已**禁用**，无法映射模式  
- [ ] 是 — 内省已**启用**，但需要管理身份验证  
- [ ] 是 — 内省已**启用**且可公开访问 *(高)*  

### 查询是否强制执行资源限制（深度和复杂度）？
- [ ] 是 — **应用**并强制执行了严格的查询深度和复杂度限制  
- [ ] 是 — **应用**了限制，但可以使用别名 (Aliases) 或片段 (Fragments) 绕过  
- [ ] 否 — 未**应用**任何限制，允许递归查询和 DoS  

### 解析器中是否存在失效的对象级授权 (BOLA)？
- [ ] 否 — 针对每个查询都在字段和对象级别**应用**了授权  
- [ ] 是 — 针对部分字段**应用**了授权，但泄露了敏感对象  
- [ ] 是 — 未**应用**授权，允许通过 ID 篡改访问任何对象  

### GraphQL 变异 (Mutations) 是否得到了妥善保护，防止未经授权的使用？
- [ ] 否 — 模式中**不**存在变异  
- [ ] 是 — 变异要求有效的身份验证和严格的授权检查  
- [ ] 是 — 未身份验证的用户**可能**执行变异或缺乏授权  

### 错误信息是否泄露敏感的实现或模式细节？
- [ ] 否 — 错误信息是通用的，且**不**泄露内部逻辑  
- [ ] 是 — 错误信息通过“您是指...？”(Did you mean...?) 泄露了底层数据库类型或建议  
- [ ] 是 — 响应中**泄露**了完整的堆栈跟踪 (Stack traces) 和敏感配置数据  

---