## WSTG-BUSL-09 — 测试恶意文件上传

文件上传功能允许用户向服务器提交数据，这为将恶意内容引入应用程序环境提供了直接向量。攻击者利用弱验证逻辑上传 Web 脚本 (Web Shell)、恶意软件或经过特殊设计的文件——例如 SVG 或 HTML——以实现远程代码执行 (Remote Code Execution, RCE) 或跨站脚本攻击 (Cross-Site Scripting, XSS)。此漏洞通常存在于个人资料设置、文档管理系统和附件处理程序等功能中，服务器在这些功能中未能正确验证文件类型、内容和执行权限。成功利用该漏洞通常会导致系统完全受损、数据外泄或将服务器用作内部网络攻击的跳板 (Pivot Point)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**工具：** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### 应用程序是否提供上传文件的功能？
- [ ] 否 — 不存在文件上传功能  
- [ ] 是 — 存在面向已认证用户的功能  
- [ ] 是 — **未经身份验证**的用户也可使用该功能 *(严重)*  

### 服务器端是否强制执行文件扩展名验证？
- [ ] 是 — 应用了严格的扩展名白名单 (Allowlist)，且**无法**绕过  
- [ ] 是 — 使用了白名单，但**可以**通过大小写敏感性或双重扩展名绕过  
- [ ] 是 — 使用了黑名单 (Blocklist)，其**可以**通过替代扩展名绕过（例如 `.phtml`, `.asa`）  
- [ ] 否 — 服务器端未应用扩展名验证  

### 除扩展名或 MIME 类型外，是否对文件内容进行了验证？
- [ ] 是 — 应用程序验证文件幻数 (Magic Bytes) 并执行深度内容检测  
- [ ] 是 — 应用程序仅检查 `Content-Type` 标头，这**很容易**被伪造  
- [ ] 否 — 应用程序完全依赖文件扩展名进行验证  

### 上传的文件是否存储在具有执行权限的目录中？
- [ ] 否 — 文件存储在专用存储服务（例如 S3）或不可执行目录中  
- [ ] 是 — 文件存储在 Web 服务器上，但已通过配置**禁用**了执行权限  
- [ ] 是 — 文件存储在 Web 可访问目录中，且**启用了**执行权限 *(严重)*  

### 是否可以通过文件名操纵绕过安全过滤器？
- [ ] 否 — 文件名已由服务器清理 (Sanitized) 或随机重新生成  
- [ ] 是 — 可以使用空字节 (Null Bytes)（例如 `shell.php%00.jpg`）绕过过滤器  
- [ ] 是 — 路径遍历 (Path Traversal) 字符（例如 `../../`）可被用于将文件上传到任意目录  

---