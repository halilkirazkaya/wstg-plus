## WSTG-CONF-11 — 测试云存储

云存储错误配置 (Cloud Storage Misconfigurations) 测试涉及识别和审计可公开访问的对象存储 (Object Storage) 服务，例如 Amazon S3、Azure Blobs 或 Google Cloud Storage。这些资源通常包含敏感数据，包括由于过度宽松的访问控制列表 (ACLs) 或身份与访问管理 (IAM) 策略而暴露的备份、源代码、个人身份信息 (PII) 或配置文件。攻击者通过对 JavaScript 文件、HTML 源代码进行侦察 (Reconnaissance) 以及暴力破解 (Brute-force) 技术来枚举存储桶 (Bucket) 名称，以寻找数据渗出 (Exfiltration) 或未经授权的文件修改入口点。利用此类漏洞可能导致重大影响，包括完整的数据泄露或利用受信任域名分发恶意文件的能力。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重* |

> *如果 PII、凭据或生产环境备份可被访问，或者启用了未经身份验证的写入访问，则严重程度变为“严重 (Critical)”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### 是否已识别并枚举云存储端点（存储桶/容器）？
- [ ] 否 —— 未使用或无法发现云存储端点  
- [ ] 是 —— 通过代码分析或流量监控识别出存储端点  
- [ ] 是 —— 通过命名常规暴力破解发现存储端点  

### 未经身份验证的用户能否列出云存储的内容？
- [ ] 否 —— 存储桶列表功能已**禁用**并返回 403 Forbidden 或 404 Not Found  
- [ ] 是 —— 列表功能已**启用**，但不存在敏感文件  
- [ ] 是 —— 列表功能已**启用**，且敏感文件名/元数据可见  

### 是否可以从识别出的存储桶中下载敏感文件？
- [ ] 否 —— 文件受 IAM 策略保护，或需要有效的身份验证  
- [ ] 是 —— 文件可访问，但需要难以被猜测的签名 URL (Signed URL)  
- [ ] 是 —— 敏感文件（备份、`.env`、PII）可**公开访问**并下载 *(严重)*  

### 是否允许未经身份验证的文件上传或修改？
- [ ] 否 —— **不可能**进行未经身份验证的上传或修改  
- [ ] 是 —— 需要经过身份验证的上传，但通过弱策略条件**可以**进行绕过  
- [ ] 是 —— **可以**进行未经身份验证的文件写入、覆盖或删除 *(严重)*  

### 存储端点上的跨源资源共享 (CORS) 策略是否具有限制性？
- [ ] 是 —— 跨源资源共享 (CORS) 已**禁用**或仅限于特定的受信任源 (Origin)  
- [ ] 否 —— CORS 已**启用**并使用通配符 (Wildcard) `*` 源，允许未经授权的基于 Web 的访问  

---