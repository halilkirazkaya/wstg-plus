## WSTG-CLNT-11 — 测试网页消息传递 (Test Web Messaging)

网页消息传递 (Web Messaging) 或 `postMessage` 能够实现窗口对象之间的跨源通信 (cross-origin communication)，例如父页面与生成的弹出窗口或嵌入的内嵌框架 (iframe) 之间的通信。该机制对于现代 Web 功能至关重要，但如果接收端应用程序未能验证发送者的身份或未正确处理消息有效负载 (Payload)，则会引入重大风险。攻击者通过托管一个嵌入目标应用程序的恶意网站，并发送精心构造的消息来触发非预期的操作、窃取敏感数据或执行基于 DOM 的跨站脚本攻击 (DOM-based Cross-Site Scripting, XSS)。当消息监听器没有严格验证 `origin` 属性，或者将 `message.data` 直接传递到危险的执行接收器 (execution sinks) 时，就会发生成功的攻击。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **测试状态** | 未执行 |
| **风险等级** | 中 / 高 (Medium / High) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**工具：** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### 应用程序是否利用 `postMessage` API 进行跨文档通信？
- [ ] 否 — 客户端代码中**不**存在 `postMessage` 监听器  
- [ ] 是 — `postMessage` 用于内部组件通信  
- [ ] 是 — `postMessage` 用于与第三方域名或挂件 (widgets) 进行通信  

### 传入消息的源 (origin) 是否经过严格验证？
- [ ] 是 — 使用 `===` 或恒等比较，根据严格的白名单检查源 *(最安全)*  
- [ ] 是 — 使用正则表达式验证源，但逻辑**容易**被绕过 (例如：`^https://trusted.com`)  
- [ ] 否 — **未**检查 `origin` 属性，允许任何域名发送消息  
- [ ] 否 — 发送者的 `targetOrigin` 中使用了通配符 `*`，可能导致数据泄露给恶意监听器  

### 消息有效负载 (Payload) 在被应用程序处理前是否经过清理？
- [ ] 是 — 数据被视为纯文本处理，且未使用危险的接收器 (sinks)  
- [ ] 是 — 数据在使用前经过解析 (例如 `JSON.parse`) 和验证，且**无法**绕过  
- [ ] 否 — 消息数据被直接传递到危险的接收器，如 `innerHTML`、`eval()` 或 `setTimeout()`  
- [ ] 否 — 消息数据在未经验证的情况下被用于窗口导航或更改 `location.href`  

### 攻击者是否可以通过恶意消息触发未授权的操作或窃取数据？
- [ ] 否 — 即使是伪造的消息，也无法访问敏感操作或数据  
- [ ] 是 — 精心构造的消息**可以**触发敏感的状态更改 (例如：修改密码、更新个人资料)  
- [ ] 是 — 精心构造的消息**可以**导致基于 DOM 的 XSS 或窃取 CSRF 令牌/会话数据 (session data)  

---