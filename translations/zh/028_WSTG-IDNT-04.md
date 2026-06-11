## WSTG-IDNT-04 — 账户枚举 (Account Enumeration) 和可猜测用户账户测试

账户枚举 (Account Enumeration) 发生在应用程序通过 HTTP 响应的变化、错误消息或可测量的时间差异泄露用户账户是否存在时。攻击者通过系统地提交用户名列表来识别有效账户，从而利用这一行为进行撞库 (Credential Stuffing)、暴力破解 (Brute Force) 或社会工程学 (Social Engineering) 攻击。这种漏洞通常出现在登录入口、密码重置功能、注册表单以及处理用户特定标识符的 API 端点中。从攻击者的角度来看，成功的枚举通过将盲目的暴力破解尝试转变为针对已知有效身份的定向利用，显著缩小了攻击面。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### 身份验证错误消息在所有场景下是否一致？
- [ ] 是 — 对于无效用户名和无效密码均使用通用的错误消息  
- [ ] 是 — 消息类似，但在标点符号或大小写上**存在**细微差异  
- [ ] 否 — 错误消息明确指出“用户未找到”或“密码错误” *(存在漏洞)*  

### 密码重置或“忘记密码”流程是否泄露账户存在性？
- [ ] 否 — 无论电子邮件/用户名是否存在，应用程序均返回通用消息  
- [ ] 是 — 已部署控制措施，但通过响应长度或状态码**可以绕过**  
- [ ] 是 — 应用程序明确确认是否发送了重置邮件，或者账户**不存在**  

### 注册过程是否针对用户枚举进行了防护？
- [ ] 否 — 注册不会泄露信息，或使用 CAPTCHA/速率限制 (Rate Limiting) 来防止批量检查  
- [ ] 是 — 已部署控制措施，但通过时间差异或 API 响应差异**可以绕过**  
- [ ] 是 — 如果用户名或电子邮件**已被占用**，应用程序会立即通知用户  

### 在比较有效与无效用户名时，时间差异是否可测量？
- [ ] 否 — 由于采用了恒定时间比较，无论账户是否有效，响应时间均保持一致  
- [ ] 是 — 存在时间差异，但差异太小，无法通过网络可靠地测量  
- [ ] 是 — **存在**可测量的时间差异（例如，由于仅对有效用户执行密码哈希 (Password Hashing) 计算）  

### 应用程序是否对账户使用可预测或可猜测的命名约定？
- [ ] 否 — 账户标识符是随机的 (UUIDs) 或由用户定义且复杂的  
- [ ] 是 — 账户标识符遵循特定模式（例如 `emp001`, `emp002`），且枚举**是可能的**  
- [ ] 是 — 标识符是连续的整数，使得完整的数据库枚举**成为可能**  

---