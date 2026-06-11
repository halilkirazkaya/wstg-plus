## WSTG-IDNT-05 — 测试薄弱或未强制执行的用户名策略 (Username Policy)

测试薄弱或未强制执行的用户名策略侧重于管理账户标识符创建的规则，以及它们对枚举 (Enumeration) 或欺骗 (Spoofing) 的敏感性。薄弱的策略通常允许可预测的序列、常见的字典词汇或反映内部员工 ID 的标识符，这会显著缩小暴力破解 (Brute Force) 和凭据填充 (Credential Stuffing) 攻击的搜索空间。从攻击者的角度来看，识别这些模式是进行大规模账户发现的第一步，通常通过分析注册错误消息、密码重置行为或面向公众的配置文件中的元数据来实现。确保强健的策略可以防止攻击者系统地映射用户群，并有助于防御针对性的网络钓鱼和自动化的身份验证绕过 (Authentication Bypass) 尝试。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### 用户名是否基于高度可预测或顺序的模式？
- [ ] 否 —— 用户名是随机的、用户定义的或高熵字符串  
- [ ] 是 —— 用户名遵循可预测的格式（例如 `user1001`，`user1002`），但身份验证 (Authentication) 机制是强健的  
- [ ] 是 —— 用户名是**严格顺序**的或遵循已知的企业格式，便于枚举  

### 应用程序是否对用户名强制执行最小长度和复杂度要求？
- [ ] 是 —— 在注册期间**强制执行**严格的长度和字符集策略  
- [ ] 是 —— 存在策略，但允许极短（例如 1-2 个字符）或过于简单的用户名  
- [ ] 否 —— **没有强制执行**关于用户名长度或字符类型的策略  

### 是否可以通过应用程序响应枚举有效的用户名？
- [ ] 否 —— 应用程序返回通用的错误消息，并对所有尝试表现出一致的响应时间  
- [ ] 是 —— 应用程序返回不同的错误消息（例如“用户名已被占用”），但**应用了**速率限制 (Rate Limiting)  
- [ ] 是 —— 应用程序通过注册错误、密码重置或时间差异**泄露**有效的用户名  

### 该策略是否防止注册常见的或保留的管理用户名？
- [ ] 是 —— 应用程序封禁了常用名称（例如 `admin`，`root`，`support`）和系统保留术语  
- [ ] 否 —— 用户**可以**使用敏感名称或管理名称注册账户  

### 用户名策略在所有入口点（API、移动端、Web）是否一致？
- [ ] 是 —— 策略在所有接口和版本中**一致应用**  
- [ ] 否 —— 遗留端点或特定 API 版本**未强制执行**相同的用户名约束  

---