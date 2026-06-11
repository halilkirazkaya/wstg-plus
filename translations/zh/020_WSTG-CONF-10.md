## WSTG-CONF-10 — 子域名接管 (Subdomain Takeover)

当 DNS 条目（通常是 CNAME，但也偶尔包括 A 或 MX 记录）指向第三方云提供商或托管服务上已停用或不存在的资源时，就会发生子域名接管 (Subdomain Takeover)。攻击者通过寻找来自 AWS、GitHub 或 Azure 等提供商的特定错误消息（表明资源不再被占用）来识别这些“悬空”DNS 记录 (Dangling DNS records)。通过在提供商平台上注册该弃用的资源名称，攻击者可以获得对子域名的完全控制权，从而允许他们托管恶意内容、窃取作用域在根域名的会话 Cookie (Exfiltrate Session Cookies)，并绕过内容安全策略 (Content Security Policy) 或 CORS 保护。这种漏洞特别危险，因为它利用了与组织合法域名结构相关的信任，从而实现高影响力的网络钓鱼 (Phishing) 和数据窃取。


| 字段 | 取值 |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重* |

> *如果子域名可用于劫持来自根域名的会话 Cookie 或绕过 SSO/OAuth 重定向，严重程度将变为“严重”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**工具：** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### 应用程序是否通过子域名利用第三方托管或云服务？
- [ ] 否 — 所有子域名均指向组织控制的 IP 空间  
- [ ] 是 — 子域名指向第三方提供商（S3, GitHub Pages, Heroku 等）  

### 是否存在指向不存在资源的“悬空”DNS 记录？
- [ ] 否 — 所有识别出的 DNS 记录均解析为受控的活动资源  
- [ ] 是 — 存在指向提供商处**不再存在**的资源的 CNAME 或 A 记录  
- [ ] 是 — DNS 记录解析为 NXDOMAIN 或特定于提供商的错误页面 *（例如 "NoSuchBucket"）*  

### 识别出的悬空资源是否可以被成功申领？
- [ ] 否 — 提供商具有防止申领弃用名称的保护措施，或者需要进行账户验证  
- [ ] 是 — 该资源名称**可以**被任何用户在第三方平台上注册  

### 根域名是否使用了可能被攻破的“域名”作用域 (Domain-scoped) Cookie？
- [ ] 否 — Cookie 严格限定在特定的子域名作用域，或使用了 `Host-Only` 标志  
- [ ] 是 — 敏感 Cookie（会话 ID、JWT）的作用域设定为父域名，并且**可以**通过被劫持的子域名窃取  

### 该子域名是否可被用于绕过安全响应头 (Security Headers) 或身份验证流程？
- [ ] 否 — 识别出的子域名不存在安全依赖关系  
- [ ] 是 — 该子域名在 CSP 的 `script-src` 或 `connect-src` 指令白名单中  
- [ ] 是 — 该子域名是 OAuth 或 SSO 配置中的有效重定向 URI (Redirect URI)  

---