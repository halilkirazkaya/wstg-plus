## WSTG-INFO-07 — 映射应用程序的执行路径 (Map Execution Paths Through Application)

映射执行路径涉及系统性地识别 Web 应用程序中所有可访问的端点 (Endpoints)、功能流和决策分支。这一过程对于确保没有任何“隐蔽”功能——如遗留代码、调试接口 (Debug Interfaces) 或未记录的 API 路由——避开安全审查至关重要，因为这些功能通常缺乏现代安全控制。攻击者利用自动化爬行 (Spidering) 和手动探索相结合的方式来可视化应用程序的结构，在此过程中往往会发现诸如未经授权的管理访问或业务逻辑漏洞 (Business Logic Flaws) 等漏洞。通过将输入与特定的服务器端响应进行关联，测试人员可以精准定位需要进行针对性深入测试的敏感处理逻辑，以确保测试覆盖范围的稳健性。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 信息性 (Informational) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### 应用程序是否已通过自动化和手动爬行 (Crawling) 进行了完整的索引？
- [ ] 是 —— 所有可见和链接资源的完整映射**已完成**  
- [ ] 是 —— 自动化爬行**已完成**，但手动探索尚在进行中  
- [ ] 否 —— 应用程序**尚未**建立索引  

### 是否通过 JS 分析或目录暴力破解 (Brute-forcing) 识别出隐藏或未记录的端点？
- [ ] 否 —— 未通过侧信道分析发现未记录的路径  
- [ ] 是 —— 发现了隐藏路径，但未经授权**无法**访问  
- [ ] 是 —— 未记录的端点**可以**访问，并提供敏感功能 *(严重 / 高危)*  

### 执行路径是否可以通过非标准参数或请求头 (Headers) 进行更改？
- [ ] 否 —— 诸如 `debug`、`test` 或 `admin` 之类的参数**不**影响执行流  
- [ ] 是 —— 参数改变了输出，但**没有**绕过安全控制  
- [ ] 是 —— **应用了**路径更改参数，并允许绕过预期逻辑  

### 多步骤业务逻辑（例如多因素身份验证 (Multi-factor Auth)、结账）的流程是否已完整映射？
- [ ] 是 —— 多步骤过程中的所有状态和转换**均已识别**  
- [ ] 否 —— 复杂的有限状态机 (State Machine) 转换**无法**完整映射  

### API 文档文件 (Swagger/OpenAPI) 或客户端映射文件 (Source Maps) 是否暴露？
- [ ] 否 —— 没有公开暴露任何架构映射或文档文件  
- [ ] 是 —— 存在文档，但**受到**身份验证保护  
- [ ] 是 —— Swagger UI, OpenAPI JSON 或 JS Source Maps **已启用**且可公开访问  

---