## WSTG-BUSL-07 — 测试针对应用程序误用的防御机制

针对应用程序误用的防御评估旨在衡量应用程序识别、记录并响应偏离预期业务逻辑 (Business Logic) 的异常用户行为的能力。与传统的基于特征 (Signature-based) 的安全性不同，这些防御机制侧重于检测表明自动化滥用的模式，例如快速连续请求、凭据填充 (Credential Stuffing) 或顺序资源枚举。从攻击者的角度来看，此测试确定了应用程序对自动化以及旨在泄露数据或耗尽资源而不触发标准安全警报的“低速且缓慢”攻击的韧性。未能实施这些防御措施通常会导致大规模数据抓取、账户接管 (Account Takeover) 或通过合法但资源密集型的应用程序功能导致拒绝服务。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### 是否在敏感端点上强制执行了速率限制 (Rate Limiting) 或流量削减 (Throttling) 机制？
- [ ] 是 — 在所有敏感端点上都**应用**并强制执行了严格的速率限制 *(最安全)*  
- [ ] 是 — **应用**了速率限制，但可以通过 IP 轮换 (IP rotation) 或标头篡改 (header manipulation) 绕过  
- [ ] 是 — 仅对身份验证端点**应用**了速率限制，导致其他端点暴露  
- [ ] 否 — 未对任何应用程序功能**应用**速率限制或流量削减  

### 应用程序是否检测并阻止自动化交互模式？
- [ ] 是 — **启用**了行为分析或验证码 (CAPTCHAs)，并有效阻止了机器人 (Bots)  
- [ ] 是 — **启用**了反自动化机制，但通过使用无头浏览器 (Headless Browsers) 或会话循环 (Session Cycling) **可能实现**绕过  
- [ ] 否 — 应用程序**无法**区分人类用户和自动化脚本  

### 异常行为是否被记录并触发防御性响应？
- [ ] 是 — 误用行为会触发自动拦截并向安全团队发出告警  
- [ ] 是 — 误用行为会被记录用于审计，但**未**采取自动防御行动  
- [ ] 否 — 应用程序**不记录**也不响应异常活动模式  

### 是否可以通过操纵客户端标识符或标头 (Headers) 来绕过防御？
- [ ] 否 — 防御依赖于服务器端状态和经过验证的遥测数据 (Telemetry)  
- [ ] 是 — **可以**通过清除 Cookies 或轮换 `User-Agent` 字符串来绕过控制  
- [ ] 是 — **可以**通过注入诸如 `X-Forwarded-For` 或 `X-Real-IP` 之类的标头来绕过控制  

---