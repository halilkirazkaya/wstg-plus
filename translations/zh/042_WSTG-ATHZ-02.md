## WSTG-ATHZ-02 — 绕过授权方案测试 (Testing for Bypassing Authorization Schema)

绕过授权方案 (Bypassing Authorization Schema) 发生在应用程序未能强制执行访问控制 (Access Controls) 时，导致攻击者能够访问其预期权限之外的资源或功能。这种漏洞通常表现为水平越权 (Horizontal Privilege Escalation)，即攻击者访问属于同级别其他用户的数据；或者垂直越权 (Vertical Privilege Escalation)，即低权限用户获得了管理员权限。攻击者通过操纵请求参数（如资源 ID）、修改会话令牌 (Session Tokens) 或利用 `X-Original-URL` 等 HTTP 标头 (HTTP Headers) 来绕过基于路径的限制，从而利用这些缺陷。确保健全的授权机制对于维护数据机密性以及防止可能危及整个系统的未经授权的管理操作至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 高 (High) / 严重 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**工具：** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### 同一角色的用户之间是否防止了水平越权？
- [ ] 是 — 授权检查**已应用于**每个资源请求，且**无法绕过**  
- [ ] 是 — 授权检查**已应用于**，但可以通过不安全的直接对象引用 (IDOR) 或参数篡改 (Parameter Tampering) 绕过  
- [ ] 否 — 用户只需更改资源 ID **即可**访问属于其他用户的数据 *(高)*  

### 低权限用户是否防止了垂直越权？
- [ ] 否 — 应用程序只有一个权限级别  
- [ ] 是 — 低权限用户**无法**访问管理功能  
- [ ] 是 — 管理功能在 UI 中已隐藏，但通过直接 URL **可以**访问  
- [ ] 否 — 低权限用户通过操纵角色或权限**可以**执行管理操作 *(严重)*  

### 是否可以使用 HTTP 谓词篡改来绕过授权？
- [ ] 否 — 无论使用何种 HTTP 方法，都会强制执行访问控制  
- [ ] 是 — 更改方法（例如从 `GET` 更改为 `POST` 或 `PUT`）**会绕过**授权检查  

### 管理端点或受限端点是否可以在没有任何身份验证的情况下访问？
- [ ] 否 — 所有敏感端点都需要有效的会话和适当的权限  
- [ ] 是 — 如果攻击者知道直接 URL，则可以访问某些管理端点  
- [ ] 是 — **可以**未经身份验证 (Unauthenticated) 访问敏感功能 *(严重)*  

### HTTP 标头是否允许规避基于路径的授权？
- [ ] 否 — 应用程序**不容易**受到基于标头的绕过攻击  
- [ ] 是 — `X-Original-URL`、`X-Rewrite-URL` 或 `X-Forwarded-For` 等标头**可被用于**绕过访问控制  

---