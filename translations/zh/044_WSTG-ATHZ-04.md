## WSTG-ATHZ-04 — 不安全的直接对象引用测试 (Testing for Insecure Direct Object References)

不安全的直接对象引用 (Insecure Direct Object References, IDOR) 发生在应用程序根据用户提供的输入直接访问对象，而未执行授权检查以验证请求者的权限时。此漏洞严重影响机密性和完整性，因为它允许攻击者查看、修改或删除属于其他用户或系统的数据。它通常表现在指向内部数据库键、文件名或账户标识符的 URL 参数、POST 正文数据或 JSON 键中。从攻击者的角度来看，利用 (Exploit) 涉及操纵这些标识符——通常通过递增整数或对 UUID 进行模糊测试 (Fuzzing)——以访问其授权范围之外的资源。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 高 (High) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**工具：** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### 应用程序是否在请求参数中使用直接对象标识符？
- [ ] 否 — 应用程序使用间接引用或加密映射 *(最安全)*  
- [ ] 是 — 应用程序使用不可预测的标识符（例如：长 UUID 或哈希值）  
- [ ] 是 — 应用程序使用可预测的标识符（例如：递增整数或用户名）  

### 对于每个涉及对象标识符的请求，是否都执行了服务器端授权？
- [ ] 是 — 服务器端检查**无法**针对任何标识符被绕过  
- [ ] 是 — 存在服务器端检查，但**可以**通过参数污染 (Parameter Pollution) 或标头篡改绕过  
- [ ] 否 — **未应用**任何授权检查来验证所请求对象的所有权  

### 攻击者是否可以访问或修改属于其他用户的对象（水平越权 / Horizontal IDOR）？
- [ ] 否 — **无法**访问其他用户的数据  
- [ ] 是 — **可以**查看其他用户的数据，但**无法**进行修改  
- [ ] 是 — **可以**同时查看和修改其他用户的数据 *(危急)*  

### 是否可以访问管理级或系统级对象（垂直越权 / Vertical IDOR）？
- [ ] 否 — **无法**访问更高权限的对象  
- [ ] 是 — **可以**访问管理配置或系统文件  

### 应用程序是否通过搜索、元数据或其他端点泄露对象标识符？
- [ ] 否 — 标识符**未被**意外泄露  
- [ ] 是 — **可以**通过二级端点 (API) 或公开个人资料枚举其他用户/对象的标识符  

---