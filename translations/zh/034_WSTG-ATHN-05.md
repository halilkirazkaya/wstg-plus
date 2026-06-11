## WSTG-ATHN-05 — 脆弱的“记住密码”功能测试

“记住我” (Remember Me) 或持久化登录 (Persistent Login) 功能旨在通过在客户端 (Client Side) 存储令牌或凭据，在浏览器重启后保持用户的身份验证状态 (Authenticated State)。如果该功能实现不当——例如在 Cookie 中存储明文密码 (Plaintext Passwords)、弱哈希 (Weak Hashes) 或低熵令牌 (Low-entropy Tokens)——则会使用户面临通过跨站脚本攻击 (Cross-Site Scripting, XSS)、物理访问或网络拦截导致的账户接管 (Account Takeover) 风险。渗透测试人员 (Pentesters) 需评估这些持久化令牌的熵 (Entropy)、应用于存储机制的安全标志 (Security Flags) 以及会话 (Session) 的生命周期，以确保持久化访问的便利性不会绕过关键的安全控制措施，如多因素身份验证 (Multi-factor Authentication, MFA) 或会话失效 (Session Invalidation) 机制。攻击者通常通过收集这些长期有效的令牌，在无需原始凭据 (Credentials) 的情况下获取账户的未经授权访问权限。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果在持久化 Cookie 中存储了明文凭据或未加盐的哈希，或者该令牌绕过了多因素身份验证 (MFA)，则严重程度变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**工具：** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### 应用程序是否实现了“记住我”或“保持登录”功能？
- [ ] 否 — 应用程序中**不存在**此功能  
- [ ] 是 — 存在此功能，并使用加密强度高的不可预测令牌  
- [ ] 是 — 存在此功能，但使用可预测或低熵令牌 *(中)*  
- [ ] 是 — 存在此功能，且存储了明文凭据或 Base64 编码的密码 *(高)*  

### 持久化 Cookie 是否正确应用了安全标志？
- [ ] 是 — 已**应用** `HttpOnly`、`Secure` 和 `SameSite` 标志  
- [ ] 是 — 已应用部分标志，但**缺失** `HttpOnly`  
- [ ] 是 — 已应用部分标志，但在 HTTPS 环境下**缺失** `Secure`  
- [ ] 否 — 持久化 Cookie 未**应用**任何安全标志  

### 在手动注销后，“记住我”令牌是否仍然有效？
- [ ] 否 — 注销后，令牌立即在服务端失效  
- [ ] 是 — 令牌仍然有效，但执行敏感操作时需要密码  
- [ ] 是 — 令牌仍然有效，且允许在**无需**重新身份验证的情况下获得完整的账户访问权限 *(中)*  

### 修改密码后，持久化会话是否终止？
- [ ] 是 — 用户更改密码时，所有持久化令牌均被撤销  
- [ ] 否 — 在**执行**密码重置/更改后，“记住我”令牌仍然有效 *(高)*  

### 在后续访问中，持久化令牌是否绕过了多因素身份验证 (MFA)？
- [ ] 否 — 即使存在“记住我”令牌，仍需要 MFA  
- [ ] 是 — MFA 被绕过，但令牌与特定的设备指纹 (Device Fingerprint) 绑定  
- [ ] 是 — 仅通过任何设备上的持久化 Cookie 即可**完全绕过** MFA *(高)*  

---