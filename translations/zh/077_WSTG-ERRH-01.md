## WSTG-ERRH-01 — 不当错误处理测试 (Testing for Improper Error Handling)

当 Web 应用程序通过其错误响应泄露敏感技术细节（如堆栈跟踪 (Stack Traces)、数据库架构信息或内部文件路径）时，就会发生不当错误处理 (Improper Error Handling)。攻击者通过提交畸形输入、访问不存在的资源或诱导服务端异常来触发这些错误，从而映射应用程序的底层架构并识别潜在的进一步利用向量。这种信息泄露 (Information Leakage) 通常是更严重攻击的前奏，通过向攻击者提供精确的环境规范，辅助其进行 SQL 注入 (SQL Injection) 或路径遍历 (Path Traversal) 等攻击。安全的实现方式必须提供通用的用户友好消息，同时仅在安全的服务端日志中记录详细的诊断信息。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 低 / 中 (Low / Medium) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### 应用程序是否针对未处理的异常使用了通用的全局错误处理器？
- [ ] 是 — 通用错误页面已**启用**且**未泄露**技术细节  
- [ ] 是 — 使用了自定义错误页面，但**可能**通过特定 HTTP 头泄露信息  
- [ ] 否 — 默认服务器错误页面（例如 Tomcat, IIS, Nginx）**可见**  

### 是否可以通过畸形输入或边界情况枚举技术信息？
- [ ] 否 — 错误得到了优雅处理，并带有用于日志记录的唯一引用 ID  
- [ ] 是 — 响应中**泄露**了堆栈跟踪或数据库查询语句  
- [ ] 是 — 响应中**泄露**了内部文件路径、环境变量或服务器版本字符串  

### API 响应是否泄露了详细的错误对象或调试信息？
- [ ] 否 — API 返回标准化的错误代码和经过清理的 JSON 消息  
- [ ] 是 — API 在响应体中返回了详细的调试对象或完整的异常细节  
- [ ] 是 — JSON 响应的 `details` 或 `exception` 字段中包含了堆栈跟踪  

### 当错误发生时，应用程序的行为是否存在差异（基于时间或基于内容）？
- [ ] 否 — 无论错误类型如何，响应特征和时间消耗都是一致的  
- [ ] 是 — 可以通过差异化响应来枚举有效与无效的状态（例如：用户枚举 (User Enumeration)）  

---