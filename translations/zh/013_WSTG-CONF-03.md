## WSTG-CONF-03 — 测试文件扩展名处理以获取敏感信息 (File Extension Handling)

文件扩展名处理测试涉及识别 Web 服务器或应用服务器是否通过提供应受限制或应执行的文件来泄露敏感信息。攻击者经常探测可能留在 Web 根目录下的备份文件 (Backup Files)、配置文件 (Configuration Files) 和源代码片段 (Source Code Fragments)（例如 `.bak`, `.old`, `.env`, `.inc`）。由于缺乏特定的处理程序映射 (Handler Mappings)，这些文件可能会以纯文本形式提供。该漏洞非常关键，因为它可能导致数据库凭据 (Database Credentials)、API 密钥 (API Keys) 和内部业务逻辑的泄露，从而便于对基础设施进行更深层次的利用。这些泄露通常发生在公共目录、上传文件夹中，或者通过服务器上配置错误的 MIME 类型 (MIME-type) 设置发生。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **测试状态** | 未执行 |
| **严重性** | 中 / 高* |

> *如果包含凭据的配置文件或完整源代码可被访问，则严重性变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### 敏感的备份或临时文件扩展名（例如 `.bak`, `.old`, `.swp`, `~`）是否可访问？
- [ ] 否 — 服务器对常见的备份扩展名返回 403 Forbidden 或 404 Not Found  
- [ ] 是 — 服务器允许下载备份文件，但其中**不包含**敏感数据  
- [ ] 是 — 服务器允许下载包含敏感数据或源代码的备份文件 *(高)*  

### 服务器是否暴露了环境或系统配置文件（Environment or System Configuration Files）（例如 `.env`, `.config`, `.yml`, `.ini`）？
- [ ] 否 — 受限的配置文件**无法**访问并返回 403/404  
- [ ] 是 — 敏感配置文件**可以**访问并泄露了内部密钥或凭据  

### 是否可以通过附加替代扩展名（例如 `.php.txt`, `.jsp.old`, `.aspx.bak`）来检索源代码？
- [ ] 否 — 无论如何操纵扩展名，应用程序代码都**不会**被渲染为纯文本  
- [ ] 是 — 源代码被泄露，因为服务器**没有**针对附加扩展名的处理程序  

### 管理目录或元数据（Metadata）目录（例如 `.git/`, `.svn/`, `.DS_Store`）是否禁止公共访问？
- [ ] 否 — 这些目录/文件在 Web 根目录下**不**存在  
- [ ] 是 — 由于服务器端的重写规则 (Rewrite Rules) 或权限设置，**无法**访问  
- [ ] 是 — 元数据目录**可以**访问，并允许进行完整的源代码重构  

### 服务器如何处理具有多重扩展名 (Multiple Extensions)（例如 `file.php.jpg`）的文件请求？
- [ ] 否 — 服务器正确地优先考虑内部扩展名的安全性或拦截该请求  
- [ ] 是 — 服务器将文件作为第一个扩展名处理，但**无法**绕过执行  
- [ ] 是 — 服务器忽略了最终扩展名，**可能**允许代码执行或源码泄露  

---