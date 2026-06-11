## WSTG-BUSL-05 — 测试功能使用次数限制 (Test Number of Times a Function Can Be Used Limits)

此项测试旨在评估应用程序是否针对单个用户、会话 (Session) 或 IP 地址，就特定操作或功能的执行频率及总次数强制执行业务逻辑约束 (Business Logic Constraints)。如果未能实现这些限制，攻击者可能会滥用高价值功能——例如发送短信一次性密码 (SMS OTP)、触发密码重置邮件、使用优惠码或执行海量数据导出——从而导致经济损失、资源耗尽 (Resource Exhaustion) 或声誉损害。这些漏洞通常存在于交易端点或通信功能中，攻击者通过自动化脚本重复执行操作，远远超出预期的业务阈值。从攻击者的角度来看，这是短信轰炸 (SMS Pumping)、邮件炸弹 (Mail Bombing) 或针对缺乏明确速率限制 (Rate Limiting) 或计数节流的工作流进行暴力破解 (Brute-forcing) 的首选目标。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 中 / 高* |

> *如果功能涉及金融交易、短信成本或影响全系统的可用性，严重程度将变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### 应用程序是否定义并强制执行敏感业务功能的限制？
- [ ] 是 — 限制被严格执行且**无法**绕过  
- [ ] 是 — 强制执行了限制，但阈值过高，无法防止滥用  
- [ ] 否 — 未对功能执行应用任何限制  

### 速率限制和使用计数器是否在服务端强制执行？
- [ ] 是 — 强制执行严格位于服务端 (Server-side) 且**不可**被操纵  
- [ ] 否 — 强制执行依赖于客户端逻辑 (Client-side Logic)（例如 JavaScript 或隐藏字段）  
- [ ] 否 — 任何层级均未实施强制执行  

### 是否可以通过请求头或会话篡改绕过使用限制？
- [ ] 否 — **无法**通过 `X-Forwarded-For`、`Origin` 或会话轮换进行绕过  
- [ ] 是 — **可以**通过伪造 IP 请求头或清除 Cookie 来绕过限制  
- [ ] 是 — **可以**通过创建多个账号或会话来绕过限制  

### 超出限制时是否触发了适当的防御性响应？
- [ ] 是 — 应用程序返回 `429 Too Many Requests` 并记录该事件 *(最安全)*  
- [ ] 是 — 应用程序返回错误，但**未**记录潜在的滥用行为  
- [ ] 否 — 应用程序继续处理请求但忽略输出结果  
- [ ] 否 — 应用程序不提供任何响应或在负载下崩溃  

### 滥用该功能对业务的影响是否重大？
- [ ] 否 — 功能滥用产生的成本或资源影响微不足道  
- [ ] 是 — 滥用**可能**导致轻微的资源消耗或用户困扰  
- [ ] 是 — 滥用**可能**导致重大的经济损失（例如短信费用）或拒绝服务 (Service Denial) *(关键)*  

---