## WSTG-INPV-08 — 服务器端包含注入 (SSI Injection) 测试

当应用程序在将用户输入并入由服务器服务器端包含 (Server-Side Includes, SSI) 引擎处理的 HTML 文件之前，未能对其进行清洗或过滤时，就会发生服务器端包含注入 (SSI Injection)。攻击者利用此漏洞注入 SSI 指令（例如 `<!--#exec cmd="id" -->`），以执行任意 Shell 命令 (Shell commands)、读取本地文件或窃取环境变量。此漏洞通常存在于提供 `.shtml`、`.shtm` 或 `.stm` 等旧版扩展名文件的应用程序中，或者 Web 服务器被显式配置为在标准 HTML 文件中解析 SSI。成功利用该漏洞可授予攻击者与 Web 服务器进程相同的权限，通常导致整个系统失陷或敏感数据泄露。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Web 服务器是否支持并处理 SSI 指令？
- [ ] 否 — 服务器**不支持** SSI 或 `.shtml` 等文件扩展名**已禁用**  
- [ ] 是 — SSI **已启用**，但用户输入**未**在处理后的页面中反射  
- [ ] 是 — SSI **已启用**，且用户输入**已**在处理后的页面中反射  

### 是否可以通过 `#exec` 指令执行任意系统命令？
- [ ] 否 — `#exec` 指令**已禁用**或受服务器配置限制  
- [ ] 是 — 通过 `#exec cmd` 或 `#exec cgi` 有效负载 (Payload) **可以执行**命令  

### 是否可以窃取敏感文件或环境变量？
- [ ] 否 — `#include` 或 `#config` 等指令**已禁用**  
- [ ] 是 — 通过 `#include virtual` **可以读取**本地文件（例如 `/etc/passwd`）  
- [ ] 是 — 通过 `#printenv` 或 `#echo` **可以窃取**服务器环境变量  

### 输入验证或 WAF 控制对 SSI Payload 是否有效？
- [ ] 是 — **应用了**严格的输入验证且**无法绕过**  
- [ ] 是 — **应用了**验证/Web 应用防火墙 (WAF)，但使用字符编码或不同的 SSI 语法**可以绕过**  
- [ ] 否 — 对 SSI 相关字符（`<`, `!`, `#`, `-`, `"`）不存在验证或 WAF 防护  

---