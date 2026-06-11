## WSTG-ATHZ-01 — 目录遍历与文件包含测试 (Testing Directory Traversal File Include)

当应用程序使用用户可控的输入来构建文件或目录路径，且未进行充分的验证或过滤 (Sanitization) 时，就会产生目录遍历 (Directory Traversal) 和文件包含 (File Inclusion) 漏洞。攻击者通过注入诸如 `../` 之类的序列来利用这些缺陷，从而跳出预定目录，可能访问到敏感的系统文件、配置数据或应用程序源代码。在涉及本地文件包含 (Local File Inclusion, LFI) 或远程文件包含 (Remote File Inclusion, RFI) 的更严重情况下，攻击者可以通过包含恶意脚本或利用日志投毒 (Log Poisoning) 和 PHP 封装器 (PHP Wrappers) 来实现远程代码执行 (Remote Code Execution, RCE)。这些漏洞通常存在于用于动态内容加载、模板引擎 (Template Engines) 或图像获取端点 (Endpoints) 的参数中，这些端点的服务端逻辑负责处理文件系统路径。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**工具：** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### 参数是否接收文件名或路径用于服务端处理？
- [ ] 否 — 似乎没有参数与文件系统交互  
- [ ] 是 — 存在参数，但使用了严格的文件标识符白名单 (Allowlist)  
- [ ] 是 — 参数接收直接的文件名或路径  

### 输入是否经过过滤以防止目录遍历序列？
- [ ] 是 — 输入已根据严格的白名单进行验证，遍历序列**不可行**  
- [ ] 是 — 输入通过删除 `../` 序列进行了过滤，但**可能**存在递归绕过 (Recursive Bypass)  
- [ ] 否 — 路径相关的输入**未应用**任何过滤或验证  

### 是否可以通过遍历序列访问受限目录之外的文件？
- [ ] 否 — 应用程序或操作系统阻止了对定义范围之外文件的访问  
- [ ] 是 — **可以**访问 Web 根目录 (Web Root) 内的文件  
- [ ] 是 — **可以**访问敏感系统文件（例如：`/etc/passwd`，`C:\Windows\win.ini`）*（严重）*  

### 服务器是否允许包含远程 URL (RFI)？
- [ ] 否 — 远程文件包含在服务器/应用程序级别已**禁用**  
- [ ] 是 — **可以**包含远程文件，但执行**不可行**  
- [ ] 是 — **可以**在服务器上包含并执行远程文件*（严重）*  

### 是否可以使用编码或特殊字符绕过过滤器？
- [ ] 否 — 过滤器能有效处理各种编码和空字节 (Null Bytes)  
- [ ] 是 — **可能**通过 URL 编码、双重 URL 编码或 16 位 Unicode 实现绕过  
- [ ] 是 — **可能**通过空字节注入 (Null Byte Injection, `%00`) 或文件系统封装器（例如：`php://filter`）实现绕过  

---