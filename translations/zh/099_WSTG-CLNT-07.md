## WSTG-CLNT-07 — 跨源资源共享 (Cross-Origin Resource Sharing, CORS)

跨源资源共享 (Cross-Origin Resource Sharing, CORS) 是一种基于浏览器的机制，允许服务器显式准许来自不同源的资源访问，从而有效地放宽了同源策略 (Same-Origin Policy, SOP)。当应用程序过度信任任意起源时，就会发生错误配置，这通常表现为在 `Access-Control-Allow-Origin` 响应标头中反射 `Origin` 标头，或使用实现不当的正则表达式 (Regex) 白名单。从攻击者的角度来看，可以通过在受控域名上托管恶意脚本来利用宽松的 CORS 策略，该脚本向受攻击的应用程序发起身份验证请求，从而允许攻击者直接从响应正文中提取敏感数据，如 CSRF 令牌 (Tokens) 或个人身份信息 (Personally Identifiable Information, PII)。此漏洞在处理已认证会话并返回非公开信息的 API 端点上最为关键。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**工具：** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### 应用程序是否在已认证端点上实现了 CORS 标头？
- [ ] 否 — **未启用** CORS；浏览器默认为严格的同源策略 (Same-Origin Policy)  
- [ ] 是 — 存在 CORS 标头，且仅限于严格的**白名单**  
- [ ] 是 — 存在 CORS 标头，但配置过于**宽松**  

### 服务器如何处理 `Access-Control-Allow-Origin` (ACAO) 标头？
- [ ] ACAO 设置为特定的、**受信任的**静态域名  
- [ ] ACAO 设置为 `*` (通配符) 且**不能**发送凭据 (Credentials)  
- [ ] ACAO 设置为 `*` (通配符) 且**可以**发送凭据 *(注：大多数浏览器会拦截此配置)*  
- [ ] ACAO **动态反射**来自请求的 `Origin` 标头值  

### `Access-Control-Allow-Credentials` (ACAC) 标头是否设置为 `true`？
- [ ] 否 — ACAC **不存在**或设置为 `false`  
- [ ] 是 — ACAC 设置为 `true`，但 ACAO **仅限于**受信任的源  
- [ ] 是 — ACAC 设置为 `true` 且 ACAO **反射**任意源 *(高危)*  

### 是否可以通过子域名或 null 源操纵绕过起源白名单？
- [ ] 否 — 白名单被严格执行且**无法**绕过  
- [ ] 是 — 白名单接受目标的**任何**子域名 (例如：`attacker.target.com`)  
- [ ] 是 — 白名单接受通过沙箱化 iframe 产生的 `null` 源  
- [ ] 是 — 白名单易受正则表达式绕过的影响 (例如：`target.com.attacker.com` 或 `target.com-safe.com`)  

### CORS 配置是否允许敏感数据泄露？
- [ ] 否 — 暴露的端点**不包含**敏感数据或特定于会话的数据  
- [ ] 是 — 已认证端点返回 PII、凭据或 CSRF 令牌，这些内容**可以**被未经授权的源读取  

---