## WSTG-SESS-04 — 测试暴露的会话变量 (Testing for Exposed Session Variables)

当敏感的会话标识符 (Session Identifiers) 或与状态相关的数据通过不安全的通道（如 URL 查询字符串 (URL Query Strings)、Referer 头部 (Referer Headers) 或系统日志）传输时，就会发生会话变量暴露。这种暴露显著增加了会话劫持 (Session Hijacking) 的风险，因为标识符可能会被缓存在浏览器历史记录中，被中间代理 (Intermediate Proxies) 记录，或者通过 Referer 头部泄露给第三方站点。渗透测试人员 (Pentesters) 通过监控所有出站请求并检查应用程序日志或服务器配置以查找疏忽导致的数据泄露来识别这些暴露。在现实场景中，攻击者通过从共享机器中收集有效的会话令牌 (Session Tokens) 或分析流量模式来利用这些泄露，从而绕过身份验证 (Authentication) 并获得对用户账户的未经授权访问。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果暴露的变量是允许立即接管账户的会话令牌，则严重程度变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**工具：** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### 会话标识符是否在 URL 查询字符串中传输？
- [ ] 否 — URL 查询字符串中**不存在**会话标识符  
- [ ] 是 — 存在标识符，但会话是短期的或低风险的  
- [ ] 是 — URL 查询字符串中**确实存在**唯一的会话 ID *(严重)*  

### 应用程序是否通过 Referer 头部泄露会话令牌？
- [ ] 否 — `Referrer-Policy` 防止了泄露，或者不存在第三方链接  
- [ ] 是 — 会话令牌**确实**通过 Referer 头部传输到了第三方域名  

### 会话变量是否存储在服务器端或应用程序日志中？
- [ ] 否 — 会话变量已被掩码处理或排除在日志之外  
- [ ] 是 — 会话变量**确实**以明文形式记录在应用程序或 Web 服务器日志中  

### 应用程序是否对更改状态的操作使用 GET 请求？
- [ ] 否 — 应用程序对敏感操作使用 `POST` 或其他非幂等方法  
- [ ] 是 — 使用了 `GET` 请求，导致会话数据被缓存在浏览器历史记录和中间代理中  

### 是否配置了 `Cache-Control` 头部以防止缓存会话相关数据？
- [ ] 是 — 敏感响应中**应用了** `Cache-Control: no-store`（或类似指令）  
- [ ] 是 — 已实施控制，但通过浏览器的极端情况**可能实现**绕过  
- [ ] 否 — 包含会话数据的敏感响应**可以**被浏览器缓存  

---