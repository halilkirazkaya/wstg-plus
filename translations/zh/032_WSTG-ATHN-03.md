## WSTG-ATHN-03 — 弱锁定机制测试

账户锁定机制 (Account Lockout Mechanism) 是一种关键的纵深防御控制措施，旨在通过在指定次数的身份验证 (Authentication) 尝试失败后禁用账户，来缓解暴力破解 (Brute-Force) 和凭证填充 (Credential Stuffing) 攻击。如果没有稳健的锁定策略，攻击者可以使用自动化工具系统地猜测多个账户的密码（密码喷洒 (Password Spraying)），或针对单个高价值账户进行尝试直到成功。这种漏洞通常出现在缺乏速率限制 (Rate Limiting) 或账户级保护的登录门户、密码重置表单和 API 端点上。从攻击者的角度来看，弱锁定机制或缺乏锁定机制允许无限次的密码尝试，而过于激进的锁定机制则可能被利用，通过故意锁定合法用户的账户来造成针对这些用户的拒绝服务 (Denial of Service, DoS) 攻击。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果应用程序处理敏感的个人身份信息 (PII)、财务数据，或者管理账户在没有任何辅助控制的情况下容易受到暴力破解的影响，严重程度将变为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**工具：** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### 是否在达到设定的失败尝试次数后强制执行账户锁定机制？
- [ ] 是 — 账户在达到定义的、合理的阈值（例如 5-10 次尝试）后被锁定  
- [ ] 是 — 账户虽然被锁定，但仅在尝试次数过高后才执行  
- [ ] 否 — 账户无法被锁定，并允许无限次的暴力破解  

### 是否可以使用常见的规避技术绕过锁定机制？
- [ ] 否 — 锁定在服务端 (Server-side) 强制执行，不受客户端 (Client-side) 更改的影响  
- [ ] 是 — 可以通过轮换 IP 地址或使用代理池来绕过锁定  
- [ ] 是 — 可以通过操作如 `X-Forwarded-For` 或 `User-Agent` 等 HTTP 标头 (HTTP Headers) 来绕过锁定  
- [ ] 是 — 可以通过清除 Cookie 或启动新会话来绕过锁定  

### 锁定机制是否提供账户恢复或过期的路径？
- [ ] 是 — 账户在设定的时长后自动解锁，或通过邮件验证解锁  
- [ ] 是 — 账户需要管理员干预才能解锁  
- [ ] 否 — 锁定是永久性的，且没有明确的恢复路径，增加了拒绝服务 (DoS) 的风险  

### 应用程序在锁定期间是否区分有效和无效的用户名？
- [ ] 否 — 应用程序对现有用户和不存在用户的响应是相同的  
- [ ] 是 — 应用程序会泄露账户是否已被锁定，从而允许用户名枚举 (Username Enumeration)  

### 锁定机制是否容易受到拒绝服务 (DoS) 攻击？
- [ ] 否 — 验证码 (CAPTCHA) 或基于 IP 的速率限制等控制措施可防止大规模账户锁定  
- [ ] 是 — 攻击者可以通过提供错误的密码，系统地锁定所有已知用户  

---