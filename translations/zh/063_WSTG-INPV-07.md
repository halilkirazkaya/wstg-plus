## WSTG-INPV-07 — XML 注入 (XML Injection)

XML 注入 (XML Injection) 发生在应用程序将用户提供的数据合并到 XML 文档或流中，且未对 XML 元字符 (Meta-characters) 进行适当的验证、净化或转义时。通过注入特定的 XML 标签或结构元素，攻击者可以操纵文档逻辑、绕过身份验证机制，或干扰后端处理的数据完整性。这种漏洞常见于基于 SOAP 的 Web 服务、接收 XML 的 REST API、SAML 断言 (SAML assertions) 以及动态生成 XML 配置文件或报告的应用程序。从攻击者的角度来看，其目标是脱离预定的数据上下文以改变文档结构，从而可能导致未经授权的权限提升 (Privilege escalation) 或执行非预期的业务逻辑 (Business logic)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 高 (High) / 中 (Medium) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**工具：** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### 应用程序是否接受并处理来自用户的 XML 格式输入？
- [ ] 否 — 应用程序**不**处理 XML 输入  
- [ ] 是 — 通过 API (SOAP/REST) 处理 XML 输入  
- [ ] 是 — 通过文件上传（SVG, DOCX 等）处理 XML  

### XML 元字符（例如：`<`, `>`, `&`, `'`, `"`）是否经过适当的转义或净化？
- [ ] 是 — 所有元字符都经过了**适当的**转义或净化 *(最安全)*  
- [ ] 是 — 应用了部分过滤，但**可以通过**编码绕过  
- [ ] 否 — 原始输入在**未经过**净化的情况下直接嵌入到 XML 结构中  

### 是否对所有 XML 输入强制执行服务端架构验证 (Schema validation - XSD/DTD)？
- [ ] 是 — **启用**了严格的架构验证，可防止结构性更改  
- [ ] 是 — **启用**了验证，但**不能**防止数据字段内的标签注入 (Tag Injection)  
- [ ] 否 — 架构验证已**禁用**或未实现  

### 攻击者能否注入新的 XML 标签以改变文档结构或逻辑？
- [ ] 否 — 无论输入如何，结构都保持不变  
- [ ] 是 — 可以注入标签，但**不能**改变业务逻辑  
- [ ] 是 — **可以**进行标签注入，并允许绕过逻辑或修改数据 *(高危)*  

### 是否可以使用 CDATA 部分 (CDATA sections) 或 XML 注释 (XML comments) 操纵 XML 解析器？
- [ ] 否 — 解析器能正确处理或拒绝 CDATA/注释标记  
- [ ] 是 — **可以**注入 `<![CDATA[...]]>` 或 `<!-- ... -->` 以隐藏输入或绕过过滤器  

---