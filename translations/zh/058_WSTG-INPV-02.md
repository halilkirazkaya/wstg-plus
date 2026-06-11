## WSTG-INPV-02 — 存储型跨站脚本攻击 (Stored Cross Site Scripting, XSS)

存储型跨站脚本攻击 (Stored Cross Site Scripting, XSS)，也称为持久型 XSS (Persistent XSS)，发生在应用程序接收来自用户的数据并将其存储在持久性数据库、文件系统或其他存储介质中，而没有进行充分的验证或编码时。当受害者随后导航到检索并显示这些未经处理的数据的页面时，恶意脚本将在其浏览器上下文中以应用程序的源 (Origin) 执行。这种漏洞特别危险，因为它不需要受害者点击专门构建的链接；任何查看受影响页面的用户都会成为目标，可能导致会话劫持 (Session Hijacking)、未经授权的操作或凭据收集 (Credential Harvesting)。攻击者通常针对评论区、用户个人资料、留言板和管理日志，在这些地方输入的内容会持续地呈现给其他用户或管理员。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**工具：** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### 是否存在存储用户输入以便稍后向其他用户显示的入口点？
- [ ] 否 — 应用程序不存储用户可控的输入用于后续渲染  
- [ ] 是 — 输入已存储（例如个人资料、评论），但**未**在 HTML 上下文中渲染  
- [ ] 是 — 输入已存储并渲染给其他用户或管理员  

### 在渲染存储的数据时，是否应用了输出编码或清理 (Sanitization)？
- [ ] 是 — **已应用**上下文感知的输出编码，且**无法**绕过 *(最安全)*  
- [ ] 是 — **已应用**清理（例如 `DOMPurify`），但由于配置错误**可以**绕过  
- [ ] 否 — 数据**未经**编码或清理直接渲染到 DOM 中 *(高危)*  

### 是否可以使用替代攻击载荷 (Payload) 绕过输入验证或清理？
- [ ] 否 — 过滤器安全地处理各种编码、非标准标签和事件处理器  
- [ ] 是 — 使用 HTML 实体编码或特定标签（例如 `<svg>`, `<img>`）**可以**实现绕过  
- [ ] 是 — 通过多语义载荷 (Polyglot Payloads) 或基于变异的 XSS (mXSS) **可以**实现绕过  

### 内容安全策略 (Content Security Policy, CSP) 是否提供了第二层防御？
- [ ] 是 — **启用**了严格的 CSP，并防止了内联脚本和未经授权来源的执行  
- [ ] 是 — **启用**了 CSP 但配置错误（例如 `unsafe-inline` 或宽泛的 `script-src`）  
- [ ] 否 — 应用程序**未启用** CSP  

### 是否可能实现“存储型 XSS 到 RCE”或其他高影响的链式攻击（盲 XSS (Blind XSS)）？
- [ ] 否 — 影响仅限于客户端浏览器上下文  
- [ ] 是 — 存储型 XSS **可以**用于针对内部面板中的管理员（盲 XSS）  
- [ ] 是 — 存储型 XSS **可以**与其他漏洞链接（例如 CSRF、敏感数据外泄）  

---