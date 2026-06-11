## WSTG-IDNT-01 — 测试角色定义 (Test Role Definitions)

测试角色定义涉及识别和记录应用程序中定义的各种用户角色和权限级别，以确保它们在逻辑上是分离的，并遵循最小权限原则 (Principle of Least Privilege)。攻击者通过分析应用程序来映射高权限角色与标准用户角色，寻找权限边界中的任何模糊之处或未记录的管理功能。识别这些角色是发现权限提升 (Privilege Escalation) 路径的第一步，因为角色定义中的不一致通常会导致对敏感数据或管理功能的未经授权访问。此阶段通常发生在初始侦查和业务逻辑映射期间，重点关注用户配置文件、管理仪表板和 API 权限结构。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 低 / 中 (Low / Medium) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### 应用程序及其文档中是否明确定义并记录了角色？
- [ ] 是 — 角色已完整记录并遵循 **最小权限原则 (Least Privilege)**  
- [ ] 是 — 角色已记录，但包含 **重叠权限**  
- [ ] 否 — 不存在正式的角色文档或定义  

### 管理功能与标准用户功能之间是否有明确的分离？
- [ ] 是 — 管理功能与标准用户视图 **完全隔离**  
- [ ] 是 — 存在分离，但管理端点 (Endpoints) 是 **可预测或可猜测的**  
- [ ] 否 — 管理功能和标准功能在同一界面中 **混合**  

### 应用程序是否使用集中化机制进行基于角色的访问控制 (RBAC)？
- [ ] 是 — 已 **实现** 并一致强制执行集中化的 RBAC 或基于属性的访问控制 (ABAC)  
- [ ] 是 — 存在分散的控制，但在各模块间 **应用不一致**  
- [ ] 否 — 访问控制逻辑被 **硬编码 (Hardcoded)** 在单个页面或组件中  

### 系统中是否存在未记录或隐藏的角色？
- [ ] 否 — 所有发现的角色均与记录的功能相符  
- [ ] 是 — **存在** 隐藏或“影子”角色（例如：`super_admin`、`support`、`test`）  

### 低权限用户是否可以枚举 (Enumerate) 其他用户的权限或角色？
- [ ] 否 — 用户 **无法** 查看或枚举其他实体的角色/权限  
- [ ] 是 — 用户 **可以** 通过个人资料页面、元数据或 API 响应枚举角色 *(中危)*  

---