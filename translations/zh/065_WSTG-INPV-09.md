## WSTG-INPV-09 — XPath 注入 (XPath Injection) 测试

当应用程序使用用户提供的信息来构建针对 XML 数据的 XPath 查询，从而允许攻击者干扰查询逻辑时，就会发生 XPath 注入 (XPath Injection)。通过注入特定的语法，如单引号、括号和逻辑运算符，攻击者可以导航 XML 文档结构、绕过身份验证 (Authentication) 或外泄敏感数据节点。这种漏洞常见于 Web 服务、配置管理界面以及依赖 XML 数据存储而非传统 SQL 数据库 (SQL Databases) 的遗留系统。从攻击者的角度来看，这通常利用基于布尔 (Boolean-based) 或基于错误 (Error-based) 的技术来系统地映射 XML 树并从后端检索隐藏值。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**工具：** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### 用于构建 XPath 查询的用户输入是否经过适当的净化 (Sanitized) 或参数化 (Parameterized)？
- [ ] 是 — 输入**未**用于 XPath 查询，或已严格参数化  
- [ ] 是 — **已应用**强健的输入验证/编码，且**无法**绕过  
- [ ] 是 — **已应用**部分验证，但使用编码字符**可能**实现绕过  
- [ ] 否 — 用户输入被直接拼接进 XPath 查询 *(严重)*  

### 应用程序是否通过错误消息泄露 XML/XPath 结构细节？
- [ ] 否 — 显示通用错误页面，且**未**泄露 XML 细节  
- [ ] 是 — 错误消息揭示了 XML 处理的存在，但**未**泄露路径细节  
- [ ] 是 — 详细的错误消息揭示了 XPath 语法、节点名称或文件路径  

### 是否可以使用 XPath 逻辑绕过身份验证或授权 (Authorization) 逻辑？
- [ ] 否 — 身份验证逻辑**不**依赖于 XML/XPath，或已安全实现  
- [ ] 是 — 可以使用如 `' or 1=1 or '` 的基于逻辑的有效负载 (Payload) 绕过登录或权限检查  

### 是否可以通过盲注 (Blind Injection) 枚举 XML 文档结构或数据？
- [ ] 否 — 应用程序**不容易**受到基于布尔或基于时间 (Time-based) 的推断攻击  
- [ ] 是 — 节点名称和数据**可以**利用基于布尔的推断进行外泄 (Exfiltrate)  
- [ ] 是 — **可以**进行完整的 XML 树遍历和数据提取  

---