## WSTG-INFO-09 — 指纹识别 (Fingerprint) Web 应用程序

指纹识别 Web 应用程序是识别特定框架 (Framework)、内容管理系统 (CMS) 或底层技术栈 (Technology Stack) 及其相关版本的过程。这一侦察 (Reconnaissance) 阶段至关重要，因为它允许攻击者将识别出的组件与已知漏洞 (CVE) 和公开漏洞利用程序 (Exploit) 进行映射，从而显著缩小攻击面 (Attack Surface)。信息通常通过 HTTP 响应头、独特的文件路径、Cookie 名称、HTML 源代码元数据以及静态资源的加密哈希来收集。通过准确确定技术栈，攻击者可以从通用的自动化扫描转向针对特定版本弱点的高度针对性漏洞利用。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重性** | 低 / 信息性 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### HTTP 响应头是否泄露了技术栈或版本号？
- [ ] 否 — 敏感响应头已被移除或包含通用值  
- [ ] 是 — 存在诸如 `X-Powered-By`、`Server` 或 `X-AspNet-Version` 的默认响应头  
- [ ] 是 — 响应头泄露了 Web 服务器或应用框架的特定过时版本  

### 是否可以通过独特的文件路径或命名约定识别应用框架或 CMS？
- [ ] 否 — 文件路径已混淆，且不会揭示底层技术  
- [ ] 是 — 常见的目录结构（例如 `/wp-content/`、`/_next/`、`/node_modules/`）可见  
- [ ] 是 — 诸如 `robots.txt`、`humans.txt` 或 `sitemap.xml` 等特定文件包含技术特定的元数据  

### HTML 源代码或静态资源中是否暴露了特定的版本号？
- [ ] 否 — 在注释或资源参数中未发现版本信息  
- [ ] 是 — 存在 HTML 注释或 Meta 标签（例如 `<meta name="generator" content="...">`）  
- [ ] 是 — 版本字符串被附加到 CSS/JS 文件名（例如 `jquery.js?v=1.12.4`）且无法禁用  

### 默认错误页面或管理界面是否揭示了软件环境？
- [ ] 否 — 应用程序对所有状态码均使用自定义的通用错误页面  
- [ ] 是 — 启用了 Web 服务器或框架的默认错误页面，并泄露了版本数据  
- [ ] 是 — 后台管理登录门户（例如 `/wp-admin`、`/phpmyadmin`）可访问且可识别  

### Cookie 是否揭示了会话管理框架？
- [ ] 否 — 会话 Cookie 使用通用或自定义名称  
- [ ] 是 — 使用了技术特定的 Cookie 名称（例如 `PHPSESSID`、`JSESSIONID`、`ASPSESSIONID`）  

---