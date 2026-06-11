## WSTG-SESS-07 — 测试会话超时 (Testing Session Timeout)

会话超时测试评估应用程序是否在预定义的非活动期间或总时长后有效地终止用户会话。此项控制对于降低会话劫持 (Session Hijacking) 的风险至关重要，特别是在共享工作站或攻击者通过网络拦截或跨站脚本攻击 (XSS) 获取会话标识符 (Session Identifier) 的场景中。渗透测试人员分析在一段时间非活动后触发的空闲超时 (Idle Timeouts) 以及限制会话总寿命（无论活动与否）的绝对超时 (Absolute Timeouts)。从攻击者的角度来看，无限期或过长的会话为维持未经授权的访问并绕过重新身份验证的需求提供了更长的机会窗口。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **测试状态** | 未执行 |
| **严重程度** | 中 (Medium) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### 应用程序是否实现了空闲会话超时？
- [ ] 是 — 会话在一段较短的非活动时间（例如 15-30 分钟）后过期  
- [ ] 是 — 会话会过期，但超时时长**过长**（例如 > 24 小时）  
- [ ] 否 — 在非活动期间会话**无限期保持活跃**  

### 会话超时是否在服务端 (Server-side) 强制执行？
- [ ] 是 — 无论客户端状态如何，服务端都会拒绝带有过期令牌 (Token) 的请求  
- [ ] 否 — 超时**仅**通过客户端 JavaScript 或元刷新 (Meta-refresh) 强制执行  

### 应用程序是否实现了绝对会话超时？
- [ ] 是 — 无论活动与否，会话都会在固定的最大时长后终止  
- [ ] 否 — 只要有持续的活动，会话**就可以**无限期延长  

### 超时后会话标识符是否在服务端失效？
- [ ] 是 — 一旦达到超时阈值，会话令牌**不可**重复使用  
- [ ] 否 — 即使客户端已重定向到登录页面，会话令牌在服务端**仍然有效**  

### 是否可以使用自动心跳请求 (Heartbeat Requests) 无限期地保持会话活跃？
- [ ] 否 — 绝对超时或其他二级控制措施**阻止**了无限期的会话延长  
- [ ] 是 — 定期的后台请求（例如 AJAX）**可以**无限期地维持会话  

---