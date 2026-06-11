## WSTG-CLNT-10 — 测试 WebSocket (Testing WebSockets)

WebSocket 测试侧重于客户端与服务器之间建立的全双工通信通道 (Full-duplex communication channel)，该通道绕过了传统的 HTTP 请求-响应模式。渗透测试人员 (Pentesters) 评估握手 (Handshake) 过程的安全性、跨站 WebSocket 劫持 (Cross-Site WebSocket Hijacking, CSWSH) 防护措施的存在，以及对消息有效载荷 (Payload) 实施的输入验证的严格程度。漏洞利用通常涉及劫持活动会话以实时窃取数据，或通过持久套接字注入恶意有效载荷，从而触发命令注入 (Command Injection) 或 SQL 注入 (SQLi) 等服务器端漏洞。由于许多传统的 Web 应用防火墙 (Web Application Firewalls, WAF) 无法有效检测 WebSocket 流量，该协议通常为绕过边界防御提供了一个隐蔽的矢量 (Vector)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**工具：** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### WebSocket 连接是否通过加密通道建立？
- [ ] 是 — 对所有连接强制执行 `wss://` (WebSocket Secure)  
- [ ] 否 — 使用了 `ws://`，允许中间人 (Person-in-the-middle, PITM) 窃听  

### 应用程序是否针对跨站 WebSocket 劫持 (CSWSH) 进行了保护？
- [ ] 否 — 服务器在握手期间严格验证 `Origin` 标头  
- [ ] 是 — `Origin` 标头验证薄弱，或者**可以**通过 null 或伪造的源 (Origin) 进行绕过  
- [ ] 是 — 未执行 `Origin` 标头验证，允许跨站攻击者发起连接 *（严重）*  

### 是否对 WebSocket 消息有效载荷 (Payload) 应用了输入验证和清理 (Sanitization)？
- [ ] 是 — 所有传入消息均经过清理，并根据模式 (Schema) 进行了验证  
- [ ] 是 — 存在部分验证，但**可能**使用编码或非标准有效载荷进行绕过  
- [ ] 否 — 消息被直接处理，导致注入（XSS, SQLi, RCE）或逻辑篡改  

### 是否对每个 WebSocket 消息都执行了身份验证 (Authentication) 和授权 (Authorization) 检查？
- [ ] 是 — 对每条消息都验证了会话，或协议使用令牌 (Token) 实现无状态化  
- [ ] 是 — 在握手时进行了身份验证，但**未**针对每条消息强制执行特定操作的授权  
- [ ] 否 — 仅在握手时检查身份验证，导致会话劫持后可获得完整访问权限  

### 服务器是否对 WebSocket 实施了速率限制 (Rate Limiting) 或资源约束？
- [ ] 是 — **已启用**并强制执行速率限制和最大消息大小  
- [ ] 否 — 不存在任何限制，允许通过消息泛洪或超大有效载荷导致拒绝服务 (Denial of Service, DoS) 攻击  

---