## WSTG-INPV-11 — 代码注入 (Code Injection)

代码注入 (Code Injection) 发生在应用程序在其运行时环境中评估用户提供的代码片段时，通常是通过不安全的语言特定函数（如 `eval()`、`exec()` 或 `system()`）实现的。此漏洞与命令注入 (Command Injection) 不同，因为它的目标是编程语言的执行上下文（例如 PHP、Python、Node.js），而不是底层操作系统的 Shell。攻击者利用这些点执行任意逻辑、窃取环境变量或在服务器上实现完全的远程代码执行 (Remote Code Execution, RCE)。渗透测试人员应调查处理数学表达式、服务端模板或动态配置参数的功能，因为这些位置的输入可能会被解释为可执行代码。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **测试状态** | 未执行 |
| **严重程度** | 严重 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**工具：** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### 应用程序的任何功能是否通过语言特定的评估函数处理动态输入？
- [ ] 否 — 代码库中不存在动态评估函数  
- [ ] 是 — 使用了 `eval()`、`exec()` 或 `include()` 等函数，但不接受用户输入  
- [ ] 是 — 使用了这些函数且正在处理用户提供的输入  

### 在评估之前，是否对输入应用了输入验证或过滤 (Sanitization) 机制？
- [ ] 是 — 输入已针对硬编码白名单 (Hardcoded Whitelist) 进行了严格验证  
- [ ] 是 — 输入通过黑名单 (Blacklisting) 进行了过滤，但可能被绕过 (Bypass)  
- [ ] 否 — 输入被直接传递给评估函数，且未应用任何控制措施  

### 执行环境是否经过沙箱化 (Sandboxed) 处理或受限以防止系统级访问？
- [ ] 是 — 执行发生在一个高度受限的沙箱中，无法进行系统调用  
- [ ] 是 — 存在某些限制，但可能实现沙箱逃逸 (Sandbox Escape)  
- [ ] 否 — 环境允许完全访问底层语言 API 和操作系统 (OS) 函数 *(严重)*  

### 代码注入是否可被用于窃取敏感信息？
- [ ] 否 — 执行是盲注 (Blind) 形式，且无法进行带外通信 (Out-of-band Communication)  
- [ ] 是 — 可以通过 HTTP/DNS 请求窃取环境变量或本地文件  
- [ ] 是 — 执行代码的结果直接反射 (Reflected) 在应用程序的响应中  

### 应用程序是否允许远程文件包含 (Remote File Inclusion) 或动态库加载？
- [ ] 否 — 仅能执行本地预定义的代码路径  
- [ ] 是 — 可以强制应用程序从远程或攻击者控制的源加载并执行代码  

---