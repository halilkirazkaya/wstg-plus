## WSTG-BUSL-08 — 测试上传非预期文件类型

上传非预期文件类型允许攻击者通过提交应用程序不打算处理的文件来绕过业务逻辑 (Business Logic) 限制，这可能导致远程代码执行 (Remote Code Execution, RCE)、跨站脚本攻击 (Cross-Site Scripting, XSS) 或拒绝服务攻击 (Denial of Service, DoS)。攻击者通过修改文件扩展名、MIME 类型 (MIME types) 和魔术字节 (Magic Bytes) 来探测这些漏洞，以诱使服务端验证逻辑接受恶意内容。这种情况通常发生在个人资料图片上传、文档管理系统或工单附件中，因为这些场景在后端 (Back-end) 缺乏足够的验证。如果上传的文件被执行，成功的利用可能会危及宿主服务器；或者如果文件以错误的 Content-Type 返回给其他用户，则会导致客户端攻击。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**工具：** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### 应用程序是否将文件上传限制在特定的扩展名集合中？
- [ ] 是 — 强制执行严格的扩展名白名单 (Whitelist)，且**无法绕过**  
- [ ] 是 — 存在扩展名白名单，但**可以通过**双扩展名 (Double Extensions) 或空字节 (Null Bytes) 绕过  
- [ ] 否 — 任何文件扩展名**都可以**被上传  

### 除了文件扩展名之外，是否还对文件内容进行了验证？
- [ ] 是 — 服务端逻辑会校验魔术字节并执行深度内容检测 (Deep Content Inspection)  
- [ ] 是 — 服务端逻辑仅通过 `Content-Type` 标头验证 MIME 类型  
- [ ] 否 — 在扩展名检查之后**未应用**任何内容验证  

### 攻击者是否可以通过上传 Web Shell 或脚本来执行代码？
- [ ] 否 — 上传的文件存储在不可执行目录或存储桶 (Storage Bucket)（如 S3/Azure Blob）中  
- [ ] 是 — 文件存储在 Web 可访问目录中，但通过服务器配置**禁用了**执行功能  
- [ ] 是 — 上传的恶意脚本**可以**通过可预测的 URL 路径直接执行 *(严重)*  

### 是否可以通过上传的文件类型进行如 XSS 的客户端攻击？
- [ ] 否 — 文件在提供下载时带有 `Content-Disposition: attachment` 和 `X-Content-Type-Options: nosniff`  
- [ ] 是 — SVG 或 HTML 文件**可以**被上传并在浏览器中渲染，导致 XSS  
- [ ] 是 — 图像元数据 (EXIF) **被处理**并反映在 UI 中，且未经过无害化处理 (Sanitization)  

---