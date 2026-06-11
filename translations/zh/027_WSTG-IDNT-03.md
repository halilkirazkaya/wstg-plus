## WSTG-IDNT-03 — 测试账户配置流程 (Test Account Provisioning Process)

账户配置流程 (Account Provisioning Process) 规定了如何在应用程序环境中创建、验证新身份并分配权限。攻击者通常以该工作流为目标，进行用于发送垃圾邮件或拒绝服务 (Denial-of-Service, DoS) 的自动化批量注册，绕过身份验证步骤以创建虚假账户，或在注册阶段操纵参数以授予自己更高的权限。漏洞通常表现在自助服务注册表单、仅限邀请的系统或管理配置控制台中，在这些地方，业务逻辑缺陷 (Business Logic Flaws) 可能导致未经授权的账户创建或权限提升 (Privilege Escalation)。从攻击者的角度来看，攻破配置流程可以获得一个合法的立足点，作为进一步漏洞利用 (Exploit)、数据外泄或社会工程学 (Social Engineering) 攻击的跳板。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果攻击者可以配置管理员账户或绕过全局注册限制，严重程度将变为严重 (Critical)。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### 自助注册是否受到严格控制或仅限于授权用户？
- [ ] 是 — 注册功能已**禁用**或需要手动管理员审批  
- [ ] 是 — 注册开放，但仅限于**授权**的电子邮件域名或邀请令牌 (Invitation Tokens)  
- [ ] 否 — 注册对任何用户**开放**，且没有域名或令牌限制  

### 在激活账户之前，是否需要稳健的身份验证机制（例如：电子邮件/短信）？
- [ ] 是 — 验证是**必需的**且**无法**被绕过  
- [ ] 是 — 验证是**必需的**，但可以通过参数篡改 (Parameter Tampering) 或可预测的令牌**绕过**  
- [ ] 否 — 提交后账户**立即激活**，无需任何验证  

### 用户在注册请求期间是否可以操纵角色或权限参数？
- [ ] 否 — 角色在**服务端分配**，不存在客户端参数  
- [ ] 是 — 存在角色参数，但由于服务端验证，操纵是**不可能的**  
- [ ] 是 — 角色/权限参数（例如：`role=admin`, `is_staff=true`）**可以**被修改以获得提升的访问权限 *(严重)*  

### 是否实施了速率限制 (Rate Limiting) 或反自动化控制以防止批量创建账户？
- [ ] 是 — 速率限制和验证码 (CAPTCHA) 已**启用**且有效  
- [ ] 是 — 存在控制措施，但可以通过请求头伪造 (Header Spoofing) 或验证码破解服务**绕过**  
- [ ] 否 — **未应用**任何速率限制或反自动化控制  

### 配置流程是否会泄露现有账户的信息（用户枚举）？
- [ ] 否 — 现有用户和不存在用户的响应消息是**一致的**  
- [ ] 是 — 不同的响应或响应时间差异**允许**对现有用户进行用户枚举 (User Enumeration)  

---