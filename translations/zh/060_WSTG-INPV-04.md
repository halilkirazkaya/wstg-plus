## WSTG-INPV-04 — HTTP 参数污染 (HTTP Parameter Pollution, HPP) 测试

HTTP 参数污染 (HTTP Parameter Pollution, HPP) 发生在应用程序接收到多个同名的 HTTP 参数，并以不一致或不安全的方式处理它们时。通过提供重复的参数，攻击者可以操纵应用程序的内部逻辑，从而可能绕过 Web 应用防火墙 (Web Application Firewalls, WAF)、输入验证过滤器或访问控制机制。该漏洞高度依赖于底层的 Web 服务器或应用程序框架，这些框架可能会选择重复参数中的第一个、最后一个或拼接后的版本。利用 (Exploit) 行为通常针对将参数传递给后端应用程序接口 (Application Programming Interface, API)、数据库查询或 URL 重定向机制的端点，造成的影响从跨站脚本攻击 (Cross-Site Scripting, XSS) 到未经授权的数据泄露不等。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **测试状态** | 未执行 |
| **严重性** | 中 |

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### 应用程序如何处理同名的多个 HTTP 参数？
- [ ] 否 — 重复的参数被服务器拒绝 (Rejected) 或忽略
- [ ] 是 — 服务器一致地仅选择第一个或最后一个出现的参数
- [ ] 是 — 服务器对值进行级联 (Concatenates)，可能允许注入攻击

### HPP 是否可被利用来绕过 WAF 或输入过滤器等安全控制措施？
- [ ] 否 — 安全控制措施能够正确检查参数的所有出现
- [ ] 是 — WAF 或过滤器仅检查第一个出现的参数，允许恶意的第二个参数通过
- [ ] 是 — 级联允许攻击者将有效载荷 (Payload) 拆分到多个参数中以规避检测

### 应用程序是否在未进行适当清理的情况下在响应中反射受污染的参数？
- [ ] 否 — 参数在 UI 中反射之前已进行清理 (Sanitized) 或编码
- [ ] 是 — 污染导致内容被反射，但已应用清理措施
- [ ] 是 — 污染导致内容被反射且未应用清理措施，从而导致 XSS 或逻辑错误

### 传递给内部 API 调用或后端系统的参数是否容易受到污染攻击？
- [ ] 否 — 后端请求使用安全、非动态的构建方法
- [ ] 是 — HPP 允许攻击者在后端请求结构中注入或覆盖参数

### 当参数在 GET 与 POST 请求中受到污染时，应用程序是否表现出不同的行为？
- [ ] 否 — 所有 HTTP 方法的行为保持一致
- [ ] 是 — 仅 GET 参数容易受到污染
- [ ] 是 — 仅 POST (body) 参数容易受到污染

---