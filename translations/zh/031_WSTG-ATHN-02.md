## WSTG-ATHN-02 — 默认凭证测试 (Testing for Default Credentials)

默认凭证测试 (Testing for Default Credentials) 涉及识别仍在使用出厂设置或供应商提供的用户名和密码的管理接口、服务或应用程序组件。这种漏洞是攻击者的高优先级目标，因为它通常能以极小的代价直接导致系统完全被攻破、获取管理权限或实现远程代码执行 (Remote Code Execution, RCE)。它通常出现在商业现成软件 (Off-the-shelf software)、内容管理系统 (CMS) 平台、数据库管理工具以及物联网 (IoT) 设备或网络设备等集成硬件中。在初始侦察 (Reconnaissance) 和漏洞利用 (Exploitation) 阶段，攻击者通常通过将识别出的软件版本与公共默认密码数据库进行交叉对比，或使用自动化的凭证填充 (Credential Stuffing) 工具来进行利用。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **测试状态** | 未执行 |
| **严重程度** | 严重 / 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**工具：** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### 管理或维护接口是否暴露在互联网或不受信任的网络网段中？
- [ ] 否 —— 无法访问任何管理接口  
- [ ] 是 —— 存在接口，但受网络层控制（如 VPN、IP 白名单 (IP Allowlist)）限制  
- [ ] 是 —— 接口**已**公开访问且需要身份验证  

### 是否已针对所有识别出的组件更改了供应商提供的默认凭证？
- [ ] 是 —— 所有组件的默认凭证**均已更改**（最安全）  
- [ ] 是 —— 主应用程序的凭证**已更改**，但次要组件（如 CMS、数据库）仍保留默认设置  
- [ ] 否 —— 默认凭证在一个或多个可识别的接口上**处于活动状态**（严重）  

### 应用程序是否强制新账户或管理账户在首次登录时更改密码？
- [ ] 是 —— 在执行任何管理操作之前，更改密码是**强制性的**  
- [ ] 否 —— 应用程序**允许**无限期使用默认凭证  

### 是否激活了锁定机制或速率限制控制以防止自动化的默认凭证测试？
- [ ] 是 —— **已应用**严格的速率限制 (Rate Limiting) 或账户锁定 (Account Lockout)  
- [ ] 是 —— 存在控制措施，但通过标头篡改或 IP 轮转**可能实现**绕过  
- [ ] 否 —— **未启用**任何速率限制或锁定机制  

### 第三方插件、中间件或管理工具（如 phpMyAdmin, Tomcat Manager）是否使用唯一的凭证？
- [ ] 否 —— 环境中不存在第三方组件  
- [ ] 是 —— 所有第三方组件均使用唯一的非默认凭证  
- [ ] 否 —— 至少有一个第三方组件可通过已知的默认凭证进行**访问**  

---