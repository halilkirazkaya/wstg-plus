## WSTG-ATHZ-03 — 权限提升测试 (Testing for Privilege Escalation)

权限提升 (Privilege Escalation) 发生在攻击者利用漏洞获取本应属于更高权限级别或不同身份用户的资源或功能访问权时。在垂直提权 (Vertical Escalation) 中，普通用户尝试访问管理功能；而水平提权 (Horizontal Escalation) 则涉及访问属于具有相同权限级别的另一用户的数据。这些缺陷通常表现在实现不当的访问控制列表 (ACLs)、不安全直接对象引用 (IDOR) 或会话 (Session) 或身份令牌中的参数篡改 (Parameter Tampering) 中。从攻击者的角度来看，这通常是通过操纵数字 ID、修改 Cookie 或 JWTs 中基于角色的参数，或直接调用缺乏服务端授权检查 (Authorization Checks) 的隐藏 API 端点来实现的。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**工具：** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### 应用程序是否实现了多个权限级别或多租户 (Multi-tenancy)？
- [ ] 否 — 应用程序为单用户或单角色系统  
- [ ] 是 — 已定义多个角色或租户，**需要**进行授权测试  

### 是否可能发生水平提权（同级访问）？
- [ ] 否 — 授权检查**已应用**于所有资源标识符和会话所有者  
- [ ] 是 — **可以**通过 IDOR 或参数篡改访问其他用户的数据  
- [ ] 是 — **可能**发生数据泄露，但**无法**修改其他用户的数据  

### 是否可能发生垂直提权（低权限到高权限）？
- [ ] 否 — 管理端点严格执行基于角色的访问控制 (RBAC)  
- [ ] 是 — 低权限用户**可以**通过直接 URL 访问方式访问管理功能  
- [ ] 是 — **可以**通过操纵角色相关的 Header、Cookie 或 JWT Claims 访问管理功能  

### 是否可以通过批量赋值 (Mass Assignment) 或参数污染 (Parameter Pollution) 修改用户角色或权限？
- [ ] 否 — 基于角色的字段严格只读，用户**无法**修改  
- [ ] 是 — 在个人资料更新或注册时，通过包含隐藏参数（例如 `role=admin`）**可以**提升用户权限  

### 授权检查是否在所有 API 版本和 HTTP 方法中一致强制执行？
- [ ] 是 — 控制措施在所有版本和方法中**一致应用**  
- [ ] 否 — 旧版本 API（例如 `/v1/`）或特定 HTTP 方法（例如 `PUT`, `DELETE`, `PATCH`）**绕过了**授权  
- [ ] 否 — 管理功能在 UI 中被隐藏，但在 API 级别对所有用户**开放**  

---