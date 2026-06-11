## WSTG-CLNT-01 — DOM 型跨站脚本攻击测试

DOM 型跨站脚本攻击 (DOM-based Cross-Site Scripting, DOM XSS) 发生在应用程序包含以不安全方式处理来自不可信源数据的客户端 JavaScript 时，通常是通过危险的接收点 (Sink) 将数据写回文档对象模型 (Document Object Model, DOM)。与反射型 (Reflected) 或存储型 (Stored) XSS 不同，该漏洞完全存在于客户端代码中；由于数据由 `innerHTML`、`eval()` 或 `document.write()` 等接收点处理，有效负载 (Payload) 通常根本不会发送到服务器。攻击者通过构造包含恶意片段或参数的 URL 来利用此漏洞，当受害者的浏览器加载这些 URL 时，会在用户会话上下文中执行任意 JavaScript。这可能导致会话令牌 (Session Tokens) 外泄、敏感数据被窃以及代表已认证用户执行未经授权的操作。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**工具：** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### 在 JavaScript 中是否使用了不可信源来捕获用户控制的数据？
- [ ] 否 — JavaScript **没有**使用 `location.hash`、`location.search` 或 `document.referrer`  
- [ ] 是 — 使用了不可信源，但数据**没有**被传递到任何执行接收点 (Sink)  
- [ ] 是 — 使用了不可信源，且数据直接流向客户端逻辑  

### 用户控制的数据是否被传递到危险的 DOM 接收点 (Sink)？
- [ ] 否 — **没有**使用诸如 `innerHTML`、`outerHTML`、`document.write()` 或 `eval()` 之类的接收点  
- [ ] 是 — 存在危险接收点，但数据已使用诸如 `DOMPurify` 之类的安全库进行了严格的**净化 (Sanitized)**  
- [ ] 是 — 存在危险接收点，但数据处理仅使用了**不足**的或自定义的基于正则表达式的过滤  
- [ ] 是 — 存在危险接收点，且数据在**没有**任何净化或编码的情况下被传递 *(关键)*  

### 执行接收点是否可以通过基于 URL 的 Payload 触发？
- [ ] 否 — **无法**通过外部输入触发从源到接收点的流  
- [ ] 是 — 流是**可能**的，但需要复杂的用户交互  
- [ ] 是 — 流是**可能**的，且可以通过简单的 URL 片段或查询参数触发  

### 现代安全响应头或基于浏览器的缓解措施是否中和了风险？
- [ ] 是 — 已**启用**带有 `script-src` 和 `trusted-types` 的严格内容安全策略 (Content Security Policy, CSP)，可防止执行  
- [ ] 是 — 存在 CSP，但 `unsafe-inline` 或弱哈希使得绕过 (Bypass) 成为**可能**  
- [ ] 否 — 不存在用于缓解 DOM 型执行的 CSP  

### 应用程序是否使用了具有内置保护功能的客户端框架？
- [ ] 是 — 框架（如 Angular, React）会自动对数据进行编码，且**无法**绕过  
- [ ] 是 — 使用了框架，但**启用**了诸如 `dangerouslySetInnerHTML` 之类的“逃生舱” (Escape Hatches)  
- [ ] 否 — 应用程序使用原生 JavaScript (Vanilla JavaScript) 或不带自动转义功能的旧版库（例如旧版本的 jQuery）  

---