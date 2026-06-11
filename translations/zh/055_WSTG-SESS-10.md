## WSTG-SESS-10 — 测试 JSON Web Tokens (JWT)

JSON Web Tokens (JWT) 常用于无状态会话管理（Session Management）和身份传播（Identity Propagation），但其安全性完全依赖于签名算法（Signing Algorithms）的正确实现和严格的声明验证（Claim Verification）。攻击者经常尝试通过利用 “none” 算法缺陷、暴力破解（Brute Force）弱 HS256 密钥，或执行算法切换攻击（Algorithm-switching attacks，即使用非对称公钥作为对称 HMAC 密钥）来绕过身份验证。这些漏洞通常体现在会话 Cookie 或授权头（Authorization headers）中，可能允许攻击者在不破坏令牌加密完整性的情况下，通过修改 `role` 或 `user_id` 等声明来提升权限（Escalate privileges）或冒充（Impersonate）任何用户。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**工具：** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### 服务器是否接受 "none" 算法？
- [ ] 否 — 服务器**拒绝**在标头（Header）中使用 `alg: none` 的令牌  
- [ ] 是 — 服务器**接受**使用 `alg: none` 且签名为空的令牌 *(严重)*  

### JWT 签名如何验证以及如何防止暴力破解？
- [ ] 是 — 使用了强非对称密钥（Asymmetric keys，如 RS256/ES256）或高熵（High-entropy）的 HMAC 密钥  
- [ ] 是 — 使用了 HS256，但由于熵值低或使用了默认值，密钥**可以**被暴力破解  
- [ ] 否 — **未应用**签名验证，或者**可以**通过算法切换（从 RS256 切换到 HS256）绕过验证  

### 后端是否严格强制执行标准声明（exp, nbf, iat）？
- [ ] 是 — 存在 `exp`（过期时间）且**无法**被绕过  
- [ ] 是 — 存在 `exp`，但服务器在验证期间**未强制**执行  
- [ ] 否 — **缺失**或忽略了过期声明，从而允许无限期的会话重放（Session Replay）  

### 该实现是否防止了基于标头的密钥注入 (kid, jku, x5u)？
- [ ] 是 — 标头已经过清理（Sanitized），仅**允许**受信任的内部密钥/来源  
- [ ] 否 — `kid` (Key ID) 标头容易受到目录遍历（Directory Traversal）或 SQL 注入 (SQL Injection) 攻击，从而指向已知文件  
- [ ] 否 — `jku` 或 `x5u` 标头**可以**被指向攻击者控制的 URL 以加载恶意密钥  

---