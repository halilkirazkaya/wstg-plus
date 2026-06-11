## WSTG-INPV-03 — HTTP 谓词篡改 (HTTP Verb Tampering) 测试

HTTP 谓词篡改 (HTTP Verb Tampering) 利用了 Web 服务器和应用程序框架在根据使用的 HTTP 方法授权访问特定资源时的缺陷。攻击者尝试通过使用 `HEAD`、`PUT`、`OPTIONS` 甚至后端可能处理不一致的任意非标准字符串来替换 `GET` 或 `POST` 等标准方法，从而绕过安全限制。这种漏洞通常发生在安全配置（例如 Java EE web.xml 过滤器或 .NET 授权规则）明确列出了允许的方法但未能考虑其他方法时，或者使用了“按方法拒绝”而不是“拒绝所有”的方法时。成功的漏洞利用可能导致对管理功能的未经授权访问、数据修改或有关服务器内部配置的信息泄露。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果可以在没有身份验证 (Authentication) 的情况下执行管理功能或数据修改 (PUT/DELETE)，严重程度将变为高。

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### 应用程序是否响应非标准或任意 HTTP 方法？
- [ ] 否 — 应用程序拒绝除明确要求的方法之外的所有方法  
- [ ] 是 — 应用程序响应标准方法 (OPTIONS, TRACE)，但**无法**绕过安全控制  
- [ ] 是 — 应用程序接受任意字符串（例如 FOO, TEST）并将其作为 `GET` 或 `POST` 请求进行处理  

### 是否可以通过更改 HTTP 方法来绕过身份验证/授权 (Authorization)？
- [ ] 否 — 无论 HTTP 谓词如何，安全控制都会全局应用  
- [ ] 是 — 已设置控制措施，但使用 `HEAD` 方法**可能**实现绕过  
- [ ] 是 — 安全控制**未应用**于替代方法，允许对受限端点进行未经授权的访问  

### 服务器上是否启用了 `PUT` 或 `DELETE` 等危险方法？
- [ ] 否 — 危险方法已**禁用**或返回 405 Method Not Allowed  
- [ ] 是 — 已启用方法，但需要有效的、高权限的身份验证  
- [ ] 是 — 方法已**启用**，并允许未经授权的文件上传或资源删除 *(紧急 - Critical)*  

### `OPTIONS` 方法是否泄露了关于允许谓词的敏感信息？
- [ ] 否 — `OPTIONS` 已**禁用**或返回通用响应  
- [ ] 是 — `OPTIONS` 返回 `Allow` 标头，但不暴露受限的内部方法  
- [ ] 是 — `OPTIONS` 泄露了仅限内部或管理使用的方法，有助于进一步漏洞利用 (Exploit)  

### 是否启用了 `TRACE` 方法，从而可能导致跨站跟踪 (Cross-Site Tracing, XST)？
- [ ] 否 — `TRACE` 和 `TRACK` 方法已**禁用**  
- [ ] 是 — `TRACE` 已**启用**并回显 HTTP 标头，包括敏感的 Cookie 或令牌 (Token)  

---