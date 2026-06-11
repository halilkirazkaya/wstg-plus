## WSTG-CLNT-02 — JavaScript 执行测试

JavaScript 执行测试侧重于识别应用程序不当处理用户提供的数据，从而导致在受害者浏览器上下文中执行未经授权脚本的情况。这种漏洞通常会导致跨站脚本攻击 (XSS)，攻击者借此可以窃取会话 Cookie、操作文档对象模型 (DOM) 或代表用户执行操作。渗透测试人员会检查接收器 (Sinks)，例如 `innerHTML`、`document.write()` 和 `eval()`，在这些位置，来自源 (Sources)（如 URL 片段、`localStorage` 或 `window.name`）的未净化 (Sanitized) 数据可能会被处理。成功的漏洞利用 (Exploit) 会破坏用户会话的完整性和机密性，并可作为更复杂的客户端攻击的跳板。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**工具：** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### 应用程序是否在危险的 JavaScript 接收器 (Sinks) 中处理用户提供的数据？
- [ ] 否 — 所有数据均经过净化或在 `textContent` 等安全接收器中使用  
- [ ] 是 — 数据在危险接收器中处理，但**已应用**净化/编码  
- [ ] 是 — 数据在危险接收器中处理，且**未应用**净化  

### 是否存在内容安全策略 (CSP) 以缓解脚本执行？
- [ ] 是 — **已启用**严格的 CSP 且无法绕过 (Bypass)  
- [ ] 是 — **已启用** CSP，但可以通过 `unsafe-inline` 或不安全的 CDN 进行绕过  
- [ ] 否 — **未启用** CSP  

### 是否可以通过 URL 参数或片段执行 JavaScript（基于 DOM 的 XSS）？
- [ ] 否 — 输入已正确编码，**无法**执行  
- [ ] 是 — 输入已在 DOM 中反射，但现代框架的保护机制**阻止了**执行  
- [ ] 是 — 输入在浏览器中直接执行，且漏洞利用**是可能的**  

### 事件处理程序 (Event Handlers) 或 URI 方案是否容易受到脚本注入攻击？
- [ ] 否 — 输入已净化，删除了事件属性和 `javascript:` 方案  
- [ ] 是 — **可以**注入事件处理程序，但过滤器限制了负载 (Payload) 的复杂性  
- [ ] 是 — **可以**通过 `onerror`、`onload` 或 `href` 属性执行任意 JavaScript

---