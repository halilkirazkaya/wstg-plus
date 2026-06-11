## WSTG-CONF-04 — 审查旧备份及未引用文件中的敏感信息 (Review Old Backup and Unreferenced Files for Sensitive Information)

审查旧备份及未引用文件涉及识别 Web 服务器上被遗忘的、临时的或隐藏的、且不打算公开访问的文件。这些文件通常包括源代码备份 (`.zip`, `.bak`)、文本编辑器交换文件 (`.swp`, `~`) 或源码控制元数据 (Source Control Metadata) (`.git`, `.svn`)，它们可能会泄露敏感信息。攻击者使用自动化字典 (Wordlists) 和发现工具进行暴力破解 (Brute Force) 常用命名规范，旨在窃取数据库凭据、硬编码的 API 密钥或有助于进一步利用 (Exploit) 的逻辑。这种漏洞通常源于手动服务器维护或未能清理生产环境 Web 根目录 (Web Root) 的缺陷 CI/CD 流水线 (CI/CD Pipelines)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果发现配置文件、源代码存档或凭据，严重程度将变为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Web 服务器是否启用了目录列表 (Directory Listings)？
- [ ] 否 — 目录列表已被禁用，并返回 403 Forbidden 或自定义错误  
- [ ] 是 — 在非敏感目录上启用了目录列表  
- [ ] 是 — 在包含源代码或敏感文件的目录上启用了目录列表 *(严重)*  

### 具有常用扩展名（如 .bak, .old, .save）的备份文件是否可被发现？
- [ ] 否 — 未发现常用的备份扩展名，或已被服务器配置拦截  
- [ ] 是 — 存在备份文件，但不包含敏感信息  
- [ ] 是 — 配置文件或源代码的备份文件可以被访问 *(高)*  

### 源码控制目录（如 .git, .svn）或元数据是否暴露？
- [ ] 否 — 源码控制元数据不存在或已被正确拦截  
- [ ] 是 — 存在元数据，但无法重构完整的代码仓库 (Repository)  
- [ ] 是 — 可以通过暴露的元数据窃取整个源代码仓库  

### Web 根目录下是否存在应用程序的压缩存档（如 .zip, .tar.gz, .rar）？
- [ ] 否 — 通过暴力破解或枚举未发现敏感的压缩文件  
- [ ] 是 — 发现了压缩存档，但它们受密码保护或仅包含公开资源  
- [ ] 是 — 包含应用程序源码或数据的未加密压缩存档可被公众访问  

### 应用程序是否泄露了由文本编辑器或 IDE 创建的临时文件？
- [ ] 否 — 不存在类似于 `.swp`, `~` 或 `.DS_Store` 的临时文件  
- [ ] 是 — 存在非敏感的临时文件  
- [ ] 是 — 临时文件泄露了源代码片段或内部路径  

---