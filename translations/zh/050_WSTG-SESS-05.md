## WSTG-SESS-05 — 跨站请求伪造测试

跨站请求伪造 (CSRF) 是一种漏洞，攻击者借此诱导受害者的浏览器在受害者当前已登录的另一个网站上执行非预期的操作。此漏洞利用了浏览器自动将“环境凭证”（如会话 Cookie (Session cookies) 或授权标头 (Authorization headers)）附加到传出请求的行为。攻击者通常通过在第三方网站上托管恶意脚本或隐藏表单，针对更改状态的操作（如更改密码、更新电子邮件地址或执行金融转账）发起攻击。成功的漏洞利用 (Exploit) 可能导致在用户不知情或未经同意的情况下实现全账户接管 (Account Takeover) 或未经授权的数据修改。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果受漏洞影响的操作允许账户接管、权限提升 (Privilege Escalation) 或未经授权的金融交易，严重程度将变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**工具：** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### 更改状态的请求是否实现了抗 CSRF 令牌 (Anti-CSRF tokens)？
- [ ] 是 — 所有更改状态的操作都需要唯一的、密码学强度高的令牌  
- [ ] 是 — 存在令牌，但每个会话**不是**唯一的，或者令牌是可预测的  
- [ ] 否 — **未**实现抗 CSRF 令牌  

### 服务器端对抗 CSRF 令牌的校验是否健壮？
- [ ] 是 — 服务器严格校验令牌，**无法**绕过  
- [ ] 是 — 执行了校验，但**可以**通过删除令牌参数来绕过  
- [ ] 是 — 执行了校验，但**可以**通过提供空白或伪造令牌来绕过  
- [ ] 是 — 执行了校验，但令牌**未**与用户会话绑定  

### 应用程序是否依赖于易被绕过的 CSRF 防护方法？
- [ ] 否 — 应用程序不单纯依赖弱标头或来源检查  
- [ ] 是 — 防护完全依赖于 `Referer` 或 `Origin` 标头，这些标头**可以**被伪造或剥离  
- [ ] 是 — 防护依赖于检查 `X-Requested-With` 标头，这**可以**通过跨源资源共享 (CORS) 配置错误来绕过  

### 会话 Cookie 是否配置了 `SameSite` 属性？
- [ ] 是 — 所有与会话相关的 Cookie 均将 `SameSite` 设置为 `Strict` 或 `Lax`  
- [ ] 否 — **缺失** `SameSite` 属性，默认为浏览器特定行为  
- [ ] 否 — `SameSite` 被显式设置为 `None`，且未配置 `Secure` 标志或额外的缓解措施  

### 是否可以通过更改 HTTP 请求方法来绕过 CSRF 防护？
- [ ] 否 — 无论使用何种 HTTP 方法，防护均强制执行  
- [ ] 是 — 令牌仅在 `POST` 请求上校验，但该操作**可以**通过 `GET` 执行  
- [ ] 是 — 更改方法（例如更改为 `PUT` 或 `DELETE`）会绕过令牌检查  

---