## WSTG-ERRH-02 — 堆栈追踪测试 (Testing for Stack Traces)

堆栈追踪 (Stack Traces) 是在应用程序未能妥善处理异常时生成的，从而导致向最终用户泄露内部执行状态和调用层次结构。对于渗透测试人员 (Penetration Tester) 而言，这些追踪信息极具价值，因为它们揭示了应用程序框架、库版本、内部文件系统路径和逻辑流，显著减少了侦察 (Reconnaissance) 所需的工作量。利用 (Exploitation) 过程包括通过畸形输入 (Malformed Input)、非预期的数值类型或违反边界条件来故意触发错误，强制应用程序进入不稳定状态并获取有关底层技术栈的信息。这种泄露通常为识别存在漏洞的第三方组件或针对披露的代码路径中发现的逻辑缺陷构建特定漏洞利用程序 (Exploit) 提供了必要的上下文。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中* |

> *如果堆栈追踪泄露了敏感的架构细节、数据库查询或内部配置参数，严重程度将提升至“中”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### 应用程序发生错误时，是否向客户端返回完整的堆栈追踪？
- [ ] 否 — 应用程序返回通用错误页面或自定义错误消息  
- [ ] 是 — 堆栈追踪已**启用**，但仅限于特定的非敏感模块  
- [ ] 是 — 完整堆栈追踪已**启用**，且在 HTTP 响应主体中可见  

### 错误消息是否泄露了敏感的环境信息？
- [ ] 否 — 错误消息不包含任何技术或环境细节  
- [ ] 是 — 消息泄露了**内部文件路径**或服务器端**目录结构**  
- [ ] 是 — 消息泄露了**数据库架构 (Database Schemas)**、**SQL 查询**或**第三方库版本**  

### 是否可以通过操纵输入参数来触发堆栈追踪？
- [ ] 否 — 畸形输入被妥善处理或被校验机制拒绝  
- [ ] 是 — 可以通过**非预期的数值类型**触发追踪（例如：在预期字符串的地方提交数组）  
- [ ] 是 — 追踪由**空字节 (Null Bytes)**、**特殊字符**或**边界值 (Boundary Values)** 触发  

### 全局错误处理程序 (Global Error Handler) 是否在整个应用程序中一致应用？
- [ ] 是 — 全局异常处理程序已在所有测试的端点中**一致应用**  
- [ ] 是 — 存在处理程序，但**未应用**于旧系统、API 或新增的端点  
- [ ] 否 — 未**探测**到全局错误处理机制，导致出现默认服务器错误  

---