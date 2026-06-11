## WSTG-INPV-14 — 潜伏型漏洞测试 (Testing for Incubated Vulnerabilities)

潜伏型漏洞 (Incubated Vulnerabilities)，也称为持久型或二阶漏洞 (Second-order Vulnerabilities)，当恶意负载 (Payload) 被应用程序以“冷”状态存储，随后在不同的上下文 (Context) 中被执行或处理时，就会发生此类漏洞。这种攻击模式通常针对后端系统、管理控制台或自动报告工具，这些工具从共享数据库或文件系统中检索数据，而没有进行充分的重新验证或输出编码 (Output Encoding)。渗透测试人员 (Pentesters) 必须追踪用户输入的生命周期，从入口点（即“孵化器”）到该数据最终被其他组件或辅助应用程序呈现或利用的每个位置。成功的利用通常会导致高影响的后果，例如通过存储型跨站脚本攻击 (Stored XSS) 劫持管理员会话，或者通过内部报表模块中的二阶 SQL 注入 (Second-order SQL Injection) 进行数据外泄。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**工具：** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### 是否存在用户控制的输入被存储以供其他用户或进程后续检索的端点 (Endpoints)？
- [ ] 否 — 应用程序**不**存储用户输入以供后续使用  
- [ ] 是 — 输入**被**存储在数据库字段、日志文件或配置设置中  

### 在将数据写入持久化存储层之前，是否应用了输入验证或清理 (Sanitization)？
- [ ] 是 — 在入口点**应用了**严格的白名单 (Allow-list) 验证  
- [ ] 是 — **应用了**通用清理，但**可能**通过编码或非标准字符绕过  
- [ ] 否 — 输入以原始、未经验证的形式存储  

### 存储的数据在管理或辅助界面中呈现时，是否经过了正确的编码或清理？
- [ ] 是 — 在所有呈现数据的地方都**应用了**上下文感知输出编码  
- [ ] 是 — 在某些位置**应用了**编码，但在其他位置（如内部管理员仪表板）**缺失**  
- [ ] 否 — 数据在没有任何编码或清理的情况下呈现，允许执行负载 (Payload)  

### 存储在数据库中的负载 (Payload) 是否会影响后端批处理过程或带外 (Out-of-band / OOB) 监控系统？
- [ ] 否 — 后端进程使用安全的解析方法或参数化查询  
- [ ] 是 — 存储的负载**可以**在后端逻辑中触发交互（例如，CSV 注入 (CSV Injection)、日志中的命令注入 (Command Injection)）  
- [ ] 是 — 存储的负载**可以**向攻击者控制的服务器触发带外 (OOB) 请求  

---