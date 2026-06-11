## WSTG-INPV-01 — 反射型跨站脚本攻击 (Reflected Cross Site Scripting, XSS)

反射型跨站脚本攻击 (Reflected Cross Site Scripting, XSS) 发生在应用程序在未经充分验证或编码的情况下，将不可信的数据包含在 HTTP 响应中，导致有效负载 (Payload) 在受害者的浏览器上下文中执行。攻击者通过社会工程学 (Social engineering) 手段，通常是通过精心构造的 URL 或表单发送恶意 Payload，以劫持用户会话 (User sessions)、窃取敏感 Cookie 或代表用户执行未经授权的操作。这种漏洞常见于搜索参数、错误消息以及任何将输入直接反射回用户界面的端点 (Endpoint)。漏洞利用依赖于受害者与恶意链接的交互，这使其成为一种非持久性但针对特定管理员或已认证用户非常有效的攻击向量。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **测试状态** | 未执行 |
| **严重程度** | 高 (High) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**工具：** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### 用户提供的输入是否在响应正文中反射？
- [ ] 否 — 输入**从未**反射回用户  
- [ ] 是 — 输入已反射，但经过正确编码，无法绕过 (Bypass)  
- [ ] 是 — 输入在**没有任何**编码或清理 (Sanitization) 的情况下被反射  

### 是否实施了上下文感知的输出编码 (Context-aware output encoding)？
- [ ] 是 — 根据具体的反射上下文**应用**了正确的编码（HTML、属性、JavaScript）  
- [ ] 是 — 已**应用**编码，但对于特定上下文而言不足（例如，在 `<script>` 标签内使用 HTML 编码）  
- [ ] 否 — **未应用**编码  

### 输入验证或 Web 应用防火墙 (WAF) 过滤器是否可以被绕过？
- [ ] 否 — 过滤器有效地拦截了所有常见及混淆的 XSS Payload  
- [ ] 是 — 存在过滤器，但使用字符编码、非标准标签或多语义负载 (Polyglots) **可以实现绕过**  
- [ ] 否 — 未部署任何过滤器或验证机制  

### 反射输入的执行上下文是什么？
- [ ] 安全 — 输入在不可执行的位置反射（例如，在标准的 `<div>` 或 `<span>` 标签内）  
- [ ] 风险 — 输入在 HTML 属性内部或标签的 `src`/`href` 属性中反射  
- [ ] 关键 — 输入直接在 `<script>` 块、事件句柄 (Event handlers) 或模板字面量 (Template literals) 中反射  

### 是否存在现代浏览器安全响应头以减轻 XSS 影响？
- [ ] 是 — 已**启用** `Content-Security-Policy` (CSP)，并配有严格的 script-src  
- [ ] 是 — 已**启用** `Content-Security-Policy` (CSP)，但包含 `unsafe-inline` 或弱白名单  
- [ ] 否 — 未提供 CSP 或过时的 `X-XSS-Protection` 响应头  

---