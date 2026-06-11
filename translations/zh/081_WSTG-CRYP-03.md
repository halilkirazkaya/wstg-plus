## WSTG-CRYP-03 — 测试通过未加密通道传输的敏感信息

通过明文 HTTP 等未加密通道传输敏感数据，会使信息面临通过中间人攻击 (Man-in-the-Middle (MITM)) 被拦截和修改的风险。当 Web 应用程序未能跨所有端点强制执行传输层安全协议 (Transport Layer Security (TLS)) 时，通常会出现此类漏洞，特别是在遗留组件、登录表单或从 HTTP 到 HTTPS 的初始过渡期间。从攻击者的角度来看，这允许在受损或不可信的网络段（如公共 Wi-Fi）上对身份验证凭据、会话令牌 (Session Token) 和个人可识别信息 (Personally Identifiable Information (PII)) 进行被动嗅探。确保所有敏感数据都封装在强大的 TLS 隧道内，对于维护用户交互的机密性和完整性以及防止会话劫持 (Session Hijacking) 至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 中 |

> *如果身份验证凭据、会话标识符或 PII 通过明文传输，严重程度将变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### 应用程序是否允许通过未加密的 HTTP 与敏感端点进行通信？
- [ ] 否 — 所有应用程序流量都严格强制通过 HTTPS *(最安全)*  
- [ ] 是 — 非敏感页面可以通过 HTTP 访问，但敏感端点**不可以**  
- [ ] 是 — 敏感端点（登录、结账、个人资料）**可以**通过未加密的 HTTP 访问 *(高风险)*  

### 是否存在从 HTTP 到 HTTPS 的全局重定向？
- [ ] 是 — 应用程序对所有 HTTP 请求执行 301/302 重定向到 HTTPS  
- [ ] 是 — 仅针对特定的敏感端点执行重定向  
- [ ] 否 — **未应用**从 HTTP 到 HTTPS 的重定向  

### 是否实现了 HTTP 严格传输安全 (HTTP Strict Transport Security (HSTS))？
- [ ] 是 — HSTS 已**启用**，具有较长的 `max-age` 且包含 `includeSubDomains` 和 `preload` 指令  
- [ ] 是 — HSTS 已**启用**，但缺少 `includeSubDomains` 或 `preload` 指令  
- [ ] 否 — 应用程序服务器上**未启用** HSTS  

### 敏感 Cookie 是否已防止明文传输？

| Cookie 名称 | 存在 Secure 标志 | 缺失 Secure 标志 |
|-------------|:-------------------:|:------------------:|
| `SessionID` |                     |                    |
| `AuthToken` |                     |                    |
| `CSRF-Token`|                     |                    |

### 应用程序是否通过未加密通道向第三方 API 传输敏感数据？
- [ ] 否 — 所有外部 API 调用均通过加密 (HTTPS) 隧道执行  
- [ ] 是 — 一些非敏感的遥测数据通过 HTTP 发送  
- [ ] 是 — API 密钥、PII 或凭据**已**通过未加密的 HTTP 发送到第三方  

---