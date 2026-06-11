## WSTG-INPV-06 — LDAP 注入测试

LDAP 注入 (LDAP Injection) 发生在应用程序将用户提供的数据合并到轻型目录访问协议 (Lightweight Directory Access Protocol, LDAP) 过滤器中，而没有进行适当的清理或转义 (Sanitization or Escaping) 时，这使得攻击者能够操纵查询逻辑。通过注入星号、括号和逻辑运算符等特殊字符，攻击者可以修改搜索过滤器以绕过身份验证机制 (Authentication mechanisms) 或窃取敏感目录信息 (Exfiltrate sensitive directory information)，包括用户名、组成员身份和组织属性。这种漏洞通常出现在与活动目录 (Active Directory) 或 OpenLDAP 对接的功能中，如公司目录搜索、员工门户或单点登录 (SSO) 系统。从攻击者的角度来看，当应用程序抑制直接查询输出时，成功利用通常涉及盲注技术 (Blind techniques)，以便逐位枚举 (Enumerate) 目录结构或属性值。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**工具：** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### 应用程序是否利用 LDAP 进行身份验证或目录搜索？
- [ ] 否 — 应用程序架构中**未使用** LDAP  
- [ ] 是 — 使用了 LDAP，且所有输入都经过严格清理或参数化  
- [ ] 是 — 使用了 LDAP，且用户输入在**未进行**适当转义的情况下被拼接进过滤器中  

### 用户输入中的 LDAP 元字符是否经过适当的转义或过滤？
- [ ] 是 — `(`, `)`, `&`, `|`, `*`, 和 `\` 等元字符 (Meta-characters) **不允许**使用或已被正确转义  
- [ ] 否 — 元字符**可以**被注入，以改变 LDAP 过滤器的结构  

### 是否可以通过注入绕过身份验证机制？
- [ ] 否 — 登录逻辑**不易受**查询操纵的影响  
- [ ] 是 — **可以**在用户名或密码字段中使用逻辑“或” (`|`) 或通配符 (`*`) 注入来绕过身份验证  

### 是否可以从目录服务中进行盲数据窃取？
- [ ] 否 — 搜索结果受限，且未观察到时间或布尔差异  
- [ ] 是 — **可以**通过基于布尔的盲注 (Boolean-based blind injection) 技术逐位枚举目录属性  
- [ ] 是 — 由于查询操纵，完整的目录记录**被反映**在应用程序响应中  

### 次要应用程序组件（例如邮件程序、SSO）是否容易受到基于 LDAP 的属性注入攻击？
- [ ] 否 — LDAP 属性在所有组件中均得到安全处理  
- [ ] 是 — 攻击者**可以**通过注入修改其自身属性（如电子邮件、组成员身份），以提升权限或重定向通信  

---