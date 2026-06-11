## WSTG-CONF-14 — 测试其他 HTTP 安全响应头配置错误

测试 HTTP 安全响应头配置错误 (HTTP security header misconfigurations) 涉及验证防御性响应头的存在及其实施的正确性，这些响应头为常见的 Web 攻击向量提供了必要的保护。虽然这些响应头并不能修复底层代码漏洞，但它们的缺失会显著增加中间人攻击 (MITM)、MIME 类型嗅探 (MIME-sniffing) 利用以及通过引用来源 (Referrer) 导致的意外跨源数据泄露的风险。从攻击者的角度来看，缺失安全响应头是环境加固程度较低的高可见性指标，通常会辅助更复杂的攻击链，如会话劫持 (Session hijacking) 或信息外泄 (Information exfiltration)。这些配置通常在服务器层级进行评估，并应一致地应用于所有响应类型，包括 API 终端节点 (API endpoints) 和静态资源。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### 安全响应头存在性审计
| 响应头 | 已部署 | 缺失 | 建议配置 |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` 或 `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (特定功能相关，例如：`geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### 安全响应头在应用程序范围内的强制执行情况如何？
- [ ] 是 — 响应头通过中央负载均衡器 (Load balancer) 或反向代理 (Reverse proxy) 全局应用，且**无法**被覆盖  
- [ ] 是 — 响应头已应用于主文档响应，但在 API 响应或静态资源中**缺失**  
- [ ] 否 — 响应头应用不一致，或在整个应用程序域名中**缺失**  

### Strict-Transport-Security (HSTS) 响应头配置是否正确？
- [ ] 是 — `max-age` 设置为较长时间（如 2 年）且 `includeSubDomains` 已**启用**  
- [ ] 是 — `max-age` 已设置，但 `includeSubDomains` 已**禁用**，导致子域名存在风险  
- [ ] 否 — HSTS 响应头**不存在**或 `max-age` 设置为 0，允许协议降级攻击 (Protocol downgrade attacks)  

### Referrer-Policy 是否防止了敏感 URL 参数的泄露？
- [ ] 是 — 策略设置为 `no-referrer` 或 `same-origin` *(最安全)*  
- [ ] 是 — 策略设置为 `strict-origin-when-cross-origin`  
- [ ] 否 — 策略**未设置**或设置为 `unsafe-url`，可能导致令牌 (Tokens) 或敏感个人身份信息 (PII) 在引用来源头中泄露  

### X-Content-Type-Options 响应头是否防止了 MIME 类型嗅探？
- [ ] 是 — `nosniff` 指令**存在**并已正确实施  
- [ ] 否 — 响应头**缺失**，可能允许浏览器将非可执行文件（如图像）作为脚本执行  

---