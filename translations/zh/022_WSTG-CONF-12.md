## WSTG-CONF-12 — 内容安全策略 (Content Security Policy, CSP)

内容安全策略 (Content Security Policy, CSP) 是一种关键的深度防御机制，通过 HTTP 响应标头实现，旨在减轻跨站脚本 (Cross-Site Scripting, XSS)、点击劫持 (Clickjacking) 和数据注入 (Data injection) 等攻击风险。通过定义允许加载哪些动态资源，配置良好的 CSP 可以限制攻击者执行未授权脚本或将敏感数据外泄到外部域的能力。渗透测试人员会评估该策略是否存在过度宽泛的指令、是否使用了 `'unsafe-inline'` 等不安全的关键词，以及是否依赖于可能导致绕过 (Bypass) 的不安全内容分发网络 (CDNs)。配置错误或缺失 CSP 本身并不会直接产生漏洞，但由于未能提供二次保护层，会显著增加成功注入攻击的影响。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 低 / 中 (Low / Medium) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**工具：** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### 应用程序响应中是否存在内容安全策略 (CSP) 标头？
- [ ] 是 — `Content-Security-Policy` 标头**已存在**并**已强制执行**  
- [ ] 是 — `Content-Security-Policy-Report-Only` **已存在**用于测试  
- [ ] 否 — 响应中**不存在** CSP 标头  

### `script-src` 或 `default-src` 指令是否得到了妥善限制？
- [ ] 是 — 指令使用严格的白名单、Nonce 或 Hash，且**无法绕过**  
- [ ] 是 — 指令**已妥善配置**，但白名单包含已知的可绕过 CDNs  
- [ ] 否 — 指令使用通配符 (`*`) 或 `data:` 方案，导致**可能被绕过**  

### 该策略是否允许执行不安全的内联脚本或 eval？
- [ ] 否 — **不存在** `'unsafe-inline'` 和 `'unsafe-eval'`  
- [ ] 是 — **存在** `'unsafe-inline'`，但受到 `nonce` 或 `hash` 保护  
- [ ] 是 — **启用了** `'unsafe-inline'` 或 `'unsafe-eval'` 且无额外保护 *(高危)*  

### 应用程序是否通过 `frame-ancestors` 指令防御点击劫持？
- [ ] 是 — `frame-ancestors` 设置为 `'none'` 或 `'self'`  
- [ ] 是 — `frame-ancestors` **已启用**，但允许特定的受信任第三方域  
- [ ] 否 — `frame-ancestors` **缺失**，依赖于过时的 `X-Frame-Options` 或无任何防护  

### 是否存在报告 CSP 违规的机制？
- [ ] 是 — `report-uri` 或 `report-to` **已配置**并处于活动状态  
- [ ] 否 — 违规报告**已禁用**或**未配置**  

---