## WSTG-CONF-07 — 测试 HTTP 严格传输安全 (HTTP Strict Transport Security, HSTS)

HTTP 严格传输安全 (HTTP Strict Transport Security, HSTS) 是一种安全机制，允许 Web 服务器通知浏览器仅应通过 HTTPS 访问该服务器，从而防止协议降级攻击 (Protocol Downgrade Attack) 和 Cookie 劫持 (Cookie Hijacking)。通过强制执行加密连接，HSTS 可以缓解中间人攻击 (Man-in-the-Middle, MitM)，例如 `SSLStrip`。在这种攻击中，攻击者拦截初始的不安全 HTTP 请求，以阻止向 TLS 的转换。从攻击者的角度来看，如果缺失该响应头或 `max-age` 持续时间过短，则会在初始明文握手 (Cleartext Handshake) 期间提供拦截敏感会话令牌 (Session Token) 或凭证 (Credentials) 的机会。本测试重点在于验证所有敏感应用程序端点 (Endpoint) 中 `Strict-Transport-Security` 响应头是否存在、持续时间以及作用范围。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中* |

> *如果应用程序处理个人身份信息 (Personally Identifiable Information, PII)、会话令牌或凭证，则严重程度变为“中”，因为缺少 HSTS 会增加中间人攻击 (MitM) 的成功率。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### HTTPS 响应中是否存在 `Strict-Transport-Security` 头？
- [ ] 是 — 所有敏感端点上**均存在**该响应头  
- [ ] 是 — **存在**该响应头，但仅限于起始页 (Landing Page)  
- [ ] 否 — 应用程序完全**缺失**该响应头  

### `max-age` 指令设置的持续时间是否足够？
- [ ] 是 — `max-age` 设置为一年或更久（例如：`31536000`）  
- [ ] 是 — 已设置 `max-age` 但**过短**（例如：少于 6 个月）  
- [ ] 否 — `max-age` 指令**缺失**或设置为 `0` *(高危)*  

### HSTS 策略是否覆盖所有子域名？
- [ ] 是 — **已应用** `includeSubDomains` 指令  
- [ ] 否 — **缺失** `includeSubDomains` 指令，导致子域名容易受到降级攻击  
- [ ] 否 — 该功能不适用，因为不存在子域名  

### 应用程序是否注册了 HSTS 预加载 (Preloading)？
- [ ] 是 — **存在** `preload` 指令且域名已在 HSTS 预加载列表中  
- [ ] 是 — **存在** `preload` 指令但域名**尚未**列入预加载列表  
- [ ] 否 — **缺失** `preload` 指令，允许在首次访问时存在中间人攻击 (MitM) 的窗口  

### HSTS 响应头是否错误地通过明文 HTTP 发送？
- [ ] 否 — 响应头**仅**通过安全的 HTTPS 连接发送  
- [ ] 是 — 响应头通过 HTTP **发送**，这会被浏览器忽略并可能泄露配置细节  

---