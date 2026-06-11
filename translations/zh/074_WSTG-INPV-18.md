## WSTG-INPV-18 — 服务器端模板注入 (Server-Side Template Injection, SSTI) 测试

服务器端模板注入 (Server-Side Template Injection, SSTI) 发生于应用程序将用户控制的输入嵌入到服务器端模板引擎中，而没有进行充分的清理 (Sanitization) 或转义时。这允许攻击者注入恶意的模板指令，这些指令随后在服务器上执行，可能通过远程代码执行 (Remote Code Execution, RCE) 导致系统完全沦陷。SSTI 通常出现在使用 Jinja2、Twig 或 FreeMarker 等引擎的动态电子邮件生成、个人资料页面和通知系统等功能中。从攻击者的角度来看，通常利用此漏洞泄露敏感环境变量、读取内部文件，或通过利用模板引擎的原生对象和方法来执行系统命令。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **测试状态** | 未执行 |
| **严重性** | 严重 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**工具：** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### 应用程序是否使用服务器端模板引擎处理用户输入？
- [ ] 否 — 应用程序不使用服务器端模板生成动态内容  
- [ ] 是 — 使用了模板，但用户输入未嵌入到模板指令中  
- [ ] 是 — 用户输入在未经适当清理的情况下嵌入到了模板中  

### 当注入输入字段时，基本算术表达式是否被执行？
- [ ] 否 — 类似 `{{7*7}}` 或 `${7*7}` 的有效载荷 (Payload) 被作为字面字符串反射  
- [ ] 是 — 表达式被执行（例如返回 `49`），确认模板引擎存在漏洞  

### 是否可以访问模板引擎的内置对象或方法？
- [ ] 否 — 对危险对象、方法或配置的访问受到限制或被沙箱 (Sandbox) 隔离  
- [ ] 是 — 可以访问内置对象（例如 `self`, `config`, `__class__`）以泄露敏感数据  
- [ ] 是 — 可能通过系统调用或操作系统库实现远程代码执行 (RCE)  

### 是否存在沙箱或白名单 (Allowlist) 机制以防止执行危险指令？
- [ ] 是 — 部署了严格的白名单或安全沙箱，且无法绕过 (Bypass)  
- [ ] 是 — 存在沙箱，但可以通过特定于引擎的有效载荷进行绕过  
- [ ] 否 — 模板引擎未应用任何沙箱或白名单  

---