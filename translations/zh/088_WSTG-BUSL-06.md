## WSTG-BUSL-06 — 业务工作流绕过测试 (Testing for the Circumvention of Work Flows)

工作流绕过 (Workflow circumvention) 涉及篡改应用程序逻辑，以绕过多步流程中预定的序列或前提条件。攻击者会识别敏感的端点 (Endpoints)——例如支付确认、管理审批或多因素身份验证 (MFA) 完成页面——并尝试直接访问它们，从而跳过强制性的中间步骤。当服务端逻辑 (Server-side logic) 未能维持稳健的状态机 (State machine)，而是依赖于用户会遵循 UI 驱动路径的假设时，就会出现此漏洞。成功利用该漏洞可能导致严重的业务逻辑缺陷 (Business logic failures)，包括未经授权的交易、权限提升 (Privilege Escalation) 以及身份验证机制的绕过。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **测试状态** | 未执行 |
| **严重性** | 中 / 高* |

> *如果绕过行为允许未经授权的金融交易、权限提升或管理访问，则严重性变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**工具：** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### 应用程序是否包含多阶段业务流程？
- [ ] 否 — 应用程序仅由单次请求操作组成  
- [ ] 是 — 存在多阶段流程（例如：购物车、向导表单、注册）  

### 服务器是否严格强制执行步骤顺序？
- [ ] 是 — 服务端状态追踪确保步骤**无法**被跳过  
- [ ] 是 — 客户端控制暗示了某种顺序，但服务器**未**验证进度  
- [ ] 否 — 应用程序**未**强制执行任何特定的操作顺序  

### 攻击者能否通过直接请求终端 URL 或端点来到达流程的最终阶段？
- [ ] 否 — 在未完成先前步骤的情况下，**无法**直接访问终端步骤  
- [ ] 是 — 终端步骤（例如：`/api/v1/checkout/complete`）**可以**通过直接请求到达  

### 应用程序是否验证先前步骤的前提数据是否存在且有效？
- [ ] 是 — 服务器在继续操作前验证所有先前步骤的数据  
- [ ] 否 — 服务器在预期数据但未进行验证的情况下，**易受**“跳过”步骤攻击的影响  

### 是否可以通过操纵工作流来绕过多因素身份验证 (MFA) 或身份验证等安全控制？
- [ ] 否 — 关键控制**无法**通过流程操纵绕过  
- [ ] 是 — 通过跳转到验证后的步骤，**可能**绕过 MFA 或身份检查  

---