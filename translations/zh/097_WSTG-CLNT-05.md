## WSTG-CLNT-05 — 测试 CSS 注入

当应用程序允许用户提供的输入在未经过适当清理或转义的情况下影响网页的层叠样式表 (CSS) 时，就会发生 CSS 注入 (CSS Injection)。虽然通常被认为只是外观问题，但攻击者可以利用 CSS 注入，通过使用属性选择器 (attribute selectors) 和背景图片 (background-image) 属性触发外部请求，从而窃取诸如 CSRF 令牌或会话 ID (session IDs) 之类的敏感数据。这种漏洞通常出现在用户偏好（例如：自定义主题、字体颜色）被反映在 `<style>` 块或内联 `style` 属性中的端点。从攻击者的角度来看，成功利用该漏洞可能导致界面劫持 (UI redressing)、通过未经授权的布局修改进行网络钓鱼 (phishing)，或者在严格的内容安全策略 (CSP) 可能会阻止基于 JavaScript 攻击的环境中进行隐蔽的数据外泄 (data exfiltration)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果注入点允许泄露 CSRF 令牌等敏感属性，或者可用于敏感工作流中的界面劫持，则严重程度变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### 应用程序是否在 CSS 上下文中反映用户控制的输入？
- [ ] 否 — 输入未在 style 标签、属性或 CSS 文件中反映  
- [ ] 是 — 输入已反映，但严格限制为安全的字母数字值  
- [ ] 是 — 输入在 `style` 属性或 `<style>` 块内反映  

### CSS 特定的元字符和函数是否经过了适当的清理？
- [ ] 是 — 诸如 `{`、`}`、`:` 的字符以及 `url()` 等函数已得到妥善转义  
- [ ] 是 — 已进行清理，但可能通过编码或 CSS 注释进行绕过 (bypass)  
- [ ] 否 — 未对 CSS 元字符应用任何清理  

### 是否可以使用 CSS 选择器进行数据外泄？
- [ ] 否 — 选择器无法触发外部请求或泄露数据  
- [ ] 是 — 可以通过属性选择器和 `background-image` 进行部分数据外泄  
- [ ] 是 — 可以使用自动化暴力破解 (brute-force) 技术实现敏感令牌的完全外泄  

### 内容安全策略 (CSP) 是否减轻了 CSS 注入的风险？
- [ ] 是 — `style-src` 仅限 'self'，且 `img-src` 或 `connect-src` 受到限制  
- [ ] 是 — 存在 CSP，但使用了 'unsafe-inline' 或允许通配符外部域  
- [ ] 否 — 未实施 CSP 来防止通过 CSS 加载外部资源  

### 该漏洞是否可用于执行界面劫持或网络钓鱼？
- [ ] 否 — 注入范围过于受限，无法显著修改布局  
- [ ] 是 — 可以修改 UI，允许覆盖恶意元素  
- [ ] 是 — 可以完全控制页面布局，从而实现极具欺骗性的网络钓鱼攻击  

---