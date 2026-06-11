## WSTG-CONF-09 — 文件权限测试 (Test File Permission)

测试文件权限涉及审计分配给 Web 服务器 (Web Server) 上文件和目录的访问控制 (Access Control) 级别，以确保敏感资源不会过度暴露给未经授权的用户或进程。配置不当的权限可能允许攻击者读取包含数据库凭据 (Database Credentials) 的敏感配置文件，查看专有源代码，或修改服务器端脚本以实现远程代码执行 (Remote Code Execution, RCE)。此漏洞通常发生在部署阶段，当时默认的“全局可读 (World-readable)”或“全局可写 (World-writable)”标志被保留在关键应用程序目录上，例如配置路径、日志文件夹或上传存储库。从攻击者的角度来看，发现一个安全性不当的文件通常是权限提升 (Privilege Escalation) 或整个系统失陷的主要诱因。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果敏感配置文件（如 `.env`、`web.config`）可读，或者可以对 Web 根目录 (Web Root) 进行写访问，严重程度将变为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**工具：** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### 敏感应用程序配置文件是否受到保护，防止未经授权的访问？
- [ ] 是 — 配置文件（如 `.env`, `config.php`）**无法**通过 Web 请求访问  
- [ ] 是 — 文件存在，但访问权**仅限**于授权的本地用户  
- [ ] 否 — 敏感配置文件**可被访问**并泄露凭据或机密信息  

### 攻击者能否修改 Web 根目录内的文件或目录？
- [ ] 否 — 所有 Web 根目录对于 Web 服务器用户均为**只读**  
- [ ] 是 — 存在可写目录，但脚本执行已**禁用**  
- [ ] 是 — 存在全局可写目录，并**允许**上传和执行脚本 *(严重)*  

### 是否由于权限松懈而导致版本控制元数据或备份文件暴露？
- [ ] 否 — `.git`, `.svn` 和备份文件（如 `.bak`, `~`）**不存在**或受保护  
- [ ] 是 — 存在元数据目录，但目录列表已**禁用**  
- [ ] 是 — 敏感元数据或备份**完全可访问**，从而导致源代码泄露  

### Web 服务器用户是否以最小权限原则 (Principle of Least Privilege) 运行？
- [ ] 是 — Web 服务器进程以**专用低权限**用户身份运行  
- [ ] 否 — Web 服务器进程以**不必要的权限**（如 `root` 或 `Administrator`）运行  

### 日志文件是否受到保护，防止未经授权的读取或修改？
- [ ] 是 — 日志文件访问**仅限**于管理用户  
- [ ] 是 — 日志文件可读，但不包含会话令牌 (Session Tokens) 或个人身份信息 (PII)  
- [ ] 否 — 日志文件**公开可读**，并泄露敏感交易数据或用户信息  

---