## WSTG-INPV-17 — 主机头注入测试 (Testing for Host Header Injection)

主机头注入 (Host Header Injection) 发生在应用程序信任用户提供的 `Host` 标头并将其纳入服务器端逻辑（如链接生成或重定向）且缺乏充分验证时。攻击者通过操纵此标头来促进网络缓存投毒 (Web Cache Poisoning)，或通过将敏感令牌重定向到外部域来实现密码重置投毒 (Password Reset Poisoning)，或绕过依赖于标头路由的安全控制。成功利用该漏洞允许攻击者窃取会话数据、执行账户接管 (Account Takeover) 或将不知情的用户重定向到恶意基础设施。此漏洞最常见于负载均衡环境或根据传入请求上下文动态生成绝对 URL 的应用程序中。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **测试状态** | 未执行 |
| **严重性** | 中 / 高* |

> *如果主机头被用于在电子邮件中生成敏感链接（如密码重置），则严重性变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**工具：** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### 应用程序是否在链接、重定向或脚本中回显了 `Host` 标头的值？
- [ ] 否 — 应用程序使用硬编码的基础 URL 或经过严格验证的配置  
- [ ] 是 — `Host` 标头被回显，但针对白名单进行了严格验证  
- [ ] 是 — `Host` 标头被回显，且**可以**通过畸形值（例如 `expected.com:attacker.com`）进行绕过  
- [ ] 是 — `Host` 标头在**没有任何**验证的情况下被回显  

### 应用程序或基础设施是否处理替代的主机相关标头 (Alternative host-related headers)？
- [ ] 否 — 类似于 `X-Forwarded-Host`、`X-Host` 或 `Forwarded` 的标头被忽略  
- [ ] 是 — 处理了替代标头，但**不会**覆盖内部逻辑  
- [ ] 是 — 替代标头**可以**用于覆盖主要的 `Host` 标头值  

### 能否操纵 `Host` 标头来投毒系统生成的电子邮件（例如密码重置）？
- [ ] 否 — 电子邮件链接使用服务器配置中静态、可信的域名  
- [ ] 是 — `Host` 标头影响电子邮件链接，但验证**无法**被绕过  
- [ ] 是 — `Host` 标头**可以**被操纵，从而将密码重置令牌重定向到攻击者控制的域 *(严重)*  

### 应用程序是否容易受到通过 `Host` 标头进行的网络缓存投毒 (Web Cache Poisoning)？
- [ ] 否 — 不存在缓存机制，或者 `Host` 标头是缓存键 (Cache Key) 的一部分  
- [ ] 是 — 存在缓存，但它针对未包含在键中的输入 (Unkeyed inputs) 严格验证或忽略了 `Host` 标头  
- [ ] 是 — 存在缓存，且**可以**通过注入的 `Host` 标头进行投毒，从而向其他用户提供恶意内容  

### 服务器是否接受用于虚拟主机 (Virtual Host) 路由的任意 `Host` 标头？
- [ ] 否 — 服务器对无法识别的 `Host` 标头返回错误或默认页面  
- [ ] 是 — 服务器接受任何 `Host` 标头，这对于侦察 (Reconnaissance) 或内部服务枚举 (Internal service enumeration) 很有用  

---