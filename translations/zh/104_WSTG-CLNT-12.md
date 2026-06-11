## WSTG-CLNT-12 — 测试浏览器存储 (Test Browser Storage)

测试浏览器存储涉及分析应用程序如何利用客户端机制，如本地存储 (LocalStorage)、会话存储 (SessionStorage)、IndexedDB 和 WebSQL 来持久化数据。如果不安全地存储敏感信息——包括个人可识别信息 (Personally Identifiable Information, PII)、身份验证令牌 (Authentication Tokens) 或会话标识符——在攻击者实现跨站脚本 (Cross-Site Scripting, XSS) 或获得对用户设备的物理访问时，这些信息可能会被泄露。渗透测试人员必须确定数据是否以明文形式存储，注销后是否仍可访问，以及存储生命周期是否得到妥善管理以防止数据泄漏。从攻击者的角度来看，这些存储机制是窃取状态维护信息的理想目标，从而实现会话劫持 (Session Hijacking) 或长期账户持久化。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果会话令牌或敏感 PII 存储在 LocalStorage 中且可通过 XSS 访问，则严重程度变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### 应用程序是否在浏览器存储中存储敏感信息？
- [ ] 否 — 应用程序不使用浏览器存储，或仅存储非敏感的 UI 状态  
- [ ] 是 — 存储了敏感数据，但使用强大的客户端加密技术进行了加密  
- [ ] 是 — 敏感数据（令牌、PII、机密）以明文 (Cleartext) 形式存储 *(中)*  

### 存储的数据是否可被未经授权的脚本访问（XSS 风险）？
- [ ] 否 — 数据存储在 HttpOnly Cookie 中（而非浏览器存储），或未利用存储  
- [ ] 是 — 使用了 LocalStorage/SessionStorage，使得数据可通过任何 XSS 有效负载 (Payload) 完全访问 *(高)*  

### 用户注销时，浏览器存储是否已被妥善清除？
- [ ] 是 — 在注销过程中，所有与应用程序相关的存储均被明确清除  
- [ ] 否 — 注销后存储依然存在，但不包含敏感会话数据  
- [ ] 否 — 会话终止后，身份验证令牌或 PII 仍保留在存储中 *(中)*  

### 应用程序是否对大型/敏感数据集使用 IndexedDB 或 WebSQL？
- [ ] 否 — 未使用这些存储机制  
- [ ] 是 — 数据已存储，且在 DOM 中渲染之前经过了妥善的清理 (Sanitized)  
- [ ] 是 — 敏感数据集以明文形式存储在 IndexedDB/WebSQL 结构中  

### 存储数据的完整性是否可以被操纵以影响应用程序逻辑？
- [ ] 否 — 应用程序在处理存储中的数据之前会对其进行验证或签名  
- [ ] 是 — 修改存储值（例如：用户角色、标志）是可能的，并会导致应用程序行为发生改变  

---