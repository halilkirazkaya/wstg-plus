## WSTG-CLNT-06 — 客户端资源操纵测试

客户端资源操纵 (Client-side Resource Manipulation) 发生在应用程序允许用户控制的输入影响浏览器加载资源（如脚本、样式表或 iframe）的来源或路径时。通过操纵 URI 分段 (URI fragments)、查询参数 (query parameters) 或其他 DOM 源 (DOM sources)，攻击者可以强制应用程序加载恶意的外部内容或执行未授权代码。此类漏洞通常存在于 JavaScript 逻辑中，这些逻辑在没有针对可信白名单 (allow-list) 进行充分验证的情况下，动态填充 HTML 元素的 `src` 或 `href` 属性。从攻击者的角度来看，通过构建一个将资源加载重定向到攻击者控制的服务器的 URL，即可利用该漏洞，这可能导致基于 DOM 的跨站脚本攻击 (DOM-based Cross-Site Scripting, XSS)、敏感数据外泄或 UI 界面重绘 (UI redressing)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**工具：** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### 应用程序是否使用客户端接收器动态加载或嵌入资源？
- [ ] 否 — 资源仅通过静态 HTML 或服务端逻辑加载  
- [ ] 是 — 客户端 JavaScript 动态填充资源属性，如 `src`、`href` 或 `data`  

### 是否针对资源 URI 来源实施了验证控制或白名单？
- [ ] 是 — 所有动态资源**均应用了**严格的白名单和源验证 (origin validation)  
- [ ] 是 — 存在验证但依赖于弱黑名单或易被绕过的正则表达式 (regex)  
- [ ] 否 — 未对用户控制的资源路径应用任何验证控制  

### 是否可以使用替代协议或编码绕过 URI 验证？
- [ ] 否 — **无法**绕过资源过滤器  
- [ ] 是 — **可以**使用不同的协议绕过，如 `data:`、`vbscript:` 或 `javascript:`  
- [ ] 是 — **可以**使用编码字符或空字节 (null bytes) 绕过简单的字符串匹配  

### 攻击者能否将资源加载重定向到外部、不可信的域名？
- [ ] 否 — 资源路径被限制在同源 (same origin) 或固定的子目录内  
- [ ] 是 — 应用程序**可以**被强制从任意外部域名加载脚本或样式  

### 资源操纵的最大影响是什么？
- [ ] 低 — 操纵仅影响非可执行元素，如图像或非敏感文本  
- [ ] 中 — 操纵允许 CSS 注入 (CSS Injection) 或显著的 UI 界面重绘  
- [ ] 高 — 操纵导致 DOM-based XSS 或执行任意 JavaScript *(紧急)*  

---