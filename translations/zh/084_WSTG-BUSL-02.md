## WSTG-BUSL-02 — 测试伪造请求的能力 (Test Ability to Forge Requests)

在业务逻辑 (Business Logic) 上下文中伪造请求涉及操作应用程序定义的参数，以执行超出预期工作流或授权级别的操作。攻击者针对多步骤过程（如结账系统或账户注册），其中状态通过客户端参数维护，而服务器未能根据受信任的后端来源重新验证这些参数。通过更改隐藏字段、JSON 值或 REST 参数（如价格、数量或内部状态标志），攻击者可以绕过支付要求、提升权限 (Privilege Escalation) 或触发非预期的状态更改。这种漏洞通常出现在有状态的 Web 表单或 API 中，其中服务器在交易过程中隐式信任客户端对业务状态的表示。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重* |

> *如果伪造的请求导致经济损失、未经授权的管理操作或完全的账户接管 (Account Takeover)，则严重程度变为“严重”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### 应用程序是否根据服务器端的“单一事实来源”(Source of Truth) 验证交易参数？
- [ ] 是 — 所有敏感参数（价格、角色、ID）都根据数据库进行验证，且无法绕过  
- [ ] 是 — 应用了验证，但可以通过参数污染 (Parameter Pollution) 或备选编码绕过  
- [ ] 否 — 应用程序信任客户端值来确定交易成本或结果 *(严重)*  

### 业务关键值（如数量、金额）是否可以修改为未经授权的值？
- [ ] 否 — 负值、零值或过高的值会被服务器拒绝  
- [ ] 是 — 服务器接受修改后的值，但仅限于有限范围内  
- [ ] 是 — 可以提交负值或零值，导致经济损失或逻辑绕过  

### 是否使用隐藏字段或 API 参数在多步骤过程中维护状态？
- [ ] 否 — 状态通过服务器端会话 (Session) 或签名令牌 (Token) 安全地维护  
- [ ] 是 — 状态通过客户端传递，但完整性检查（HMAC/签名 (Signatures)）已启用且健全  
- [ ] 是 — 状态通过客户端传递，且完整性检查被禁用或可以被剥离  

### 是否可以伪造请求以跳过工作流中的强制性中间步骤？
- [ ] 否 — 服务器对每项交易执行严格的操作顺序  
- [ ] 是 — 步骤可以跳过，但最终操作仍需要来自先前步骤的有效数据  
- [ ] 是 — 可以直接伪造最终的“提交”或“完成”请求，以绕过授权 (Authorization) 或支付步骤  

---