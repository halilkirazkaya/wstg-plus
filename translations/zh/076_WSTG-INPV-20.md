## WSTG-INPV-20 — 批量赋值 (Mass Assignment)

批量赋值 (Mass Assignment)，也称为过度提交 (Overposting) 或参数的不安全反序列化 (Insecure Deserialization of parameters)，发生在 Web 应用程序在没有进行适当属性过滤的情况下，将传入的 HTTP 请求参数自动绑定到内部对象属性时。这种漏洞在现代 MVC 框架和 RESTful API 中非常普遍，其中 JSON、XML 或表单编码的数据被直接反序列化为应用程序端的数据模型。攻击者通过在请求中注入额外的、未列出的参数（例如 `isAdmin`、`role` 或 `account_balance`）来利用此行为，从而修改开发人员本意不希望暴露给用户控制的敏感字段。成功利用此漏洞通常会导致权限提升 (Privilege Escalation)、未授权的数据修改或绕过关键业务逻辑工作流。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 中* |

> *如果可以修改管理属性、权限级别或计费字段，则严重程度为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### 应用程序是否对传入的请求参数使用自动绑定？
- [ ] 否 — 参数被手动映射到特定的变量或安全对象  
- [ ] 是 — 使用了自动绑定，但强制执行了严格的**白名单**或数据传输对象 (DTO)  
- [ ] 是 — 使用了自动绑定，且敏感的内部属性**可以**被修改  

### 是否可以成功注入管理或权限相关的属性？
- [ ] 否 — 注入的参数被忽略、剥离或导致验证错误  
- [ ] 是 — 额外的参数被接受，但**不会**影响后端对象状态  
- [ ] 是 — 注入诸如 `is_admin`、`role` 或 `permissions` 等参数并**成功**提升权限 *(危急)*  

### 是否可以通过过度提交修改敏感的业务逻辑或财务字段？
- [ ] 否 — `price`、`balance` 或 `status` 等字段已受到保护，防止批量赋值  
- [ ] 是 — 通过将敏感业务属性添加到 JSON 或表单请求体中，**可以**对其进行修改  

### 应用程序是否泄露了有助于参数猜测的内部对象结构？
- [ ] 否 — 响应仅包含预期的公共字段，且文档受到限制  
- [ ] 是 — 内部对象字段在 GET 响应中可见，使参数发现成为**可能**  
- [ ] 是 — API 文档（Swagger/OpenAPI）明确列出了**可以**被赋值的内部属性  

---