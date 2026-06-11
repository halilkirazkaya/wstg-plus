## WSTG-CLNT-03 — HTML 注入 (HTML Injection)

HTML 注入 (HTML Injection) 发生在应用程序在浏览器渲染用户提供的输入之前未能对其进行清理 (Sanitize) 时，允许攻击者将任意 HTML 标签注入到网页的文档对象模型 (DOM) 中。虽然与跨站脚本攻击 (XSS) 相似，但 HTML 注入的主要目标是操纵页面的视觉呈现，或通过注入恶意表单和欺骗性内容来促成网络钓鱼 (Phishing) 攻击。攻击者针对反映在用户界面 (UI) 中的参数（如搜索词、用户配置文件字段或错误消息），以诱骗受害者泄露敏感凭据 (Credentials) 或与未经授权的第三方链接进行交互。此漏洞对应用程序用户界面的完整性构成重大风险，并可能是更复杂的社会工程学 (Social Engineering) 或会话 (Session) 相关攻击的前奏。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### 用户控制的输入是否在没有进行适当 HTML 编码 (HTML encoding) 的情况下反映在 DOM 中？
- [ ] 否 — 所有反映的输入在渲染前都经过严格的 HTML 编码  
- [ ] 是 — 某些字符已编码，但可能绕过特定标签  
- [ ] 是 — 输入以原始形式反映，HTML 注入是可能的  

### 是否可以注入结构性 HTML 元素来改变页面布局？
- [ ] 否 — 应用程序过滤或剥离了如 `<div>`、`<table>` 或 `<iframe>` 等结构性标签  
- [ ] 是 — 可以注入结构性元素，但不会对 UI 产生重大影响  
- [ ] 是 — 通过注入的标签可能导致严重的 UI 篡改 (Defacement)  

### 是否可以注入功能性网络钓鱼元素，如表单或恶意链接？
- [ ] 否 — `<form>`、`<input>` 和 `<a>` 标签被有效阻止或清理  
- [ ] 是 — 可以注入恶意链接以将用户重定向到外部站点  
- [ ] 是 — 可以注入功能性 `<form>` 元素以获取凭据 (中)  

### 安全标头或内容安全策略 (CSP) 是否减轻了注入的影响？
- [ ] 是 — 已实施限制性 CSP，且应用了 `form-action` 或 `frame-src`  
- [ ] 否 — 存在安全标头，但 CSP 已禁用或包含 unsafe-inline/通配符  
- [ ] 否 — 未启用 CSP 或相关的安全标头  

---