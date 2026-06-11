## WSTG-INFO-05 — 审查网页内容以查找信息泄露 (Information Leakage)

审查网页内容涉及对应用程序响应（包括 HTML、JavaScript 和 CSS 文件）进行手动和自动化检查，以识别无意中向用户暴露的敏感信息。攻击者会仔细检查客户端源代码中的开发人员注释、隐藏表单字段、内部服务器路径、硬编码 (Hardcoded) 的 API 密钥以及有助于进一步漏洞利用阶段的调试信息。这种泄露通常发生在开发人员在移至生产环境之前忘记剥离元数据 (Metadata) 或调试代码时，可能会揭示逻辑缺陷或后端基础设施细节。从攻击者的角度来看，这提供了一种低成本的方法来映射应用程序的内部结构并发现被忽视的入口点，而不会触发安全警报或与服务器的后端逻辑进行交互。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中* |

> *如果发现敏感的硬编码凭证 (Credentials)、私钥或内部环境变量，严重程度将变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**工具：** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### 源代码中是否存在包含敏感信息的开发人员注释？
- [ ] 否 — 未发现注释，或仅发现通用的非敏感注释  
- [ ] 是 — 存在注释，但不包含敏感逻辑或数据  
- [ ] 是 — 注释揭示了内部路径、SQL 查询或管理指令  
- [ ] 是 — 注释泄露了凭证或敏感的开发人员笔记 *(高)*  

### 隐藏的 HTML 输入字段是否包含敏感数据或状态信息？
- [ ] 否 — 未发现隐藏字段，或仅包含非敏感令牌 (Token)  
- [ ] 是 — 隐藏字段用于状态管理，但已加密或经过签名  
- [ ] 是 — 隐藏字段包含明文密码、用户角色或内部 ID  

### JavaScript 文件中是否存在硬编码的密钥、API 密钥或内部 IP 地址？
- [ ] 否 — JS 文件中未发现敏感字符串或密钥  
- [ ] 是 — JS 文件包含具有适当限制的公共 API 密钥  
- [ ] 是 — JS 文件包含未经保护的 API 密钥、内部 IP 或私有端点 (Endpoint)  
- [ ] 是 — JS 文件包含硬编码的管理凭证或私钥 *(紧急)*  

### 应用程序是否在源码中泄露了框架版本或内部文件路径？
- [ ] 否 — 框架信息和文件路径已缩小化 (Minified) 或混淆 (Obfuscated)  
- [ ] 是 — 框架名称和版本可见，但已修复补丁  
- [ ] 是 — 源代码中暴露了内部文件路径或服务器绝对路径  

### 是否由于缺乏缩小化或混淆而导致源代码易于泄露？
- [ ] 否 — 生产代码已完全缩小化并剥离了注释  
- [ ] 是 — 代码已缩小化，但注释仍保留在源码中  
- [ ] 是 — 源码映射 (Source maps，即 `.map` 文件) 可公开访问，允许完全重构源代码  

---