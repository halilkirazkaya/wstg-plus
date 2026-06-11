## WSTG-CONF-02 — 测试应用程序平台配置

应用程序平台配置测试涉及对底层 Web 服务器 (Web Server)、应用程序服务器 (Application Server) 和框架设置进行审计，以确保其针对常见漏洞进行了加固 (Hardened)。攻击者会主动寻找默认凭据 (Default Credentials)、未修复的平台漏洞以及暴露的管理接口，以获取未经授权的访问或执行代码。配置错误通常会导致敏感环境变量、内部系统路径的泄露，或因存在不必要的示例应用程序而扩大攻击面 (Attack Surface)。从专业角度来看，配置不当的平台是实现目标环境初始访问 (Initial Access) 的高概率入口点。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果管理接口可以使用默认凭据访问，或者示例脚本允许远程代码执行 (Remote Code Execution, RCE)，严重程度将变为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### 平台上是否存在激活的默认凭据或账户？
- [ ] 否 —— 所有默认账户均已禁用 (Disabled) 或已更改密码  
- [ ] 是 —— 默认账户存在，但无法从外部网络访问  
- [ ] 是 —— 默认账户处于激活状态，且可以通过互联网访问 *(严重)*  

### 平台是否通过响应头或错误页面泄露敏感版本信息？
- [ ] 否 —— 版本字符串已隐藏或为通用形式  
- [ ] 是 —— HTTP 响应头（例如 `Server`, `X-Powered-By`）中泄露了详细的版本信息  
- [ ] 是 —— 详细的错误消息中暴露了完整的堆栈跟踪 (Stack Trace) 和内部系统路径  

### 管理或维护接口是否受到妥善限制？
- [ ] 否 —— 未暴露管理接口  
- [ ] 是 —— 接口存在，但受 IP 白名单 (Allowlisting) 或多因素身份验证 (Multi-Factor Authentication, MFA) 限制  
- [ ] 是 —— 接口（例如 `/admin`, `/manager`, `/console`）可公开访问，且仅使用单因素验证  

### 生产服务器上是否存在示例文件、脚本或文档？
- [ ] 否 —— 所有非必要文件均已移除  
- [ ] 是 —— 存在示例文件或文档，但不会泄露敏感信息  
- [ ] 是 —— 存在示例脚本（例如 `info.php`, `examples/`）并提供了直接的攻击向量 (Attack Vector)  

### 服务器配置是否支持不安全的 HTTP 方法？
- [ ] 否 —— 仅启用了 `GET` 和 `POST`  
- [ ] 是 —— 启用了潜在危险的方法，如 `PUT`、`DELETE` 或 `TRACE`，但受到限制  
- [ ] 是 —— 启用了危险的方法，允许未经授权的文件修改或凭据窃取  

---