## WSTG-INFO-04 — 枚举 Web 服务器上的应用程序 (Enumerate Applications on Webserver)

应用程序枚举 (Application Enumeration) 是指识别托管在单个 Web 服务器或 IP 地址上的所有 Web 应用程序和服务的过程。由于现代 Web 服务器通常托管多个虚拟主机 (Virtual Hosts)、遗留的暂存环境 (Staging Environments) 或非标准端口及路径上的管理界面，如果未能识别这些隐藏资产，将导致很大一部分攻击面 (Attack Surface) 未经测试。攻击者利用 DNS 区域传输 (DNS Zone Transfers)、虚拟主机暴力破解 (Virtual Host Brute-forcing) 和反向 IP 查询 (Reverse IP Lookups) 等技术来发现这些被遗忘或隐藏的入口点，这些入口点的安全性通常低于主要的生产环境应用程序。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重程度** | 信息级 / 低 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**工具：** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### 目标基础设施是否关联了多个 IP 地址或 DNS 别名 (DNS Aliases)？
- [ ] 否 — 仅存在单个 IP 和 A 记录 (A-record)  
- [ ] 是 — 已识别出多个 IP 地址或别名，扩大了审计范围  

### Web 服务器是否在同一个 IP 上托管了多个虚拟主机 (vhosts)？
- [ ] 否 — 服务器仅提供主域名的服务  
- [ ] 是 — 识别出额外的虚拟主机，但无法访问（例如：403 Forbidden）  
- [ ] 是 — 识别出隐藏的、开发或暂存环境的虚拟主机且可以访问  

### 应用程序是否托管在非标准端口上？
- [ ] 否 — 仅开放了标准端口 (80/443)  
- [ ] 是 — 非标准端口已开放，但并未托管 Web 应用程序  
- [ ] 是 — 在非标准端口（如 8080, 8443, 9000）上识别出额外的 Web 应用程序  

### 不同的应用程序是否映射到了特定的 URI 路径？
- [ ] 否 — 单个应用程序提供整个根目录的服务  
- [ ] 是 — 通过目录枚举发现映射了多个应用程序（例如：`/api`, `/portal`, `/backup`）  

### 第三方侦察 (Reconnaissance) 服务是否揭示了历史或次要应用程序？
- [ ] 否 — 在 `Shodan`、`Censys` 或 `Wayback Machine` 中无显著发现  
- [ ] 是 — 历史快照或服务扫描揭示了当前活跃但未链接的应用程序  

---