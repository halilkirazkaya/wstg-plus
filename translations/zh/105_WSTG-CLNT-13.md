## WSTG-CLNT-13 — 跨站脚本包含 (Cross-Site Script Inclusion, XSSI) 测试

跨站脚本包含 (Cross-Site Script Inclusion, XSSI) 发生在 Web 应用程序在动态生成的 JavaScript 文件或 JSONP (JSON with Padding) 响应中导出敏感数据时，攻击者控制的站点随后可以通过 `<script>` 标签包含这些文件。由于脚本包含绕过了同源策略 (Same-Origin Policy, SOP)，攻击者可以在其自己的上下文中执行该脚本，并覆盖全局对象或变量以窃取 (Exfiltrate) 敏感信息。这种漏洞通常出现在返回用户特定数据（如电子邮件地址、API 密钥 (API Keys) 或会话元数据）的经过身份验证的端点中。从攻击者的角度来看，利用 (Exploit) 过程涉及构建一个引用易受攻击脚本的恶意页面，并捕获由此产生的全局变量更改或函数调用。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高 |

> *如果泄露的数据包含 CSRF 令牌、会话标识符或高度敏感的个人可识别信息 (PII)，严重程度将变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### 是否有任何身份验证端点返回包含敏感信息的动态 JavaScript 或 JSONP？
- [ ] 否 — 所有脚本都是静态的，**不能**包含用户特定数据  
- [ ] 是 — 存在动态脚本，但**不包含**敏感信息  
- [ ] 是 — 动态脚本包含 PII 或令牌，并且**可以**跨源访问  

### 敏感脚本响应是否实现了 `X-Content-Type-Options: nosniff` 标头？
- [ ] 是 — 标头已**应用**，可防止 MIME 类型嗅探 (MIME-type sniffing)  
- [ ] 否 — 标头**缺失**，可能允许将非脚本内容作为脚本执行  

### 敏感脚本是否受到抗 XSSI 机制（如不可执行前缀或自定义标头）的保护？
- [ ] 是 — 脚本需要无法通过标准 `<script>` 标签发送的自定义标头或令牌  
- [ ] 是 — 脚本使用无法轻易绕过的不可执行前缀（例如 `)]}'`）  
- [ ] 否 — 脚本可以通过使用环境凭据 (Ambient Credentials) 的 GET 请求直接访问 *(严重)*  

### 是否可以通过覆盖全局变量或原型 (Prototypes) 来窃取敏感数据？
- [ ] 否 — 敏感数据的作用域限制在局部，或通过现代浏览器功能进行保护  
- [ ] 是 — 敏感数据被分配给全局变量，并且**可以**被捕获  
- [ ] 是 — 数据通过**可以**被拦截的 JSONP 回调泄露  

---