## WSTG-CLNT-14 — 测试反向标签页钓鱼 (Testing for Reverse Tabnabbing)

反向标签页钓鱼 (Reverse Tabnabbing) 是一种客户端漏洞 (Client-side vulnerability)。当通过 `target="_blank"` 打开一个页面时，该页面通过 `window.opener` 对象保留对父页面的功能性引用。这允许攻击者控制的目标站点通过编程方式将原始的、受信任的标签页重定向到恶意 URL，通常是旨在窃取凭证的钓鱼页面 (Phishing page)。该攻击利用了用户对原始标签页的固有信任，因为用户不太可能注意到他们刚刚离开的页面已经导航到了不同的域名 (Domain)。现代安全实践要求在锚点标签 (Anchor tags) 上使用 `rel="noopener"` 或 `rel="noreferrer"` 属性，或者在 JavaScript 中将 `opener` 属性设置为 null，以防止这种跨窗口通信。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中* |

> *严重程度在大多数现代浏览器中为“低”，因为它们现在默认将 `target="_blank"` 处理为 `rel="noopener"`。如果应用程序针对旧版浏览器，或者在不将 `opener` 属性设置为 null 的情况下使用 `window.open()`，则严重程度变为“中”。

**参考资料:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### 是否存在在新窗口或标签页中打开的链接？
- [ ] 否 — 没有链接使用 `target="_blank"` 或等效的基于 JavaScript 的重定向  
- [ ] 是 — `target="_blank"` 仅用于内部、受信任的链接  
- [ ] 是 — `target="_blank"` 用于外部链接或用户提供的 URL  

### 是否对锚点标签限制了 `window.opener` 关系？
- [ ] 是 — `rel="noopener"` 或 `rel="noreferrer"` **始终应用于**所有带有 `target="_blank"` 的链接 *(最安全)*  
- [ ] 是 — 已应用属性，但在高风险或用户生成的链接中**缺失**  
- [ ] 否 — **未应用**属性，允许子页面访问 `window.opener`  

### 应用程序是否容易受到通过 JavaScript `window.open()` 触发的反向标签页钓鱼攻击？
- [ ] 否 — 未使用 `window.open()`，或者已显式将 `opener` 属性设置为 null  
- [ ] 是 — 使用了 `window.open()` 且子窗口的 `opener` 引用**仍可访问**  

### 攻击者能否控制在新标签页中打开的链接目标？
- [ ] 否 — 所有链接目标都是硬编码的，并指向受信任的域名  
- [ ] 是 — 链接目标由用户提供（例如：社交媒体个人资料、简介链接），且**未**针对标签页钓鱼防护进行清理 (Sanitize)  

---