## WSTG-SESS-11 — 测试并发会话 (Testing for Concurrent Sessions)

并发会话 (Concurrent Sessions) 测试旨在评估应用程序是否允许单个用户帐户在不同的浏览器、设备或 IP 地址上维持多个同时进行的活动会话。如果未能限制并发会话，攻击者在利用被劫持的会话令牌 (Session Tokens) 或泄露的凭据 (Compromised Credentials) 时，可以在不提醒合法用户或不终止现有会话的情况下获得更长的攻击窗口。这种行为通常通过从多个来源进行身份验证并监控会话稳定性来评估，往往会暴露会话管理逻辑中的弱点或缺乏以安全为核心的业务规则。从攻击者的角度来看，缺乏并发控制允许在初始入侵后实现隐蔽的持久化 (Persistence)，并有助于开展并行的自动化攻击。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### 应用程序策略是否限制了并发会话？
- [ ] 是 — 每个用户仅**允许**一个会话；新的登录会使旧会话失效（最安全）  
- [ ] 是 — **强制执行**固定数量的并发会话且不能超过该限制  
- [ ] 否 — **可以**建立无限数量的并发会话  

### 当达到会话限制时，应用程序如何响应？
- [ ] 最早的活动会话**被自动终止**（会话固定 (Session Fixation)/劫持 (Hijacking) 缓解措施）  
- [ ] 在授权新会话之前，系统会**提示**用户终止现有会话  
- [ ] 新的登录尝试**被阻止**，直到从另一台设备手动注销为止  
- [ ] 未采取任何措施 — 多个会话同时保持**启用**状态  

### 应用程序是否为用户提供会话管理界面？
- [ ] 是 — 用户**可以**查看活动会话（IP、设备、时间）并远程终止它们  
- [ ] 是 — 用户**可以**查看活动会话，但**不能**远程终止它们  
- [ ] 否 — 用户**无法**查看或管理与其帐户关联的其他活动会话  

### 当检测到并发登录活动时，是否通知用户？
- [ ] 是 — 针对来自新设备或位置的登录，**触发**告警（电子邮件、短信或应用内通知）  
- [ ] 否 — 建立并发会话时**不提供**任何通知  

---