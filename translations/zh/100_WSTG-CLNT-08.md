## WSTG-CLNT-08 — 跨站 Flash 攻击测试 (Testing for Cross Site Flashing)

跨站 Flash 攻击 (Cross-Site Flashing, XSF) 是一种客户端漏洞，当 Flash (SWF) 文件未能正确处理用户控制的输入时就会发生，允许攻击者执行恶意 ActionScript 代码或桥接到浏览器的 JavaScript 环境中。通过操纵传递给 `ExternalInterface.call`、`getURL` 或 `loadMovie` 等接收器 (Sinks) 的参数（如 `FlashVars` 或 URL 查询字符串），攻击者可以在受漏洞影响的域名上下文中执行操作，包括会话劫持 (Session Hijacking) 和数据泄露 (Data Exfiltration)。这种漏洞主要存在于仍托管 SWF 文件的遗留企业级应用程序中，由于输入验证不足，Flash 影片被重新利用作为跨站脚本攻击 (Cross-Site Scripting, XSS) 的矢量，或通过宽松的跨域配置绕过同源策略 (Same-Origin Policy, SOP)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### 应用程序中是否托管了遗留的 Adobe Flash (.swf) 文件？
- [ ] 否 — 应用程序范围内没有托管或引用 Flash 文件  
- [ ] 是 — 存在 SWF 文件，但仅提供静态内容  
- [ ] 是 — 存在 SWF 文件，并通过 `FlashVars` 或 URL 字符串接受动态参数  

### 传递给 ActionScript 接收器的输入是否经过了清洗？
- [ ] 是 — 所有输入都经过严格验证，ActionScript 接收器 (Sinks) **无法**被操纵  
- [ ] 是 — 已应用验证，但**有可能**通过特定的编码技术绕过  
- [ ] 否 — 输入直接传递到敏感接收器，如 `ExternalInterface.call` 或 `getURL` *(高危)*  

### Flash 文件是否可被用于执行任意 JavaScript (XSS)？
- [ ] 否 — `ExternalInterface` 已**禁用**，或 `allowScriptAccess` 参数被设置为 `never`  
- [ ] 是 — `allowScriptAccess` 设置为 `sameDomain`，但 SWF 托管在目标域名上  
- [ ] 是 — `allowScriptAccess` 设置为 `always`，允许来自任何域名的 XSS  

### `crossdomain.xml` 策略是否防止了未经授权的跨源请求？
- [ ] 是 — `crossdomain.xml` 具有严格限制，仅允许受信任的特定源 (Origins)  
- [ ] 否 — `crossdomain.xml` 文件**缺失**  
- [ ] 是 — 策略过于宽松（例如：`<allow-access-from domain="*" />`）  

### 是否可以强制 SWF 文件加载外部受攻击者控制的影片？
- [ ] 否 — **无法**强制应用程序加载外部 SWF 文件  
- [ ] 是 — `loadMovie` 或 `loadMovieNum` 函数接受未经验证的外部 URL  

---