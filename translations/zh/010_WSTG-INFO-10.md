## WSTG-INFO-10 — 映射应用程序架构 (Map Application Architecture)

映射应用程序架构涉及识别支持 Web 应用程序的底层技术、基础设施组件和数据流路径。通过分析 HTTP 响应头 (HTTP response headers)、错误消息、Cookie 格式和文件扩展名，测试人员可以重建 Web 服务器、应用程序服务器、数据库和所使用的第三方集成的图表。这些知识对于定制后续攻击至关重要，因为它允许选择特定于平台的漏洞利用程序 (Exploit)，并识别潜在的瓶颈或配置错误的中间设备，如负载均衡器 (Load Balancers) 和 Web 应用防火墙 (WAF)。攻击者利用此结构图从通用扫描转向针对特定软件栈 (Software stack) 及其互连组件的定向攻击。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重程度** | 信息性 / 低* |

> *如果泄露了内部网络拓扑或私有 IP 地址，严重程度将变为中。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### 是否可以准确识别 Web 服务器和应用程序服务器技术？
- [ ] 否 — 由于有效的加固 (Hardening) 或混淆 (Obfuscation)，无法进行识别  
- [ ] 是 — 技术类型已知，但无法确定具体版本  
- [ ] 是 — 技术和具体版本通过响应头或文件特征完全泄露 *(信息性)*  

### 在通信路径中是否可以检测到中间设备（WAF、负载均衡器、代理）？
- [ ] 否 — 未检测到中间设备，或它们是完全透明的  
- [ ] 是 — 通过响应时间或行为怀疑其存在，但身份**未**确认  
- [ ] 是 — 通过特定的响应头或行为识别出具体产品（例如 Cloudflare, Nginx, F5）  

### 应用程序是否泄露了有关其内部目录结构或网络拓扑的信息？
- [ ] 否 — 内部路径和 IP 地址**未**泄露  
- [ ] 是 — 内部路径在错误消息或堆栈追踪 (Stack traces) 中泄露，但内部 IP **未**泄露  
- [ ] 是 — 内部目录结构和私有 IP 地址均被泄露  

### 是否可以从应用程序行为推断出后端数据库的类型和版本？
- [ ] 否 — 无法确定数据库详情  
- [ ] 是 — 通过错误消息或行为推断出数据库类型（例如 PostgreSQL, MSSQL）  
- [ ] 是 — 数据库类型和版本在响应体或响应头中明确泄露  

### 应用程序是否托管在可识别的云服务提供商或特定的第三方内容分发网络 (CDN) 上？
- [ ] 否 — 托管基础设施无法识别  
- [ ] 是 — 识别出云提供商（AWS, Azure, GCP），但未识别出具体服务  
- [ ] 是 — 特定云服务（例如 S3 buckets, Lambda, Azure Functions）已被映射且可访问  

---