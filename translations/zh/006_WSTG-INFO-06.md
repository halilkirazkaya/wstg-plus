## WSTG-INFO-06 — 识别应用程序入口点 (Identify Application Entry Points)

识别应用程序入口点涉及通过枚举 (Enumerating) 用户或外部系统与应用程序交互的所有可能方式来映射整个攻击面 (Attack Surface)。此过程包括发现所有 URL、API 端点 (API Endpoints)、参数 (Parameters)（GET、POST、基于 Cookie）、HTTP 标头 (HTTP Headers) 以及 WebSockets 或 gRPC 等专门协议。对于渗透测试人员 (Penetration Tester) 而言，这一阶段至关重要，因为漏洞经常出现在未记录的“影子”API (Shadow APIs)、遗留端点 (Legacy Endpoints) 或开发过程中被忽视的复杂参数结构中。彻底的枚举可确保应用程序的任何组件都不会成为未经测试的黑盒 (Black Box)，从而防止攻击者找到进入系统的未受监控路径。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **测试状态** | 未执行 |
| **严重程度** | 信息性 (Informational) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### 是否已在整个应用程序中枚举了所有的 GET 和 POST 参数？
- [ ] 是 — 已通过自动化和手动爬取完成**全面枚举**  
- [ ] 是 — 仅通过标准爬取识别了常见参数  
- [ ] 否 — 参数在很大程度上**未被识别**或未被测试  

### 是否通过模糊测试 (Fuzzing) 搜索了未记录或隐藏的参数（例如：debug、admin）？
- [ ] 否 — 对于此特定范围，发现隐藏参数**并非必要**  
- [ ] 是 — **已执行**参数模糊测试（例如使用 `Arjun`）且无发现  
- [ ] 是 — **已发现**隐藏参数并映射以进行进一步测试  
- [ ] 否 — **未执行**隐藏参数发现  

### 是否已针对每个端点识别了所有支持的 HTTP 方法 (HTTP Methods)（PUT、DELETE、PATCH 等）？
- [ ] 是 — 方法发现**已应用于**所有已发现的端点  
- [ ] 是 — 方法发现**仅应用于**高价值或敏感端点  
- [ ] 否 — 仅识别了标准的 GET 和 POST 方法  

### 是否映射了非标准入口点，如 WebSockets、gRPC 或自定义标头 (Custom Headers)？
- [ ] 否 — 应用程序**不使用**非标准协议或自定义标头  
- [ ] 是 — 所有协议特定的入口点和自定义标头**均已识别**  
- [ ] 否 — **存在**非标准入口点但尚未映射  

### 是否已全面映射 API 攻击面（包括带有版本号的端点，如 /v1/ 或 /v2/）？
- [ ] 否 — 应用程序**不开放** API  
- [ ] 是 — 所有 API 版本和端点**均已识别**并记录  
- [ ] 是 — 仅识别了当前的生产 API 版本  
- [ ] 否 — API 端点**仍未映射**或仅部分发现  

---