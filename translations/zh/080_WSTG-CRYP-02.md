## WSTG-CRYP-02 — Testing for Padding Oracle (填充预言机测试)

填充预言机 (Padding Oracle) 漏洞发生在应用程序的解密过程中泄露了关于解密密文的填充 (Padding) 是否有效的信息。通过观察这些侧信道 (Side-channel) 响应——如不同的 HTTP 状态码、特定的错误消息或响应时间差异——攻击者可以在不知道加密密钥的情况下解密数据，并可能重新加密任意明文。这种漏洞通常出现在使用密码块链接 (CBC) 模式和 PKCS#7 填充的分组密码 (Block Cipher) 的加密会话 Cookie、身份验证令牌 (Authentication Token) 或 URL 参数中。利用此类漏洞会导致机密性 (Confidentiality) 完全丧失，并可能通过构造伪造的加密块导致完整性 (Integrity) 受损。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 高 / 关键* (High / Critical*) |

> *如果漏洞存在于会话管理 (Session Management)、身份令牌或 PII 加密中，严重程度为“关键”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### 敏感数据是否使用了 CBC 模式的分组密码？
- [ ] 否 —— 应用程序使用了认证加密 (Authenticated Encryption) (AES-GCM/ChaCha20-Poly1305)  
- [ ] 是 —— 应用程序使用了分组密码，但**不需要**填充 (例如流密码 (Stream Cipher) 或 CTR 模式)  
- [ ] 是 —— 应用程序使用了 CBC 模式且**应用了**填充  

### 服务器是否通过侧信道泄露了填充的有效性？
- [ ] 否 —— 服务器返回统一的错误消息和一致的响应时间  
- [ ] 是 —— 服务器针对填充错误返回不同的 HTTP 状态码 (例如 200 与 500)  
- [ ] 是 —— 服务器返回特定的错误消息 (例如 "Invalid Padding"、"Decryption failed")  
- [ ] 是 —— 服务器在解密失败期间表现出可测量的响应时间差异  

### 是否可以对密文进行自动化解密？
- [ ] 否 —— 侧信道不够稳定，无法利用  
- [ ] 是 —— 密文**可以**被部分解密 (最后几个块)  
- [ ] 是 —— 使用 `PadBuster` 等工具**可以实现**完整的明文恢复  

### 攻击者是否可以执行位翻转或伪造有效的密文？
- [ ] 否 —— 完整性检查 (HMAC) 在解密**之前**已完成验证  
- [ ] 是 —— 不存在完整性检查，但载荷 (Payload) 构造不可行  
- [ ] 是 —— **可以**构造任意密文，从而导致权限提升 (Privilege Escalation) 或数据注入 (Data Injection)  

---