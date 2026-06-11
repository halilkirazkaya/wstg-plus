## WSTG-INPV-12 — 命令注入 (Command Injection)

命令注入 (Command Injection) 发生在应用程序将不安全的、由用户提供的数据（如表单输入、HTTP 标头 (HTTP Headers) 或 Cookies）传递给系统外壳 (System Shell) 时。此漏洞允许攻击者在服务器上执行任意操作系统命令，通常具有 Web 应用程序进程的权限。从攻击者的角度来看，利用该漏洞的方法是使用分号、管道符或反引号等 Shell 元字符 (Shell Metacharacters) 将恶意命令链接到预期的执行流中。其影响通常是灾难性的，会导致全系统沦陷、未经授权的数据外传或在内网中进行横向移动 (Lateral Movement)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **测试状态** | 未执行 |
| **严重程度** | 严重 (Critical) |

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**工具：** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### 应用程序是否调用系统级命令或 Shell 可执行文件？
- [ ] 否 — 应用程序**不**与操作系统 Shell 交互  
- [ ] 是 — 应用程序使用内部 API 或高级库执行系统任务  
- [ ] 是 — 应用程序通过 `system()`、`exec()` 或 `popen()` 等系统调用直接调用 Shell 命令  

### 在传递给系统调用之前，是否对用户输入进行了验证或清理？
- [ ] 是 — **应用了**严格的白名单 (Allow-listing) 和输入验证  
- [ ] 是 — 使用了清理 (Sanitization) 或转义，但**可能被绕过**  
- [ ] 否 — 用户输入在**未经**验证的情况下直接传递给 Shell  

### 是否可以使用 Shell 元字符来操纵命令执行流？
- [ ] 否 — 元字符被正确过滤或转义  
- [ ] 是 — **可能实现**带内 (In-band) 执行，即命令输出反映在响应中  
- [ ] 是 — **可能实现**盲 (Blind) 执行，即没有输出反映在响应中  

### 是否可以从主机进行带外 (Out-of-band, OOB) 数据外传？
- [ ] 否 — 服务器没有出站连接或带外 (OOB) 信号（DNS/HTTP）失败  
- [ ] 是 — **可能**通过基于 DNS 或 HTTP 的带外 (OOB) 信号进行数据外传  

### 应用程序进程是否以提升的系统权限运行？
- [ ] 否 — 应用程序使用专用的低权限服务帐户运行  
- [ ] 是 — 应用程序以 `root`、`Administrator` 或高权限用户身份运行 *（严重）*  

---