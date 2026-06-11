## WSTG-INFO-08 — Web 应用程序框架指纹识别 (Fingerprint Web Application Framework)

Web 应用程序框架指纹识别 (Fingerprinting) 涉及识别用于构建和提供应用程序服务的特定技术栈 (Technology stack) 和软件版本。攻击者通过分析 HTTP 响应头 (HTTP response headers)、框架特定的 Cookie、可预测的文件结构以及唯一的 JavaScript 伪像 (Artifacts)，来判断目标是否运行在 React、Angular、Django 或 Spring 等平台上。这一侦察 (Reconnaissance) 阶段至关重要，因为它允许测试人员将攻击面 (Attack surface) 缩小到该框架特有的已知漏洞 (CVE) 和常见配置缺陷。通过了解底层架构，攻击者可以从通用测试转向针对性极强的利用 (Exploit) 策略。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重程度** | 信息 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### HTTP 响应头是否泄露了框架名称或版本？
- [ ] 否 — `X-Powered-By` 或 `Server` 等响应头**不存在**或已进行脱敏处理 (Sanitized)  
- [ ] 是 — 框架名称存在，但版本**已隐藏**  
- [ ] 是 — 框架名称和版本**均已泄露**  

### 框架特定的 Cookie 或目录结构是否可识别？
- [ ] 否 — 常见的框架伪像（如 `.js` 路径、Cookie 名称）已混淆或重命名  
- [ ] 是 — 框架特定的 Cookie（如 `JSESSIONID`, `PHPSESSID`, `csrftoken`）**可**识别  
- [ ] 是 — 目录结构（如 `/wp-content/`, `_next/static/`）**可见**并确认了框架  

### 错误消息或源代码注释是否泄露了框架细节？
- [ ] 否 — 错误处理是通用的，且注释已被移除  
- [ ] 是 — 框架特定的堆栈跟踪 (Stack traces) **已启用**且在响应中可见  
- [ ] 是 — HTML 源代码包含识别框架的注释或元数据标签 (Metadata tags)  

### 识别出的框架版本是否存在已知漏洞？
- [ ] 否 — 识别出的框架已是最新版本，且**无已知**公开漏洞  
- [ ] 是 — 框架版本过旧，**可以**识别出特定的 CVE  

---