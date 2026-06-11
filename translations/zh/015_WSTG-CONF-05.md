## WSTG-CONF-05 — 枚举基础设施和应用程序管理接口 (Enumerate Infrastructure and Application Admin Interfaces)

管理接口代表了应用程序及其底层基础设施的管理控制平面，通常授予对系统配置、用户数据和服务器端操作的特权访问。攻击者优先通过目录暴力破解 (Brute Force)、分析 `robots.txt` 或观察可预测的 URL 模式来枚举这些接口，以寻找可能缺乏强大身份验证或依赖默认凭据的入口点。发现暴露的管理面板会显著增加整个系统被攻陷、未经授权的数据修改或服务中断的风险。这些接口经常出现在非标准端口或子域名上，使其成为自动化扫描器和人工侦察 (Reconnaissance) 的主要目标。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果该接口允许在没有多因素身份验证 (MFA) 的情况下进行系统级配置更改、用户管理或数据外泄 (Data Exfiltration)，则严重程度将变为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### 是否可以通过目录模糊测试 (Fuzzing) 或侦察发现管理接口？
- [ ] 否 — 未暴露或无法发现任何管理接口  
- [ ] 是 — 接口可被发现，但访问**受限于**内部 IP 范围  
- [ ] 是 — 接口**可以**被发现且可公开访问  

### 发现的管理门户是否强制执行身份验证？
- [ ] 是 — **强制执行**多因素身份验证 (MFA)  
- [ ] 是 — **强制执行**单因素身份验证  
- [ ] 否 — 访问该接口不需要身份验证 *(紧急)*  

### 管理路径是否隐藏或采用非标准命名以防止被发现？
- [ ] 是 — 路径使用非标准、随机或不可预测的命名  
- [ ] 否 — 路径使用常见的命名约定（例如 `/admin`, `/manager`, `/console`），并且**可以**被轻易猜出  

### 该接口是否提供对敏感系统或应用程序功能的访问？
- [ ] 否 — 接口是“只读”状态仪表板，影响极小  
- [ ] 是 — 接口**可以**修改应用程序配置或用户数据  
- [ ] 是 — 接口**可以**执行系统级命令或进行数据库管理  

---