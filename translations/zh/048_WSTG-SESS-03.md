## WSTG-SESS-03 — 会话固定 (Session Fixation)

会话固定 (Session Fixation) 发生在应用程序在用户成功通过身份验证 (Authentication) 后未能作废或轮换会话标识符 (Session Identifier) 的情况下，这允许攻击者将已知的会话令牌 (Session Token) 强制加载给受害者。如果应用程序在登录后保留了身份验证前的会话 ID (Session ID)，攻击者可以向受害者提供包含固定会话 ID 的特制链接，随后劫持已验证的会话。此漏洞通常表现在登录表单中，或者通过 URL 参数和 Cookie 接受的会话标识符。从攻击者的角度来看，利用过程包括通过恶意链接或响应头注入 (Header Injection) 漏洞“固定”会话，并等待受害者提供凭据，从而有效地绕过窃取活动令牌的需要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### 身份验证成功后，会话标识符是否发生变化？
- [ ] 是 — 已签发新的会话标识符，并且旧的标识符已作废 *(最安全)*  
- [ ] 是 — 已签发新的会话标识符，但旧的标识符仍然有效  
- [ ] 否 — 身份验证前后的会话标识符保持不变 *(严重)*  

### 应用程序是否接受通过 URL 参数提供的会话标识符？
- [ ] 否 — 会话 ID 仅通过 Cookie 进行管理  
- [ ] 是 — 应用程序接受 URL 中的会话 ID，但在登录时会将其忽略或轮换  
- [ ] 是 — URL 中的会话 ID 被接受，并在身份验证后继续保留  

### 攻击者定义的会话 ID 是否可以强制注入应用程序？
- [ ] 否 — 应用程序拒绝用户提供的无效或不存在的会话 ID  
- [ ] 是 — 应用程序接受并使用 Cookie 中提供的任意 ID 初始化会话  
- [ ] 是 — 应用程序接受并使用 URL 参数中提供的任意 ID 初始化会话  

### 身份验证前的会话是否已正确终止？
- [ ] 是 — 匿名会话在登录事件发生时被完全销毁  
- [ ] 否 — 匿名会话未被销毁，允许潜在的会话收集 (Session Harvesting)  

### 会话 Cookie 是否受到保护以防止客户端注入？
- [ ] 是 — 已应用 `HttpOnly` 和 `Secure` 标志，以防止基于脚本的会话固定  
- [ ] 否 — 未应用相关标志，允许通过跨站脚本攻击 (Cross-Site Scripting (XSS)) 设置会话 ID  

---