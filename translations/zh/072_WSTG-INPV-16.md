## WSTG-INPV-16 — HTTP 请求走私 (HTTP Request Smuggling)

HTTP 请求走私 (HTTP Request Smuggling, HRS) 发生在 HTTP 服务器链（如负载均衡器和后端 Web 服务器）对请求边界的解析不一致时，通常是由于冲突的 `Content-Length` 和 `Transfer-Encoding` 标头引起的。攻击者利用这种去同步 (Desynchronization) 现象将一个条目“走私”到后端服务器的请求缓冲区中，从而有效地在下一个合法用户的请求前添加一个由攻击者控制的分段。这种技术可以导致严重的攻击，包括会话劫持 (Session Hijacking)、绕过安全控制以及缓存投毒 (Cache Poisoning)。利用过程通常通过基于时间的分析 (Timing-based analysis) 或通过观察后续请求发送到服务器时后端返回的异常响应来进行。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **测试状态** | 未执行 |
| **严重程度** | 严重 (Critical) / 高 (High) |

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**工具：** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### 前端和后端服务器对 `Transfer-Encoding: chunked` 标头的处理是否一致？
- [ ] 是 — 两台服务器对分块编码的处理完全相同，去同步**不可行**  
- [ ] 否 — 服务器对标头的解析不同，但已应用清理/规范化 (Sanitization/Normalization)  
- [ ] 否 — CL.TE (Content-Length/Transfer-Encoding) 去同步**可行** *(严重)*  
- [ ] 否 — TE.CL (Transfer-Encoding/Content-Length) 去同步**可行** *(严重)*  

### 攻击者是否可以使用走私的请求对服务器或客户端缓存进行投毒？
- [ ] 否 — 缓存已**禁用**或不易受到投毒攻击  
- [ ] 是 — 走私的请求**可以**通过缓存投毒将用户重定向到恶意域名或注入脚本  

### 是否可以通过请求拼接捕获其他用户的请求或会话令牌 (Session Tokens)？
- [ ] 否 — 走私**不**会导致跨用户数据泄露  
- [ ] 是 — 走私允许将下一个用户的请求附加到攻击者控制的 `POST` 正文中，使数据外泄 (Exfiltration) **可行**  

### 基础设施是否使用 HTTP/2，并且是否容易受到 H2.CL 或 H2.TE 去同步的影响？
- [ ] 否 — 应用程序仅使用 HTTP/1.1，或者 HTTP/2 的实现是安全的且没有降级  
- [ ] 是 — 发生了 HTTP/2 到 HTTP/1.1 的明文降级 (Cleartext downgrades)，且去同步**可行**  

### 畸形标头 (Malformed headers)（例如：带空格的 `Transfer-Encoding:  chunked`）是否得到了安全处理？
- [ ] 是 — 畸形标头在所有层级均被拒绝或规范化  
- [ ] 否 — 由于对畸形或混淆标头 (Obscured headers) 的处理不一致，TE.TE 去同步**可行**  

---