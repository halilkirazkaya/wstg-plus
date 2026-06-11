## WSTG-INPV-05 — SQL 注入 (SQL Injection)

SQL 注入 (SQL Injection) 发生在用户提供的输入在未经过适当参数化或过滤的情况下被纳入 SQL 查询中，从而允许攻击者操纵查询逻辑。成功利用此漏洞可能导致未经授权的数据泄露、身份验证绕过、数据修改，在某些情况下还可以通过 `xp_cmdshell` 或 `LOAD_FILE()` 等数据库功能执行远程代码执行 (Remote Code Execution)。该漏洞常见于登录表单、搜索功能、REST API 参数、HTTP 标头以及任何从用户控制的输入构建动态 SQL 查询的端点。从攻击者的角度来看，识别注入点 (Injection Point) 是全面攻破数据库以及在基础设施内进行潜在横向移动 (Lateral Movement) 的主要入口。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **测试状态** | 未执行 |
| **严重程度** | 严重 (Critical) |

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**工具：** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### 用户可控的参数是否使用安全的数据库访问方法进行处理？
- [ ] 是 — 应用程序完全使用对象关系映射 (ORM) 或参数化查询 (Parameterized Queries)，且**无法**绕过  
- [ ] 是 — 使用了参数化查询，但通过边缘情况（如 `ORDER BY` 子句）**可以**绕过  
- [ ] 否 — 在**没有**参数化的情况下，使用原始字符串拼接来构建 SQL 查询  

### 身份验证机制是否可以通过 SQL 注入被规避？
- [ ] 否 — 登录参数**不存在**注入漏洞  
- [ ] 是 — 使用基于恒真式 (Tautology) 的有效载荷 (Payload)（例如 `' OR 1=1 --`）**可以**绕过身份验证  

### 是否可以通过带内 (In-band) 或带外 (Out-of-band) 技术进行数据泄露？
- [ ] 否 — 未发现数据泄露  
- [ ] 是 — 基于联合查询 (UNION-based) 或基于错误 (Error-based) 的泄露**是可能的**  
- [ ] 是 — 盲注 (Blind)（基于布尔值或基于时间）泄露**是可能的**  
- [ ] 是 — 带外 (DNS/HTTP) 泄露**是可能的**  

### 应用程序功能是否存在二阶 SQL 注入 (Second-order SQL Injection) 漏洞？
- [ ] 否 — 存储的数据在检索用于后续查询时得到了安全处理  
- [ ] 是 — 用户输入被存储，随后在**未经**验证的情况下被用于不安全的 SQL 查询  

### 数据库服务账户权限是否被限制在所需的最低限度？
- [ ] 是 — 服务账户拥有**最小权限 (Least Privilege)**（例如，仅限于特定的表/操作）  
- [ ] 否 — 服务账户拥有高权限（例如，`DROP TABLE`、`FILE` 权限，或**启用了** `xp_cmdshell`）  

---