## WSTG-ATHN-01 — 测试通过加密通道传输的凭据

测试通过加密通道传输的凭据旨在确保敏感的身份验证数据（如用户名、密码和会话令牌 (Session Tokens)）在传输过程中受到保护，防止被截获。位于网络中的攻击者——例如通过公共 Wi-Fi 或受损内部网络进行的中间人攻击 (Man-in-the-Middle (MitM))——如果未严格执行传输层安全协议 (TLS/SSL)，可以使用数据包嗅探 (Packet Sniffing) 工具捕获明文凭据。这种漏洞通常出现在登录端点 (Login Endpoints)、密码重置表单和 API 身份验证标头 (Authentication Headers) 中，原因是应用程序未能强制执行 HTTPS 或从 HTTP 重定向不当。成功利用此漏洞可使攻击者获得用户账户的完全访问权限，导致用户数据被全面攻破，并可能在应用程序内部进行横向移动 (Lateral Movement)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**工具：** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### 整个身份验证过程是否强制执行 HTTPS？
- [ ] 是 — 已启用 HSTS 且所有流量均被严格强制通过 HTTPS 传输  
- [ ] 是 — 存在到 HTTPS 的重定向，但未启用 HSTS  
- [ ] 否 — 凭据**可以**通过未加密的 HTTP 提交  

### 登录页面和表单是否通过加密通道提供？
- [ ] 是 — 包含登录表单的页面是通过 HTTPS 交付的  
- [ ] 否 — 登录页面通过 HTTP 提供，允许表单操作劫持 (Form-Action Hijacking) 或凭据嗅探  

### 是否为所有敏感会话 Cookie 应用了 `Secure` 标志？
- [ ] 是 — 已为所有身份验证和会话 Cookie **应用**了 `Secure` 标志  
- [ ] 是 — 仅为部分 Cookie **应用**了 `Secure` 标志  
- [ ] 否 — **未应用** `Secure` 标志，导致 Cookie 可能通过未加密的请求泄露 (Exfiltrated)  

### 服务器是否支持弱 TLS 版本或不安全的密码套件 (Cipher Suites)？
- [ ] 否 — 仅**启用**了现代 TLS (1.2+) 和强密码套件  
- [ ] 是 — **支持**遗留版本 (TLS 1.0/1.1) 或弱密码算法 (RC4, DES, CBC)  

### 应用程序是否防止凭据通过未加密通道传输到第三方域名？
- [ ] 是 — 未向外部域名发送敏感数据，或所有外部调用均强制通过 HTTPS 传输  
- [ ] 否 — 身份验证标头或凭据通过 HTTP 发送到第三方端点（例如分析工具或单点登录 (SSO)）  

---