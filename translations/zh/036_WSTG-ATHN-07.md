## WSTG-ATHN-07 — 弱密码策略测试 (Testing for Weak Password Policy)

弱密码策略 (Weak Password Policy) 测试涉及评估应用程序在账户创建和凭证更新期间强制执行的复杂度、长度和熵 (Entropy) 要求。不充分的策略会使账户容易受到自动化字典攻击 (Dictionary Attacks)、暴力破解 (Brute-force) 尝试和凭证填充 (Credential Stuffing) 的影响，从而直接威胁到用户账户的机密性。这些漏洞通常存在于注册端点、密码重置工作流和管理员用户管理面板中。从攻击者的角度来看，宽松的策略显著降低了使用常用单词列表或基于模式的破解方式来成功获取凭证所需的计算成本和时间。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 低* |

> *如果应用程序缺乏账户锁定 (Account Lockout) 或速率限制 (Rate Limiting) 机制来防止自动化猜解，则严重程度变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**工具：** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### 应用程序是否强制执行最小密码长度？
- [ ] 是 — 最小长度为 12 个字符以上 *(最安全)*  
- [ ] 是 — 最小长度在 8 到 11 个字符之间  
- [ ] 是 — 最小长度**少于** 8 个字符  
- [ ] 否 — 未强制执行最小长度限制  

### 是否强制执行复杂度要求（大写字母、小写字母、数字、符号）？
- [ ] 是 — 必须包含多种字符类型，且**无法绕过**  
- [ ] 是 — 建议使用多种字符类型，但**未**强制执行  
- [ ] 否 — 未应用任何复杂度要求  

### 应用程序是否封禁通用/弱密码或包含用户特定数据的密码？
- [ ] 是 — **无法**使用已知的弱密码（例如 "password123"）和基于用户名的密码  
- [ ] 是 — 通用密码被封禁，但**可以**使用用户特定数据（如用户名、电子邮件）  
- [ ] 否 — 接受任何符合长度/复杂度要求的字符串  

### 是否可以通过客户端修改绕过复杂度或长度要求？
- [ ] 否 — 策略在**服务端 (Server-side)** 严格执行  
- [ ] 是 — 策略通过 JavaScript 执行，且**可以**通过拦截请求绕过  

### 密码策略是否在不泄露实现细节的情况下清晰地告知用户？
- [ ] 是 — 策略在输入期间可见，并提供通用的失败消息  
- [ ] 是 — 策略可见，但失败消息泄露了特定的正则表达式 (Regex) 或逻辑缺陷  
- [ ] 否 — 策略**不可见**，迫使用户通过反复试验来发现  

---