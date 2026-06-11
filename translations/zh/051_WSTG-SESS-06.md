## WSTG-SESS-06 — 注销功能测试 (Testing for Logout Functionality)

注销功能 (Logout functionality) 是一项关键的安全控制，旨在终止用户的会话 (Session) 并使客户端和服务器端关联的会话标识符失效。未能正确实现注销会导致会话固定 (Session Fixation) 或劫持攻击，因为在用户“注销”后访问该机器的攻击者可能会重复使用仍处于活动状态的会话令牌 (Session Token)。渗透测试人员 (Pentesters) 通过检查浏览器是否清除了会话 Cookie、服务器端会话状态是否被显式销毁，以及通过返回按钮导航是否允许访问缓存的敏感数据来评估这一点。一个安全的注销实现应确保一旦触发该操作，应用程序将不再授权任何使用旧会话令牌发起的后续请求。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **测试状态** | 未执行 |
| **严重程度** | 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**工具：** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### 是否存在且可访问的功能性注销机制？
- [ ] 是 — 注销按钮存在并触发终止请求  
- [ ] 否 — 注销按钮**缺失**或**未**触发终止事件  

### 会话标识符是否在服务器端失效？
- [ ] 是 — 服务器拒绝所有使用旧会话令牌的后续请求  
- [ ] 否 — 服务器在注销后**继续接受**旧会话令牌  

### 应用程序在注销时是否清除了浏览器中的会话 Cookie？
- [ ] 是 — Cookie 被过期日期或空值覆盖  
- [ ] 是 — Cookie 仍然存在，但在服务器上**不再有效**  
- [ ] 否 — Cookie 保留在浏览器中并**保留**其原始值  

### 注销后是否可以通过浏览器“返回”按钮访问经过身份验证的敏感内容？
- [ ] 否 — `Cache-Control: no-store` 或类似的标头防止在注销后查看敏感页面  
- [ ] 是 — 敏感页面被缓存，并且在会话终止后仍可以通导航**查看**  

---