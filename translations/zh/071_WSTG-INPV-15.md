## WSTG-INPV-15 — HTTP 响应拆分与请求走私 (HTTP Splitting Smuggling)

HTTP 响应拆分 (HTTP Splitting) 和 HTTP 请求走私 (HTTP Request Smuggling) 漏洞源于前端代理 (Frontend Proxies) 和后端服务器 (Backend Servers) 在解释和处理 HTTP 请求边界时存在差异，特别是涉及 `Content-Length` 和 `Transfer-Encoding` 头部时。通过构造具有歧义的请求，攻击者可以向后端“走私”一个隐藏的请求，或者注入 CRLF 序列 (CRLF sequences) 来拆分响应，从而导致缓存投毒 (Cache Poisoning)、请求劫持 (Request Hijacking) 或安全控制绕过 (Security Control Bypass)。这些缺陷通常出现在使用反向代理 (Reverse Proxies)、负载均衡器 (Load Balancers) 或内容分发网络 (CDNs) 且解析逻辑不一致的复杂环境中。从攻击者的角度来看，这允许重定向用户流量、窃取敏感会话令牌 (Session Tokens)，以及在其他用户会话的上下文中执行未授权操作。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 高 (High) / 严重 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**工具：** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### 环境是否容易受到由于 CL.TE 或 TE.CL 不一致导致的请求走私攻击？
- [ ] 否 — 前端和后端服务器处理请求边界的方式一致  
- [ ] 是 — 存在不一致，但由于基础设施缓解措施，无法利用  
- [ ] 是 — 可以进行 CL.TE 或 TE.CL 走私，允许隐藏请求到达后端 *(高)*  
- [ ] 是 — 可以进行 TE.TE（双重编码/混淆）以绕过前端过滤器 *(严重)*  

### 是否可以通过头部中的 CRLF 注入实现 HTTP 响应拆分？
- [ ] 否 — 应用程序正确清理了所有头部输入中的 CRLF 序列  
- [ ] 是 — 头部中反映了 CRLF 序列，但响应拆分不可行  
- [ ] 是 — 可以进行 CRLF 注入，允许头部注入或缓存投毒  

### 是否可以使用走私请求绕过安全控制 (WAF/ACLs)？
- [ ] 否 — 安全控制同时应用于外部请求和走私请求  
- [ ] 是 — 走私请求可以绕过前端 WAF 规则或基于 IP 的 ACL  

### 是否可能劫持其他用户的会话或重定向其流量？
- [ ] 否 — 请求/响应流是隔离的，无法交叉  
- [ ] 是 — 可以进行请求走私，并允许捕获其他用户的请求 *(严重)*  
- [ ] 是 — 可以进行响应拆分，并允许浏览器端缓存投毒或跨站脚本 (XSS)  

---