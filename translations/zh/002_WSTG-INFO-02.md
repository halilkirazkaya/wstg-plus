## WSTG-INFO-02 — Web 服务器指纹识别 (Fingerprint Web Server)

Web 服务器指纹识别 (Web server fingerprinting) 是识别目标 Web 服务器的特定软件类型、版本以及底层操作系统的过程。攻击者进行此项侦察是为了识别针对特定版本 Apache、Nginx、IIS 或其他服务器技术的已知漏洞，例如未打补丁的常见漏洞与披露 (CVE) 或配置缺陷。这通常通过分析 HTTP 响应头（例如 `Server`、`X-Powered-By`）、检查独特的协议行为，或观察通过默认错误页面和文件泄露的信息来实现。成功的指纹识别允许攻击者量身定制其攻击策略，从常规扫描转向针对已知架构漏洞的高概率攻击。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重程度** | 信息性 / 低危* |

> *如果指纹识别揭示了存在已知高危漏洞的过时或停止维护 (EOL) 的服务器版本，严重程度将变为高危。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**工具：** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### HTTP 响应头中是否存在识别字符串？
- [ ] 否 — `Server` 和 `X-Powered-By` 响应头**不存在**或包含通用值  
- [ ] 是 — 响应头泄露了服务器软件，但**未泄露**具体版本号  
- [ ] 是 — 响应头泄露了特定服务器软件及**确切**版本号  

### 默认错误页面或系统生成的响应是否泄露服务器详细信息？
- [ ] 否 — 使用了自定义错误页面，且**未公开**服务器信息  
- [ ] 是 — 默认错误页面泄露了服务器软件名称和/或版本  
- [ ] 是 — 错误页面泄露了服务器详情、版本及**底层操作系统**  

### 是否通过默认文件或目录结构泄露服务器版本？
- [ ] 否 — 默认安装文件、手册和测试脚本已**移除**  
- [ ] 是 — 默认文件（例如 `info.php`、`manual/`、`test.html`）**存在**并泄露了版本数据  

### 是否能通过独特的行为分析准确推断出服务器类型？
- [ ] 否 — 服务器响应已标准化，**无法进行**基于行为的指纹识别  
- [ ] 是 — 独特的行为（例如响应头排序、特定错误代码或 TCP/IP 堆栈特征）**允许**进行服务器识别  

### 识别出的服务器版本目前是否受支持并已打补丁？
- [ ] 是 — 识别出的版本是当前版本，且受厂商**支持**  
- [ ] 否 — 识别出的版本已**过时**或**停止维护 (EOL)**，存在已知漏洞  

---