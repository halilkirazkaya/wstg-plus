## WSTG-APIT-01 — API 侦察 (API Reconnaissance)

API 侦察 (API Reconnaissance) 是识别和映射 API 攻击面 (Attack Surface) 的系统化过程，旨在发现端点 (Endpoints)、支持的方法以及底层数据结构。攻击者执行此阶段是为了发现未公开的（影子）API (Shadow API)、带有遗留漏洞的弃用版本，以及暴露的 Swagger 或 OpenAPI 定义等文档文件，这些文件可能会泄露内部业务逻辑。通过分析可预测的 URL 模式、客户端 JavaScript 文件和目录暴力破解 (Brute Force) 结果，攻击者可以构建 API 的全面映射，从而识别失效的对象级授权 (BOLA) 或批量赋值 (Mass Assignment) 攻击的目标。此类侦察通常针对 `/api/v1/`、`/swagger.json` 或 `/graphql` 等常见路径，以获取应用程序后端功能的路线图。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **风险等级** | 信息级 / 低危 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**工具：** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### API 文档文件（Swagger, OpenAPI, WSDL）是否可以公开访问？
- [ ] 否 — API 文档不可访问或受到身份验证限制  
- [ ] 是 — 文档可以访问，但需要有效的凭据  
- [ ] 是 — API 文档（例如 `/swagger-ui.html`）在未经身份验证的情况下可以**公开访问**  

### 是否可以通过模糊测试 (Fuzzing) 发现未公开或“影子”API 端点？
- [ ] 否 — 目录和端点模糊测试未发现任何未公开路径  
- [ ] 是 — 发现了未公开端点，但未经授权**无法**访问  
- [ ] 是 — 发现了未公开端点，且在没有有效授权的情况下**可以**访问  

### 应用程序是否泄露了多个 API 版本（例如 /v1/, /v2/, /beta/）？
- [ ] 否 — 仅当前加固后的 API 版本可访问  
- [ ] 是 — 存在旧版本，但实施了与当前版本**相同**的安全控制  
- [ ] 是 — 旧版本（例如 `/v1/`）可以访问，且**缺乏**新版本的安全控制  

### 客户端资源（JavaScript/移动应用）是否泄露了 API 端点结构或密钥？
- [ ] 否 — 客户端代码**不**包含硬编码的 API 路径或敏感密钥  
- [ ] 是 — 客户端代码包含 API 端点映射，但**不**包含敏感密钥  
- [ ] 是 — API 密钥、密钥 (Secrets) 或仅限内部使用的端点 URL **被硬编码**在客户端资源中  

### 在已知端点上是否可以发现隐藏的参数或请求头 (Headers)？
- [ ] 否 — 参数模糊测试（例如通过 `Arjun`）**未**发现隐藏输入  
- [ ] 是 — 发现了隐藏参数（例如 `debug=true`, `admin=1`），但它们**无效**  
- [ ] 是 — 发现了隐藏参数，且**可以**用于改变应用程序行为  

---