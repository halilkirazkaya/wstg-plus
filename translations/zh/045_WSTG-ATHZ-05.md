## WSTG-ATHZ-05 — OAuth 弱点测试 (Testing for OAuth Weaknesses)

OAuth 弱点涵盖了在实现 OAuth 2.0 或 OpenID Connect (OIDC) 协议时的各种缺陷，通常会导致完整的账户接管 (Account Takeover) 或未经授权的数据访问。这些漏洞通常表现在授权流 (Authorization Flow) 过程中，特别是涉及 `redirect_uri` 的验证、`state` 参数的熵 (Entropy) 及其验证，以及访问令牌 (Access Token) 或 ID 令牌 (ID Token) 的安全处理。攻击者通过拦截授权码 (Authorization Code)、通过开放重定向 (Open Redirect) 将令牌重定向到攻击者控制的域名，或利用跨站请求伪造 (CSRF) 将受害者的会话链接到攻击者的账户来利用这些弱点。由于 OAuth 通常作为主要的身份验证机制，单一的错误配置就可能损害整个用户身份系统的完整性。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 紧急 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**工具：** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### `state` 参数是否已实现并经过验证以防止 CSRF？
- [ ] 是 — `state` 是强制性的，每个会话唯一，并在服务器端经过严格验证  
- [ ] 是 — 存在 `state` 但在不同会话之间是静态的或可预测的  
- [ ] 是 — 存在 `state` 但应用程序在回调时**未**对其进行验证  
- [ ] 否 — 授权请求中**未使用** `state` 参数 *(紧急)*  

### `redirect_uri` 是否针对白名单 (Whitelist) 进行了严格验证？
- [ ] 是 — 仅接受与预注册白名单完全匹配的项  
- [ ] 是 — 应用了验证，但**可以通过**路径遍历 (Path Traversal) 或子域名操纵绕过  
- [ ] 是 — 应用了验证，但**可以通过**参数污染 (Parameter Pollution) 或片段注入 (Fragment Injection) 绕过  
- [ ] 否 — 应用程序**允许**在 `redirect_uri` 参数中使用任意 URL *(紧急)*  

### `response_type` 是否可以被操纵以改变流程？
- [ ] 否 — 应用程序仅接受预期的流（例如 `code`）并拒绝其他流  
- [ ] 是 — 从 `code` 切换到 `token`（隐式流/Implicit Flow）**是可能的**，并且会导致令牌在 URL 中泄露  
- [ ] 是 — 混合流 (Hybrid Flows) 或未经授权的 `response_type` 值被**启用**并处理  

### 访问令牌或授权码是否通过 Referer 标头或浏览器历史记录泄露？
- [ ] 否 — 令牌/授权码被安全处理，且敏感页面使用了适当的 `Referrer-Policy`  
- [ ] 是 — 令牌/授权码通过 `Referer` 标头泄露到第三方域名  
- [ ] 是 — 由于不安全的缓存或 GET 请求，令牌/授权码在浏览器历史记录中**可见**  

### 应用程序是否针对 OAuth 范围 (Scope) 执行“最小权限原则 (Least Privilege Principle)”？
- [ ] 是 — 应用程序仅请求功能所需的最小范围  
- [ ] 否 — 应用程序默认请求过度的或管理权限范围  
- [ ] 否 — 用户在同意阶段**无法**审查或选择退出特定的范围  

---