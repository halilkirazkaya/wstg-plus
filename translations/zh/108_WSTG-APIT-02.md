## WSTG-APIT-02 — API 失效的对象级授权

失效的对象级授权 (Broken Object Level Authorization, BOLA)，也被称为不安全的直接对象引用 (Insecure Direct Object Reference, IDOR)，当 API 未能验证用户是否具有访问或操作由 ID 标识的特定资源的适当权限时，就会发生此漏洞。攻击者通过系统地枚举或猜测请求路径、查询参数或 JSON 正文中的标识符（例如数字 ID 或 UUID）来利用此漏洞，从而访问属于其他用户的数据。这种漏洞是现代 API 安全中最常见且影响最大的问题，可能导致大规模数据外泄、未经授权的修改或完全接管账户。从攻击者的角度来看，目标是识别接受对象标识符的端点 (Endpoint)，并测试服务器端授权逻辑是否缺失，或者是否可以通过操纵这些 ID 来绕过该逻辑。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 紧急 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**工具：** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### 在 API 请求中，对象标识符是否可预测或可枚举？
- [ ] 否 — 标识符较长、随机且符合密码学安全（例如 UUIDv4）  
- [ ] 是 — 标识符是连续的整数（例如 `101`, `102`）  
- [ ] 是 — 标识符遵循可预测的模式，或源自公开信息  

### API 是否对每个请求执行对象所有权的服务器端验证？
- [ ] 是 — 每个请求都应用了授权检查，且**无法绕过**  
- [ ] 是 — 应用了检查，但可以通过参数污染 (Parameter Pollution) 或方法隧道 (Method Tunneling) **绕过**  
- [ ] 否 — 应用程序仅依赖用户已通过身份验证，而不检查特定资源的所有权  

### 是否可以通过更改标识符来访问或修改另一个用户的资源？
- [ ] 否 — **无法**未经授权访问其他用户的资源  
- [ ] 是 — **可能**存在未经授权的读取访问（水平 BOLA / Horizontal BOLA）  
- [ ] 是 — **可能**存在未经授权的修改或删除（水平 BOLA）  

### API 是否允许通过替换 ID 来访问管理级或系统级对象？
- [ ] 否 — 管理资源受二级授权层保护  
- [ ] 是 — **可能**访问或修改系统级资源（垂直 BOLA / Vertical BOLA）  

### 是否可以通过将标识符移动到请求的不同部分来绕过授权检查？
- [ ] 否 — 无论参数位置如何，授权逻辑都是一致的  
- [ ] 是 — 当 ID 从 URL 路径移动到 JSON 正文或请求头 (Headers) 时，**可以绕过**  
- [ ] 是 — 当提供多个 ID 且服务器处理了未经授权的 ID 时，**可以绕过**  

---