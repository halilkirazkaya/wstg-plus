## WSTG-CONF-08 — 测试 RIA 跨域策略

富互联网应用 (Rich Internet Application, RIA) 跨域策略涉及 `crossdomain.xml` 和 `clientaccesspolicy.xml` 等文件，这些文件定义了 Web 客户端（特别是 Adobe Flash 和 Microsoft Silverlight）如何处理跨源请求。这些文件对安全至关重要，因为它们明确地覆盖了同源策略 (Same-Origin Policy, SOP)，决定了哪些外部域被允许与应用程序交互并读取其响应。过度宽松的配置（例如在 `domain` 属性中使用通配符）允许攻击者在第三方站点上托管恶意 RIA 对象，从而窃取敏感数据或代表已认证用户执行操作。渗透测试 (Pentesting) 人员通常在 Web 根目录下定位这些文件，以评估授予第三方源的信任级别，并识别潜在的跨源数据泄露。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果应用程序处理可以通过宽松策略窃取的敏感用户数据或会话标识符，严重程度将变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Web 根目录下是否存在跨域策略文件（`crossdomain.xml` 或 `clientaccesspolicy.xml`）？
- [ ] 否 — 根目录下未找到文件  
- [ ] 是 — 存在 `crossdomain.xml` (Flash)  
- [ ] 是 — 存在 `clientaccesspolicy.xml` (Silverlight)  
- [ ] 是 — 两种 RIA 策略文件都存在  

### `allow-access-from` 域属性是否仅限于受信任的源？
- [ ] 否 — 文件存在但未包含 `allow-access-from` 条目  
- [ ] 是 — 明确白名单化了特定的受信任域，且无法绕过  
- [ ] 是 — 域已加入白名单，但包括不受信任的第三方或子域  
- [ ] 是 — 使用了通配符 `*`，允许来自任何源的访问 *(严重/Critical)*  

### 策略是否允许通过 HTTP 对 HTTPS 资源进行不安全通信？
- [ ] 否 — 设置了 `secure="true"`（或为默认值），防止 HTTP 源访问 HTTPS 数据  
- [ ] 是 — 明确设置了 `secure="false"`，允许中间人攻击 (Man-in-the-Middle, MITM) 攻击者窃取加密数据  

### 跨域配置中的 HTTP 请求头是否过度宽松？
- [ ] 否 — `allow-http-request-headers-from` 已受限或未启用  
- [ ] 是 — 允许受信任域使用特定请求头  
- [ ] 是 — 请求头使用了通配符 `*`，允许攻击者从任何域发送自定义请求头  

### `site-control`（主策略）配置是否具有限制性？
- [ ] 否 — `permitted-cross-domain-policies` 设置为 `none` 或 `master-only`  
- [ ] 是 — 策略允许服务器上的其他文件定义自己的规则 (`all`)  

---