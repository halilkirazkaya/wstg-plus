## WSTG-CRYP-01 — 弱传输层安全测试 (Testing for Weak Transport Layer Security)

弱传输层安全测试涉及分析 SSL/TLS 的实现和配置，以确保数据在传输过程中的机密性 (Confidentiality) 和完整性 (Integrity)。攻击者利用过时协议（如 SSLv2, TLS 1.0）、弱密码套件 (Cipher Suites，如 NULL、RC4 或匿名套件) 以及无效或签名强度弱的证书等配置错误来执行中间人 (Man-in-the-Middle, MitM) 攻击。此项测试通常针对建立加密通信的应用程序 Web 服务器或负载均衡器 (Load Balancer) 端点。成功的利用可使攻击者拦截敏感数据、劫持用户会话，或通过协议降级 (Protocol Downgrade) 或流量解密将连接降级至不安全状态。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 中* |

> *如果传输敏感数据（个人身份信息 (PII)、凭据、会话令牌 (Session Tokens)），严重程度为“高”；对于一般性信息流量，严重程度为“中”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### 是否支持过时或不安全的 TLS/SSL 协议？
- [ ] 否 — 仅**启用**了 TLS 1.2 和 TLS 1.3  
- [ ] 是 — **启用**了 TLS 1.0 或 TLS 1.1  
- [ ] 是 — **启用**了 SSLv2 或 SSLv3 *(危急)*  

### 服务器是否接受弱密码套件或不安全的密码套件？
- [ ] 否 — 仅**支持**强效、现代的算法（如 AES-GCM, ChaCha20）  
- [ ] 是 — **支持**弱加密算法（如 3DES, RC4）或低位长度的算法  
- [ ] 是 — **支持** NULL、匿名 (ADH) 或出口级 (Export) 密码套件 *(危急)*  

### 服务器证书是否有效且受信任？
- [ ] 是 — 证书有效，与域名匹配，且由**受信任的 CA** 签名  
- [ ] 否 — 证书已**过期**或使用了**弱签名算法**（如 SHA-1）  
- [ ] 否 — 证书为**自签名**或存在**域名不匹配**  

### 服务器是否支持安全重新协商 (Secure Renegotiation) 并防止降级攻击？
- [ ] 是 — **支持**安全重新协商且**启用**了 TLS Fallback SCSV  
- [ ] 否 — **不支持**安全重新协商，可能允许 MitM 注入  
- [ ] 否 — 服务器易受 **POODLE** 或 **ROBOT** 攻击  

### 是否正确实现了 HTTP 强制传输安全 (HTTP Strict Transport Security, HSTS)？

| 标头 | 存在 | 缺失 |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] 是 — 存在 HSTS 标头，具有较长的 `max-age` 且**启用**了 `includeSubDomains`  
- [ ] 是 — 存在 HSTS 标头但缺失 `includeSubDomains` 或 `max-age` **过短**  
- [ ] 否 — **不存在** HSTS 标头  

---