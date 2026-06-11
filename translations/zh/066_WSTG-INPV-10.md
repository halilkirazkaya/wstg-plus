## WSTG-INPV-10 — IMAP SMTP 注入 (IMAP SMTP Injection)

当 Web 应用程序在将用户提供的数据纳入发送至邮件服务器的命令之前，未能对其进行适当过滤时，就会产生 IMAP 和 SMTP 注入 (IMAP SMTP Injection) 漏洞。通过注入回车和换行 (Carriage Return and Line Feed, CRLF) 字符，攻击者可以终止预定命令并附加任意邮件指令，例如添加额外的收件人（抄送/密送，CC/BCC）或修改邮件正文。这些缺陷最常见于 Web 到邮件网关、联系表单以及通过 IMAP4 或 SMTP 等协议直接与邮件服务器通信的邮件客户端。成功利用此漏洞可导致未经授权的垃圾邮件转发 (Relaying)、敏感邮件内容外泄，在某些情况下，甚至会导致邮件服务器完整性的全面受损。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### 应用程序是否处理用户提供的输入以构建 IMAP 或 SMTP 命令？
- [ ] 否 — 应用程序**不**通过用户输入与邮件服务器交互  
- [ ] 是 — 应用程序与邮件服务器交互，但使用了安全的 API 或库  
- [ ] 是 — 应用程序使用用户受控的参数构建原始邮件命令  

### 是否对输入进行了验证以防止注入回车 (`\r`) 和换行 (`\n`) 序列？
- [ ] 是 — CRLF 字符被**严格**过滤、编码或拒绝  
- [ ] 是 — 输入根据严格的字符白名单 (Allowlist) 进行验证  
- [ ] 否 — CRLF 字符**未**被过滤，允许命令终止和注入  

### 攻击者能否注入额外的邮件标头，如 `Bcc:` 或 `Cc:`？
- [ ] 否 — 由于严格的输入限制，**无法**修改标头  
- [ ] 是 — **可以**通过 CRLF 序列注入额外的标头  
- [ ] 是 — 外部域的邮件转发**已启用**且可被利用 *(严重)*  

### 应用程序是否允许操纵 IMAP 命令以访问未经授权的邮箱？
- [ ] 否 — 应用程序**不**使用 IMAP，或将邮箱访问严格限制为已验证身份的用户  
- [ ] 是 — IMAP 命令注入**是可能的**，但仅限于元数据枚举 (Metadata Enumeration)  
- [ ] 是 — IMAP 命令注入允许读取或删除其他用户的电子邮件  

### 后端邮件服务器是否存在命令流水线 (Command Pipelining) 漏洞？
- [ ] 否 — 邮件服务器拒绝批处理命令或单个会话中的多个命令  
- [ ] 是 — 邮件服务器处理通过单个输入字段注入的多个命令  

---