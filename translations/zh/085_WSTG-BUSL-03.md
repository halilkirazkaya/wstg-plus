## WSTG-BUSL-03 — 完整性检查测试 (Test Integrity Checks)

完整性检查测试 (Integrity Check Testing) 侧重于验证应用程序是否确保数据在流经各种业务流程或在客户端与服务器之间传输时，保持未经篡改且可信。攻击者通过篡改 HTTP 请求 (HTTP Requests)、隐藏字段或 cookies 中的关键参数（如产品价格、数量、用户权限或交易状态）来攻击此类逻辑。如果应用程序缺乏服务端校验 (Server-side verification) 或强健的完整性控制机制，如哈希 (Hashing) 或 HMAC (Hash-based Message Authentication Codes)，攻击者就可以操纵这些值以实施欺诈、提升自身权限 (Privilege Escalation) 或绕过预设的业务约束。对于任何处理金融交易、敏感状态转换或输出作为下一阶段可信输入的多步骤工作流应用程序，此项测试都至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果完整性失效导致资金窃取、未经授权的权限提升或持久记录的修改，严重程度将变为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### 敏感业务参数是否暴露于客户端篡改？
- [ ] 否 — 敏感值仅在服务端进行管理  
- [ ] 是 — 价格、数量或状态标识等值**已**暴露，但经过加密  
- [ ] 是 — 值以明文形式暴露在隐藏字段、URL 参数或 cookies 中  

### 客户端控制的参数是否应用了完整性机制（例如 HMAC、校验和）？
- [ ] 是 — **已应用**强健的完整性检查，并在每次请求时进行验证  
- [ ] 是 — **已应用**完整性检查，但使用了弱/可预测的算法或硬编码的密钥  
- [ ] 否 — 未对敏感参数**应用**完整性检查  

### 攻击者能否绕过或重新计算完整性检查？
- [ ] 否 — 签名验证被严格执行，绕过 (Bypass) **不可行**  
- [ ] 是 — 可以通过省略签名参数来绕过签名验证  
- [ ] 是 — 签名**可以**被重新计算，因为密钥或逻辑已泄露/强度不足  

### 应用程序是否针对可信源执行数据的后端重新验证？
- [ ] 是 — 服务端针对数据库重新验证所有值，绕过**不可行**  
- [ ] 是 — 服务端执行部分验证，但在特定边缘案例下绕过**可行**  
- [ ] 否 — 服务端直接信任客户端提供的数据，未进行后端验证 *（严重）*  

### 是否可以篡改多步骤流程的状态？
- [ ] 否 — 会话状态 (Session State) 在服务端管理，无法操纵  
- [ ] 是 — 状态信息通过客户端传递，**可以**通过篡改来跳过步骤或重复操作  

---