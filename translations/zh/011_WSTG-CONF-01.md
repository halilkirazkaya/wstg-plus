## WSTG-CONF-01 — 测试网络基础设施配置

网络基础设施配置测试涉及识别支持 Web 应用程序的底层服务器和网络组件中的弱点。攻击者通常针对配置不当的服务、过时的协议和不必要的开放端口，以此获取立足点、进行横向移动 (Move Laterally) 或执行侦察 (Reconnaissance)。常见的发现项包括不安全的传输层安全协议 (TLS) 版本、冗余的服务横幅 (Service Banners) 以及本应限制在内部网络的暴露的管理接口 (Administrative Interfaces)。通过利用这些基础设施层面的缺陷，攻击者可以破坏传输数据的机密性或获得对主机操作系统的未经授权访问。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中 |

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**工具：** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### 是否有不必要的开放端口或服务暴露在应用主机上？
- [ ] 否 — 仅必要的端口（如 443）是**可访问的**  
- [ ] 是 — 暴露了额外的服务，但遵循了**安全**配置  
- [ ] 是 — 非必要的服务（如 FTP, Telnet, SMB）被公开**暴露**  

### 服务横幅或响应头是否泄露了敏感的版本信息？
- [ ] 否 — 横幅已被移除或显示为通用信息  
- [ ] 是 — 横幅泄露了服务器软件（如 Apache/nginx），但**未泄露**版本号  
- [ ] 是 — 冗余的横幅泄露了具体的软件版本，有助于**漏洞利用 (Exploit)**  

### TLS 配置是否遵循现代安全最佳实践？
- [ ] 是 — 仅**启用**了带有强密码套件的 TLS 1.2/1.3  
- [ ] 是 — **启用**了遗留协议（TLS 1.0/1.1）  
- [ ] 否 — **启用**了弱密码套件或不安全的协议（SSLv2/v3）  

### 管理或维护接口是否限制在授权网络内？
- [ ] 是 — 接口（如 SSH, RDP, 控制面板）从公共互联网**不可访问**  
- [ ] 否 — 接口可以公开**访问**，但需要多因素认证  
- [ ] 否 — 接口可以公开**访问**，且仅使用单因素认证或默认凭据  

---