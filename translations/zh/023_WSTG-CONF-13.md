## WSTG-CONF-13 — Path Confusion (路径混淆)

路径混淆 (Path Confusion) 源于不同的 Web 组件（如反向代理 (Reverse Proxy)、负载均衡器 (Load Balancer) 和后端应用服务器 (Backend Application Server)）在解析和解释 URL 路径 (URL Path) 时存在的差异。攻击者利用这些不一致性，通过注入特定字符（如分号、编码斜杠或点段 (Dot-segments)）来欺骗安全过滤器，并访问受限的端点或静态资源。这种漏洞可能导致关键的安全失败，包括身份验证绕过 (Authentication Bypass)、未授权数据访问以及 Web 缓存毒化 (Web Cache Poisoning)，因为前端可能会将安全规则应用于后端以不同方式解释的路径。成功的利用通常发生在技术栈中请求路由 (Request Routing) 和规范化 (Normalization) 逻辑发散的架构边界处。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果路径混淆导致身份验证绕过或敏感管理端点的访问控制失效，则严重程度为“高”。

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### 不同的架构层对路径分隔符（如 `;`、`#`、`?`）的解释是否一致？
- [ ] 是 — 所有层都以相同的方式规范化并解释路径分隔符  
- [ ] 否 — 存在不一致性，但**无法**用于访问受限资源  
- [ ] 否 — 差异允许攻击者绕过前端过滤器并到达后端逻辑，这是**可能**的  

### 是否可以使用路径遍历 (Path Traversal) 序列或编码字符绕过访问控制？
- [ ] 否 — 在安全检查之前始终应用规范化  
- [ ] 是 — 通过点段序列（例如 `/admin/..;/`）**可能**实现绕过  
- [ ] 是 — 通过 URL 编码字符（例如 `%2f`、`%2e%2e%2f`）**可能**实现绕过  

### 应用程序是否容易通过路径混淆受到 Web 缓存毒化 (Web Cache Poisoning) 的影响？
- [ ] 否 — 缓存逻辑不受路径歧义或额外路径信息的影响  
- [ ] 是 — 由于映射差异，恶意内容**可以**被缓存到合法路径中  
- [ ] 是 — 敏感信息**可以**通过 `RCD` (Relative Path Overwrite) 技术缓存到公共目录中  

### 服务器是否安全地处理“额外路径信息” (PathInfo)？
- [ ] 否 — 该功能已**禁用**或不存在  
- [ ] 是 — `PathInfo` 已处理且**不**会干扰安全过滤器  
- [ ] 否 — `PathInfo` 允许攻击者附加数据，从而混淆应用程序的路由逻辑  

---