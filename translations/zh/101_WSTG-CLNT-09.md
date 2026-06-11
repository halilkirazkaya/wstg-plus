## WSTG-CLNT-09 — 点击劫持 (Clickjacking) 测试

点击劫持 (Clickjacking)，也称为用户界面重绘 (UI redressing)，发生在攻击者使用透明或不透明的层，诱导用户在打算点击顶层页面时，点击另一个页面上的按钮或链接。这种漏洞破坏了用户操作的完整性，可能导致代表受害者执行未经授权的数据修改、账户更改或资金转账。攻击者通常通过将目标网站嵌入到受控恶意站点的 `<iframe>` 中，并覆盖欺骗性内容或诱导性元素来利用此漏洞。它最常出现在执行敏感状态更改操作的页面上，例如“删除账户”按钮、密码重置表单或管理配置面板。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果可以通过点击劫持触发敏感操作（例如资金转账、账户删除或权限提升 (Privilege Escalation)），则严重程度变为“高”。

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**工具：** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### 应用程序是否实现了服务端响应头以防止未经授权的框架嵌套 (Framing)？
- [ ] 是 — `X-Frame-Options` 或 `Content-Security-Policy` **已应用**并能防止框架嵌套 *(最安全)*  
- [ ] 是 — 响应头存在但**配置错误**（例如 `X-Frame-Options: ALLOW-FROM` 缺乏广泛的浏览器支持）  
- [ ] 否 — 未应用任何框架保护响应头  

### 是否实现了 `Content-Security-Policy` (CSP) 的 `frame-ancestors` 指令？
- [ ] 是 — `frame-ancestors 'none'` 或 `'self'` **已应用**  
- [ ] 是 — `frame-ancestors` 存在但**允许**不受信任或过于宽泛的来源  
- [ ] 否 — 该指令**缺失**或未实现 CSP  

### `X-Frame-Options` (XFO) 响应头是否针对旧版浏览器支持进行了正确配置？
- [ ] 是 — `DENY` 或 `SAMEORIGIN` **已应用**  
- [ ] 是 — XFO 存在，但由于存在 CSP `frame-ancestors` 指令，现代浏览器会将其**忽略**  
- [ ] 否 — XFO 响应头**缺失**或设置为不安全的值  

### 框架保护是否可以通过已知技术绕过 (Bypass)？
- [ ] 否 — 无法通过标准技术（双重框架嵌套等）**绕过**  
- [ ] 是 — 由于 `frame-ancestors` 白名单中的正则表达式或逻辑薄弱，保护**可以**被绕过  
- [ ] 是 — 由于应用程序仅依赖基于 JavaScript 的框架破解 (Frame-busting)，保护**可以**被绕过  

### 敏感操作是否可以通过点击劫持概念验证 (Proof-of-Concept / PoC) 被利用？
- [ ] 否 — 框架嵌套被阻断或无法触及敏感操作  
- [ ] 是 — 用户界面重绘**是可能的**，但需要复杂的用户交互  
- [ ] 是 — 用户界面重绘**是可能的**，并且允许立即执行状态更改操作 *(紧急/Critical)*  

---