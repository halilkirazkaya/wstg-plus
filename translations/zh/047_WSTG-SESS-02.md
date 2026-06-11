## WSTG-SESS-02 — Cookie 属性测试

会话管理 (Session Management) 通常依赖 HTTP Cookie 来保持状态，这使得这些 Cookie 的安全属性成为攻击者的主要目标。通过省略或错误配置诸如 `HttpOnly`、`Secure` 和 `SameSite` 等属性，应用程序会使会话令牌暴露于通过跨站脚本攻击 (Cross-Site Scripting, XSS) 被窃取、在未加密通道中被拦截或跨站请求伪造 (Cross-Site Request Forgery, CSRF) 攻击的风险中。渗透测试人员通过检查这些标志来确定攻击者是否可以从浏览器的文档对象模型 (Document Object Model, DOM) 中提取会话标识符，或强制浏览器在跨源请求中发送这些标识符。正确的属性配置是一项基础的深度防御 (Defense-in-Depth) 措施，旨在确保敏感令牌仅限于授权的、安全的上下文中。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **测试状态** | 未执行 |
| **严重程度** | 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**工具：** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Cookie 标志实施分析
| 属性 | 存在 | 缺失 |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### `HttpOnly` 标志是否应用于敏感会话 Cookie？
- [ ] 是 — 已应用于**所有**敏感会话 Cookie *（最安全）*  
- [ ] 是 — 仅应用于部分会话 Cookie  
- [ ] 否 — **未应用**该标志，允许通过客户端脚本进行访问 *（高危）*  

### 对于通过 HTTPS 传输的 Cookie，是否强制执行了 `Secure` 标志？
- [ ] 是 — 已应用于**所有**通过 TLS 传输的 Cookie  
- [ ] 否 — **未应用**该标志，允许 Cookie 通过明文 HTTP 发送  

### 是否使用 `SameSite` 属性来缓解 CSRF 风险？
- [ ] 是 — 为会话 Cookie 设置为 `Strict` 或 `Lax`  
- [ ] 是 — 设置为 `None` 但同时带有 `Secure` 标志  
- [ ] 否 — 属性**缺失**，或在**没有** `Secure` 的情况下设置为 `None`  

### `Domain` 和 `Path` 属性是否限制在必要的最小范围内？
- [ ] 是 — 作用域限定为**特定**子域名和应用程序路径  
- [ ] 否 — 作用域过**宽**（例如：设置为父域名或根目录 `/`），增加了攻击面  

### 敏感会话 Cookie 是否通过 `Expires` 或 `Max-Age` 属性正确过期？
- [ ] 是 — 设置了适当的过期日期  
- [ ] 否 — Cookie 是持久性的且生存周期过**长**  
- [ ] 否 — Cookie 仅限会话，但**缺乏**服务器端超时强制执行  

---