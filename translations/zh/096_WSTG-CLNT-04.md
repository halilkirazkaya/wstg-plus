## WSTG-CLNT-04 — 客户端 URL 重定向测试 (Testing for Client-Side URL Redirect)

客户端 URL 重定向 (Client-side URL Redirect) 发生在 Web 应用程序接受用户控制的输入（通常通过 URL 参数或 Hash 片段 (hash fragments)）并将其用于客户端重定向接收器 (Sink)，如 `window.location` 或 `document.location.href` 时。攻击者主要利用此漏洞进行复杂的网络钓鱼 (Phishing) 活动，因为受害者最初信任合法域名，随后被静默重定向到恶意网站。除了钓鱼攻击外，如果重定向发生时 URL 或 `Referer` 请求头中存在敏感信息，客户端重定向通常可以串联起来以窃取 OAuth 访问令牌 (OAuth access tokens) 或会话标识符 (Session identifiers) 等敏感数据。在现代单页面应用 (Single Page Applications - SPAs) 中，此漏洞经常出现在路由逻辑、身份验证后的 “return to” 参数或语言切换功能中。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **测试状态** | 未执行 |
| **严重程度** | 中危 (Medium)* |

> *如果重定向可被用于窃取 OAuth 令牌、会话 ID 或绕过身份验证流程中的安全控制，严重程度将变为高危 (High)。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**工具：** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### 应用程序是否使用客户端脚本根据用户输入处理重定向？
- [ ] 否 — 重定向完全由服务端通过 `3xx` HTTP 状态码处理  
- [ ] 是 — 应用程序使用 `window.location` 等 JavaScript 接收器 (Sinks) 根据 URL 参数进行跳转  
- [ ] 是 — 应用程序使用客户端框架路由（例如 React Router, Vue Router）处理用户提供的路径  

### 是否针对目标 URL 实施了输入验证或白名单 (Whitelist) 控制？
- [ ] 是 — 强制执行严格的允许域名/路径白名单 (Whitelist)，且无法绕过 (Bypass)  
- [ ] 是 — 存在验证但依赖于弱正则表达式 (Regex) 或黑名单 (Blacklists)，且可以绕过  
- [ ] 否 — 应用程序接受任何任意字符串作为重定向目标  

### 是否可以使用常见的混淆 (Obfuscation) 技术绕过重定向逻辑？
- [ ] 否 — 控制措施能正确处理编码字符、协议相对 URL (Protocol-relative URLs) (`//`) 和反斜杠  
- [ ] 是 — 可以通过 URL 编码 (URL encoding) 或双重编码绕过  
- [ ] 是 — 可以通过协议相对 URL 或 `@` 字符（例如 `https://expected.com@attacker.com`）绕过  
- [ ] 是 — 可以通过特定的浏览器特性 (Quirks) 或空字节注入 (Null byte injection) 绕过  

### 重定向过程是否会向目标站点暴露敏感数据？
- [ ] 否 — URL 中不存在敏感数据，重定向过程中也不通过请求头传输敏感数据  
- [ ] 是 — 敏感数据（例如会话令牌、API 密钥）通过 `Referer` 请求头泄露到外部站点  
- [ ] 是 — URL 片段 (`#`) 中的敏感数据可被目标站点的脚本访问  

### 是否可以通过重定向接收器执行 JavaScript (DOM-based XSS)？
- [ ] 否 — 接收器仅允许跳转到 `http` 或 `https` 协议  
- [ ] 是 — 重定向接收器允许 `javascript:` 或 `data:` URI，导致 DOM 型跨站脚本攻击 (DOM-based XSS) 成为可能  

---