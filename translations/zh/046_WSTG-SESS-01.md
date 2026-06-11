## WSTG-SESS-01 — 会话管理方案测试 (Testing for Session Management Schema)

会话管理方案 (Session Management Schema) 测试侧重于分析应用程序如何处理会话标识符 (Session Identifier) 的生命周期和结构，以确保它们能够抵御预测和劫持。攻击者通过捕获多个会话令牌 (Session Token) 进行统计分析，寻找模式、低熵 (Low Entropy) 或可预测的增量，从而允许他们为其他用户伪造有效的会话。该方案中的弱点通常表现为会话固定 (Session Fixation) 漏洞或使用随机性不足的值，这些值通常出现在 HTTP Cookie、URL 参数 (URL Parameters) 或自定义标头实现中。破坏会话方案是一种高影响力的攻击，它使攻击者无需获取受害者的主要凭据即可完全访问其已认证状态。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 高 (High) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**工具：** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### 会话标识符是否足够复杂且随机？
- [ ] 是 — 令牌通过了统计随机性测试（高熵）且**无法**绕过  
- [ ] 是 — 令牌很长，但包含可预测的模式或静态部分  
- [ ] 否 — 令牌是顺序生成的、基于可预测的时间戳，或者具有**极低**的熵 *(严重)*  

### 会话标识符在何处传输和存储？
- [ ] 标识符存储在带有 `Secure` 和 `HttpOnly` 标志的 Cookie 中 *(最安全)*  
- [ ] 标识符存储在 Cookie 中，但缺少 `Secure` 或 `HttpOnly` 标志  
- [ ] 标识符**仅**通过 URL 参数或 `GET` 请求传输 *(高风险)*  

### 应用程序在身份验证成功后是否签发新的会话标识符？
- [ ] 是 — 登录后立即**生成**一个新的、唯一的会话 ID  
- [ ] 否 — 登录前后会话 ID 保持不变 *(可能存在会话固定攻击)*  

### 会话标识符是否与特定的 IP 地址或 User-Agent 绑定以防止重用？
- [ ] 是 — 如果客户端指纹发生显著变化，会话将失效  
- [ ] 否 — 会话**可以**在不同的 IP 地址或设备上重用，而无需额外验证  

### 退出登录或超时后，会话标识符是否在服务端正确失效？
- [ ] 是 — 退出登录后，服务端会话状态被**完全销毁**  
- [ ] 否 — 即使客户端 Cookie 已删除，会话在服务器上仍然有效  
- [ ] 否 — 会话**永不过期**或超时期限过长  

---