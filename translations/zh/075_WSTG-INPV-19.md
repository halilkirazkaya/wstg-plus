## WSTG-INPV-19 — 服务器端请求伪造 (SSRF) 测试

服务器端请求伪造 (Server-Side Request Forgery, SSRF) 发生在攻击者能够诱导 Web 应用程序向内部或外部资源发出未经授权的请求时。通过操纵预期接收 URL、主机名或 IP 地址的参数，攻击者可以绕过防火墙和访问控制列表 (ACL) 等网络层控制，以探测内部网络段、访问云元数据服务 (IMDS) 或与易受攻击的内部 API 和数据库进行交互。这种漏洞通常出现在涉及图像抓取、PDF 生成、Webhooks 或通过 URL 进行文件上传的功能中。从攻击者的角度来看，SSRF 是一个强大的原语 (Primitive)，用于跳转到隔离环境、窃取敏感配置数据，或通过与 Redis 或 Memcached 等内部服务交互来潜在地实现远程代码执行 (Remote Code Execution)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**工具：** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### 应用程序是否处理用户提供的 URL 或主机名？
- [ ] 否 — 没有接收外部 URL 或主机名的功能  
- [ ] 是 — 存在该功能，但输入根据白名单 (Allow-list) **经过严格验证**  
- [ ] 是 — 存在该功能且接收用户提供的 URL  

### 是否对请求的目标应用了验证控制或黑名单？
- [ ] 是 — 强制执行受信任域名/IP 的严格**白名单** (Allow-list)  
- [ ] 是 — 使用了**黑名单** (Deny-list)（例如，封禁 `127.0.0.1`, `169.254.169.254`）  
- [ ] 否 — **未对输入进行验证**或过滤  

### 是否可以通过编码、重定向或 DNS 重绑定绕过过滤器？
- [ ] 否 — 过滤器健壮，并能安全地处理重定向/DNS 解析  
- [ ] 是 — 使用 URL 编码或替代 IP 格式（十六进制、八进制）**可以绕过**  
- [ ] 是 — 通过指向内部目标的 3xx HTTP 重定向 (Redirects) **可以绕过**  
- [ ] 是 — 通过 DNS 重绑定 (DNS Rebinding) 攻击解析为内部 IP **可以绕过**  

### 应用程序能否访问内部元数据服务或本地接口？
- [ ] 否 — 本地网络和云元数据端点**不可达**  
- [ ] 是 — **可以访问**本地主机 (`127.0.0.1`) 或回环 (Loopback) 服务  
- [ ] 是 — **可以访问**云元数据服务（例如 AWS/GCP/Azure IMDS）*(严重)*  

### SSRF 响应的性质是什么？
- [ ] 全盲 (Blind) — 不向用户返回任何响应或元数据  
- [ ] 半盲 (Partial) — 向用户返回响应元数据（响应头、大小、时间耗时）  
- [ ] 完全 (Full) — 内部资源的完整响应体**被反射**给用户  

---