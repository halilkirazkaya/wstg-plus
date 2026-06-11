## WSTG-ATHN-04 — 绕过身份验证机制测试

绕过身份验证机制 (Bypassing Authentication Schema) 涉及识别和利用应用程序逻辑中的缺陷，这些缺陷允许攻击者在不提供有效凭证 (Credentials) 的情况下访问受保护的资源。这种漏洞通常发生在应用程序依赖客户端控制 (Client-side controls)、未能对每个请求强制执行服务端授权 (Server-side authorization)，或者在生产环境中暴露了管理“后门” (Backdoors) 和调试端点 (Debug endpoints) 的情况下。攻击者通过对敏感 URL 进行强制浏览 (Forced browsing)、操纵诸如 `authenticated=true` 之类的 HTTP 参数 (HTTP parameters) 或伪造请求头 (Spoofing headers) 来诱导应用程序认为会话 (Session) 已经建立，从而利用这些弱点。成功的利用会导致身份验证周界的全面瓦解，可能导致未经授权的数据外泄 (Data exfiltration) 或整个系统受损。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **测试状态** | 未执行 |
| **严重程度** | 严重 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**工具：** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### 是否可以在没有活动会话的情况下直接访问敏感内部页面？
- [ ] 否 — 直接访问会导致重定向至登录页面或返回 403/404 响应  
- [ ] 是 — 某些敏感页面可以访问，但提供的数据有限或不敏感  
- [ ] 是 — 敏感的管理页面或用户特定页面在没有身份验证的情况下**完全**可以访问 *(严重)*  

### 应用程序是否依赖客户端标志或参数来确定身份验证状态？
- [ ] 否 — 身份验证状态严格通过服务端会话状态进行管理  
- [ ] 是 — 存在客户端标志，但修改标志**不会**授予对受保护区域的访问权限  
- [ ] 是 — 修改参数（例如 `is_authenticated=true`, `user_role=admin`）**允许**绕过身份验证  

### 是否可以通过伪造或篡改特定的 HTTP 请求头来绕过身份验证？
- [ ] 否 — 诸如 `X-Forwarded-For`、`Referer` 或自定义请求头对身份验证没有影响  
- [ ] 是 — 篡改请求头（例如 `X-Custom-IP-Authorization`, `X-Remote-User`）**是可能的**，并能授予未经授权的访问  

### 是否存在规避标准登录流程的其他路径或开发工件 (Development Artifacts)？
- [ ] 否 — 所有受保护资源都由集中的、生产就绪的身份验证检查进行管控  
- [ ] 是 — 遗留端点、调试路由（例如 `/debug/login`）或遗忘的 API 版本在没有凭证的情况下**可以**访问  

### 应用程序是否未能针对高权限操作进行重新验证或校验会话？
- [ ] 否 — 高权限或敏感操作需要新的或有效的会话令牌  
- [ ] 是 — 一旦在某个端点发现绕过漏洞，就**可以**利用它在整个应用程序中执行管理操作  

---