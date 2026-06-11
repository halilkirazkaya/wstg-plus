## WSTG-ATHN-09 — 测试弱密码修改或重置功能

密码修改和重置机制是身份验证管理的关键组件，如果实现不当，攻击者将能够接管用户账户。这些漏洞通常涉及可预测的重置令牌 (Token)、在暴力破解 (Brute Force) 尝试期间缺乏账户锁定机制、令牌传输不安全，或者在未验证当前密码的情况下允许修改密码。攻击者通过拦截或猜测恢复链接、利用主机头注入 (Host Header Injection) 重定向重置邮件，或利用会话固定 (Session Fixation) 在密码修改后维持访问权限来利用这些缺陷。确保这些过程需要严格的验证并使用加密随机性，对于维护账户完整性和防止未经授权的访问至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 紧急 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**工具：** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### 在标准修改过程中，设置新密码是否需要当前密码？
- [ ] 是 — **需要**并验证当前密码  
- [ ] 是 — **需要**当前密码，但可以通过参数污染 (Parameter Pollution) 或篡改绕过  
- [ ] 否 — **不要求**输入当前密码，允许通过会话劫持 (Session Hijacking) 接管账户 *(高)*  

### 密码重置令牌是否具有加密安全性且不可预测？
- [ ] 是 — 令牌具有高熵、随机性且**不可预测**  
- [ ] 是 — 生成了令牌，但显示出低熵或可预测的模式（例如基于时间戳）  
- [ ] 否 — 令牌**可预测**或基于静态用户数据，如 Base64 编码的电子邮件/ID *(紧急)*  

### 密码重置链接是否可以通过主机头注入被重定向？
- [ ] 否 — 应用程序在生成链接时忽略或验证了 `Host` 头  
- [ ] 是 — 应用程序使用 `Host` 或 `X-Forwarded-Host` 头构建链接，但验证**存在**  
- [ ] 是 — 链接**可以**通过请求头篡改重定向到攻击者控制的域名 *(高)*  

### 重置令牌是否具有有限的生命周期并能立即失效？
- [ ] 是 — 令牌很快过期，并在使用或密码更改后**立即**失效  
- [ ] 是 — 令牌在较长时间后过期（例如 24 小时以上）或允许**多次使用**  
- [ ] 否 — 令牌**永不过期**，或在密码成功更改后仍然有效  

### 密码重置和修改端点是否存在速率限制或锁定机制？
- [ ] 是 — **应用了**严格的速率限制 (Rate Limiting) 和验证码 (CAPTCHA)，以防止枚举和暴力破解  
- [ ] 是 — 存在有限的控制，但可以通过 IP 轮换或请求头欺骗进行绕过  
- [ ] 否 — **未应用**速率限制，允许进行批量账户枚举或令牌暴力破解  

---