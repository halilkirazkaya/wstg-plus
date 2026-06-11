## WSTG-ATHN-10 — 测试备选渠道中的弱身份验证 (Testing for Weaker Authentication in Alternative Channel)

测试备选渠道中的弱身份验证涉及识别和分析账户访问的辅助路径——例如移动 API (API)、密码恢复工作流、帮助台接口或遗留子域名——这些路径可能未执行与主 Web 门户相同的安全严谨性。这些备选渠道通常缺乏健壮的多因素身份验证 (Multi-factor authentication, MFA)、严格的速率限制 (Rate Limiting) 或复杂的密码要求，从而形成了一个破坏整个身份验证系统的“最薄弱环节”。攻击者专门针对这些被忽视的入口点进行撞库攻击 (Credential Stuffing)，或通过利用不同接口在处理身份验证时的差异来绕过 (Bypass) MFA。确保所有身份验证表面的一致性对于防止未经授权的访问并维护用户会话和数据的完整性至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### 是否存在并识别了备选身份验证渠道？
- [ ] 否 — 仅存在单一身份验证渠道  
- [ ] 是 — 识别出多个渠道（例如：移动应用 API、桌面客户端、遗留门户、SOAP 服务）  

### 备选渠道是否执行与主渠道等同的安全策略？
- [ ] 是 — 所有渠道均执行**完全相同**的密码复杂度和 MFA 要求  
- [ ] 是 — 渠道执行了密码复杂度要求，但 MFA **不是必需的**或**可以**被绕过  
- [ ] 否 — 备选渠道的身份验证要求**显著薄弱**  

### 速率限制和账户锁定在所有渠道中是否保持一致？
- [ ] 是 — 速率限制和锁定在所有端点上均**一致执行**  
- [ ] 是 — 存在控制措施，但在特定渠道（如移动 API）上**可能实现**绕过  
- [ ] 否 — 速率限制或锁定**未应用于**辅助身份验证路径  

### 是否可以通过切换到备选渠道绕过 MFA？
- [ ] 否 — MFA 是强制性的，无论入口点如何都**无法**被绕过  
- [ ] 是 — Web 门户要求 MFA，但移动或遗留 API **未强制执行**  

### 密码恢复或“忘记密码”流程是否比标准登录更薄弱？
- [ ] 否 — 恢复流程使用强力的带外验证 (Out-of-band verification) 且不泄露信息  
- [ ] 是 — 恢复流程使用弱安全问题，这些问题**可以**被轻易猜测或研究出来  
- [ ] 是 — 恢复流程易受枚举 (Enumeration) 攻击，或者**不需要**与标准登录相同级别的证明  

---