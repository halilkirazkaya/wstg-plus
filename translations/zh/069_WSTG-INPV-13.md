## WSTG-INPV-13 — 格式化字符串注入 (Format String Injection)

格式化字符串注入 (Format String Injection) 发生在 Web 应用程序将用户控制的输入直接传递给变长参数函数（如 C 语言的 `printf` 系列或底层日志封装函数）的格式化字符串参数时。虽然在现代托管代码 (Managed Code) Web 框架中较少见，但在遗留的 CGI 应用程序、原生扩展或与底层系统库交互的特定边缘案例日志系统中，该漏洞依然非常关键。攻击者通过提供如 `%x` 或 `%p` 的格式说明符来泄露栈中的敏感内存地址和数据，或者使用 `%n` 说明符执行任意内存写入，从而利用此漏洞。成功的利用通常会导致通过远程代码执行 (Remote Code Execution, RCE) 实现完整的系统破坏，或者至少通过进程崩溃造成重大的拒绝服务 (Denial-of-Service, DoS) 状况。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 紧急 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### 应用程序是否通过原生代码或遗留 CGI 接口处理用户输入？
- [ ] 否 — 应用程序完全构建在托管代码框架（如 JVM、CLR）之上  
- [ ] 是 — 应用程序使用 JNI、原生 C/C++ 扩展或遗留二进制 CGI，这些地方可能存在格式化字符串漏洞  

### 用户提供的输入是否作为格式感知函数的主要参数传递？
- [ ] 否 — 输入作为数据参数传递，且格式化字符串是静态且硬编码的  
- [ ] 是 — 用户输入被直接拼接到格式化字符串参数中，但应用了净化 (Sanitization)  
- [ ] 是 — 用户输入被直接用作格式化字符串，且未应用任何净化  

### 是否可以使用格式说明符泄露内存内容？
- [ ] 否 — 输入已通过验证，且 `%p`、`%x` 或 `%s` 等说明符不被处理  
- [ ] 是 — 提供 `%p` 或 `%x` 说明符会导致栈或堆内存地址的回显  
- [ ] 是 — 敏感数据（如指针、栈饼图 (Stack Cookies)）可以通过格式说明符被窃取  

### 是否可以使用 `%n` 说明符使应用程序崩溃或对其进行操纵？
- [ ] 否 — 允许内存写入的说明符已被禁用或被编译器/环境拦截  
- [ ] 是 — 当提供 `%s%s%s%s` 或类似字符串时，应用程序崩溃 (DoS)  
- [ ] 是 — 通过 `%n` 进行任意内存写入是可能的，可能导致远程代码执行 (RCE)  

### 是否可以通过此注入绕过现代二进制保护机制（ASLR、DEP、栈饼图）？
- [ ] 否 — 环境已加固，且内存泄露无法用于绕过保护  
- [ ] 是 — 格式化字符串注入可用于泄露基地址，使得绕过地址空间布局随机化 (Address Space Layout Randomization, ASLR) / 数据执行保护 (Data Execution Prevention, DEP) 变得可能  

---