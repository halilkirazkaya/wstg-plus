## WSTG-CONF-06 — 测试 HTTP 方法

测试 HTTP 方法 (HTTP Methods) 涉及识别 Web 服务器和应用程序支持哪些谓词 (Verbs)，以确保仅暴露必要的功能。除了标准的 `GET` 和 `POST` 之外，服务器可能会无意中启用危险的方法，如 `PUT`、`DELETE` 或 `TRACE`，这可能允许攻击者上传恶意文件、删除现有内容或通过跨站追踪 (Cross-Site Tracing (XST)) 窃取会话 Cookie (Session Cookies)。渗透测试人员 (Pentesters) 通过检查诸如 `Allow` 或 `Public` 之类的响应头，并尝试使用方法覆盖 (Method Overriding) 技术或谓词篡改 (Verb Tampering) 来绕过限制，从而访问未经授权的功能。此项测试对于巩固攻击面 (Attack Surface) 以及防止可能导致服务器沦陷或未经授权数据篡改的配置错误至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中* |

> *如果 `PUT` 或 `DELETE` 允许未经授权的文件操作，或者 `TRACE` 促成了会话令牌 (Session Token) 窃取，严重程度将变为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### 整个应用程序是否禁用了 `PUT` 或 `DELETE` 等危险的 HTTP 方法？
- [ ] 是 — 仅启用了安全的方法 (GET, POST, HEAD)  
- [ ] 否 — 启用了 `PUT` 或 `DELETE` 但需要有效的身份验证 (Authentication)  
- [ ] 否 — 启用了 `PUT` 或 `DELETE` 且无需身份验证即可访问 *(紧急/Critical)*  

### 是否禁用了 `TRACE` 方法以防止跨站追踪 (XST)？
- [ ] 是 — `TRACE` 方法已禁用或返回 405 Method Not Allowed  
- [ ] 否 — `TRACE` 方法已启用，但不会反射敏感响应头  
- [ ] 否 — `TRACE` 方法已启用，且会反射 `Cookie` 或 `Authorization` 响应头 *(中危)*  

### 服务器是否支持 HTTP 方法覆盖 (HTTP Method Overriding)（例如 `X-HTTP-Method-Override`）？
- [ ] 否 — 服务器不处理方法覆盖响应头  
- [ ] 是 — 服务器处理覆盖响应头，但强制执行访问控制 (Access Controls)  
- [ ] 是 — 服务器处理覆盖响应头，且可能绕过访问控制  

### 服务器对任意或畸形的 HTTP 方法响应是否安全？
- [ ] 是 — 服务器返回 405 Method Not Allowed 或 501 Not Implemented  
- [ ] 否 — 对于诸如 `JEFF` 或 `TEST` 之类的任意方法，服务器返回 200 OK 或 500 Error  
- [ ] 否 — 使用任意方法允许绕过身份验证或授权 (Authorization) 过滤器  

### `OPTIONS` 方法是否受到限制，或配置为最小化信息泄露？
- [ ] 是 — `OPTIONS` 已禁用或返回极简的 `Allow` 响应头  
- [ ] 否 — `OPTIONS` 泄露了广泛的已启用方法，包括 DAV 或调试谓词  

---