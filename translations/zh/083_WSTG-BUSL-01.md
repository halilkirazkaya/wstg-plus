## WSTG-BUSL-01 — 测试业务逻辑数据验证 (Test Business Logic Data Validation)

业务逻辑数据验证 (Business Logic Data Validation) 测试的重点是应用程序识别和拒绝在业务领域上下文中虽然语法正确但逻辑无效的数据的能力。攻击者通过提交非预期的值——如购物车中的负数数量、未来日期事件的过去日期或极大整数——来绕过约束并操纵应用程序的状态。这些漏洞通常存在于多步工作流 (Multi-step workflows)、结账流程和账户管理模块中，在这些模块中，服务端逻辑未能验证用户提交数据的上下文完整性。成功利用 (Exploit) 可能会导致经济损失、完整性受损以及对受限功能或数据的未经授权访问。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**工具：** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### 应用程序是否对数值输入实施了逻辑范围限制？
- [ ] 是 — 应用程序正确拒绝了不合适的负值、零或过大的数值 *(最安全)*  
- [ ] 是 — 应用程序拒绝了某些无效范围，但仍容易受到整数溢出 (Integer Overflow) 或边界绕过的影响  
- [ ] 否 — 应用程序接受任何数值，无论其业务上下文如何 *(危急)*  

### 服务端验证检查在应用程序的所有层中是否一致？
- [ ] 是 — 验证一致地应用于 API/服务层和数据库层  
- [ ] 是 — 验证已应用于 UI/前端，但可以通过直接 API 请求进行绕过  
- [ ] 否 — 未应用验证或验证不一致，导致逻辑数据损坏  

### 是否可以通过操纵多步工作流中的数据来破坏业务逻辑？
- [ ] 否 — 状态在服务器上安全维护，且前一步骤的数据不可被篡改  
- [ ] 是 — 验证发生在早期步骤中，但最终提交时未重新验证数据的完整性  
- [ ] 是 — 可以通过跳过步骤或提交乱序数据来实现工作流绕过  

### 应用程序是否妥善处理了绕过业务约束的“边缘情况 (Edge Case)”数据？
- [ ] 是 — 约束稳固，能够处理异常但语法有效的输入（例如：闰年、时区偏移）  
- [ ] 是 — 处理了某些边缘情况，但特定组合可能导致非预期行为  
- [ ] 否 — 边缘情况直接导致逻辑失败，例如免费购买或未经授权的状态变更  

---