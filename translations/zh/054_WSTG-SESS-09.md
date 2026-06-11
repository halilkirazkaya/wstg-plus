## WSTG-SESS-09 — 会话劫持测试 (Testing for Session Hijacking)

会话劫持 (Session Hijacking) 发生在攻击者捕获、预测或固定有效的会话标识符 (SID) 以获得对用户活动会话的未经授权访问时。此漏洞通常表现为传输层安全不足、缺乏 Cookie 安全标志或可预测的 SID 生成算法，从而允许攻击者完全绕过身份验证 (Authentication)。从攻击者的角度来看，成功的利用可以在不需要受害者凭据的情况下完全冒充受害者，这通常通过网络嗅探、跨站脚本攻击 (XSS) 或会话固定 (Session Fixation) 攻击来实现。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **测试状态** | 未执行 |
| **严重程度** | 高 (High) / 严重 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**工具：** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### 会话标识符在传输过程中是否受到保护？
- [ ] 是 — `Secure` 标志已**启用**且 HTTP 严格传输安全 (HSTS) 已严格强制执行  
- [ ] 是 — `Secure` 标志已**启用**但 HSTS 已**禁用**  
- [ ] 否 — 未应用 `Secure` 标志，允许通过未加密通道拦截 SID *(高)*  

### 会话标识符是否防止了客户端脚本访问？
- [ ] 是 — 所有与会话相关的 Cookie 均已**应用** `HttpOnly` 标志  
- [ ] 否 — 未应用 `HttpOnly` 标志，允许通过 XSS 窃取 SID *(严重)*  

### 应用程序是否实现了与客户端属性的会话绑定？
- [ ] 是 — 会话已绑定至客户端属性（IP/User-Agent）且**无法**重复使用  
- [ ] 是 — 存在会话绑定，但可能通过请求头伪造 (Header Spoofing) 进行**绕过**  
- [ ] 否 — 不存在会话绑定；SID 从任何来源使用均**有效**  

### 会话标识符是否具有足够的随机性以防止预测？
- [ ] 是 — SID 使用加密安全伪随机数生成器 (CSPRNG)  
- [ ] 否 — SID 长度不足或遵循**可预测**的序列/模式  

### 是否对并发会话进行了管理以限制攻击窗口？
- [ ] 否 — 应用程序严格强制每个用户仅限一个活动会话  
- [ ] 是 — 多个并发会话已**启用**但受到监控  
- [ ] 是 — 跨不同设备/位置**可能**存在无限并发会话  

---