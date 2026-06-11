## WSTG-ATHN-11 — 多因素身份验证 (MFA) 测试

多因素身份验证 (Multi-Factor Authentication, MFA) 测试用于评估第二层安全屏障的稳固性，该屏障旨在即便在主要凭证泄露的情况下也能防止未经授权的访问。攻击者经常尝试通过识别未强制执行 MFA 的端点 (Endpoints) 来绕过它，例如旧版 API 版本、移动端后端或密码重置工作流。利用手段通常涉及篡改服务器响应、暴力破解 (Brute Force) 短效代码 (OTP)，或利用竞态条件 (Race Conditions) 和会话管理缺陷，使用户在未提供第二个因素的情况下转换为已认证状态。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 紧急 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### MFA 是否在所有身份验证门户中一致强制执行？
- [ ] 是 — 所有 Web、移动端和基于 API (API) 的登录尝试均要求 MFA  
- [ ] 是 — 但在特定端点未强制执行 MFA（例如：旧版 `/api/v1/login` 或特定移动端门户）  
- [ ] 否 — 应用程序中**未实现** MFA *(紧急)*  

### 是否可以通过直接 URL 导航或响应篡改绕过 MFA 验证步骤？
- [ ] 否 — 直接导航或参数篡改**无法**绕过挑战  
- [ ] 是 — **可以**在未完成 MFA 挑战的情况下直接导航到内部仪表板 URL  
- [ ] 是 — **可以**通过篡改服务器响应（例如：将 `401 Unauthorized` 更改为 `200 OK`）来获取访问权限  

### MFA 代码验证过程是否受到保护以防御暴力破解攻击？
- [ ] 是 — 在多次 OTP 尝试失败后，**应用了**严格的速率限制 (Rate Limiting) 或账户锁定  
- [ ] 是 — 存在速率限制，但**可以**通过 IP 轮换或请求头篡改绕过  
- [ ] 否 — 未强制执行速率限制，且**可以**对代码进行自动暴力破解 (Brute Force)  

### 应用程序在 MFA 转换期间是否维持安全的会话状态？
- [ ] 是 — 高权限会话令牌 (Session Tokens) **仅在**成功完成 MFA 后发放  
- [ ] 否 — 在完成 MFA **之前**即发放了功能齐全的会话 Cookie (Session Cookie)，允许访问某些功能  
- [ ] 否 — 应用程序使用静态标识符或可预测的会话转换，且**可以**被劫持  

### 备选 MFA 因素（短信、电子邮件、备份代码）是否存在可被利用的漏洞？
- [ ] 否 — 所有因素均使用安全、不可预测且有时限的值  
- [ ] 是 — 备份代码是可预测的或**可以**被枚举  
- [ ] 是 — 可以滥用“重新发送代码”功能执行短信/电子邮件轰炸，或泄露部分联系方式  

---