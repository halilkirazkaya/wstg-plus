## WSTG-INFO-01 — 开展搜索引擎发现与信息泄露侦察 (Conduct Search Engine Discovery and Reconnaissance for Information Leakage)

搜索引擎发现涉及利用公共搜索引擎、缓存页面和索引服务，来识别目标组织无意中泄露的信息。攻击者使用高级搜索运算符 (Google Dorks) 和第三方索引服务，来发现不应公开访问的敏感文件、目录、登录入口、错误消息和元数据 (Metadata)。这一侦察 (Reconnaissance) 阶段至关重要，因为它不需要与目标进行任何直接交互，使得该过程几乎无法被察觉，同时可能揭示高价值的入口点，如暴露的管理面板、配置文件、数据库转储 (Database Dumps) 和内部文档。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重程度** | 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**工具：** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### 敏感文件或目录是否已被搜索引擎索引？
- [ ] 否 — 搜索引擎结果中未发现敏感内容  
- [ ] 是 — 配置文件、备份或内部文档**已**被索引 *(严重)*  

### Google Dorking 是否泄露了暴露的管理或登录界面？
- [ ] 否 — 管理面板和登录页面**未**被索引  
- [ ] 是 — 管理面板**已**被索引，但需要强大的多因素身份验证 (Multi-factor Authentication)  
- [ ] 是 — 管理面板**已**被索引，且在**没有**足够控制措施的情况下可公开访问  

### 网页的缓存版本是否暴露了之后已被删除的敏感数据？
- [ ] 否 — 缓存快照中未发现敏感数据  
- [ ] 是 — 缓存页面包含凭据、内部 IP 地址或敏感信息  

### `robots.txt` 文件是否无意中泄露了敏感路径？
- [ ] 否 — `robots.txt` **未**泄露敏感目录结构  
- [ ] 是 — `robots.txt` 列出了有助于攻击者侦察的敏感路径  
- [ ] 不存在 `robots.txt` 文件  

### 第三方服务（Shodan, Censys, Wayback Machine）是否揭示了历史或当前的暴露情况？
- [ ] 否 — 第三方索引服务未发现重大发现  
- [ ] 是 — 历史快照或服务扫描揭示了敏感元数据或服务横幅 (Service Banners)

---

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

## WSTG-INFO-03 — 检查 Web 服务器元文件以防信息泄露 (Information Leakage)

Web 服务器元文件 (Metafiles)，如 `robots.txt`、`sitemap.xml` 和 `security.txt`，旨在引导爬虫 (Crawlers) 并提供管理元数据，但它们经常无意中泄露敏感目录结构或隐藏端点 (Endpoints)。攻击者分析这些文件以枚举应用程序的攻击面 (Attack Surface)，识别通常指向未链接的管理面板、备份目录或开发环境的 "Disallow" 指令。除了搜索引擎指令外，`.well-known/` 目录中的文件或 `humans.txt` 等文件可能会披露技术栈 (Technology Stacks)、开发人员信息或组织的安全性联系方式。对这些文件进行系统审查可以让测试人员在不执行激进的暴力破解 (Brute Force) 发现的情况下获得结构性和技术性的见解。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重程度** | 低 (Low) / 信息性 (Informational)* |

> *如果元文件泄露了未经身份验证的管理界面或敏感配置备份，严重程度将变为中 (Medium)。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### `robots.txt` 文件是否存在并暴露了敏感目录？
- [ ] 否 — `robots.txt` **不**存在  
- [ ] 是 — `robots.txt` 存在，但仅包含标准的公共路径  
- [ ] 是 — `robots.txt` 泄露了敏感目录路径或管理界面  
- [ ] 是 — `robots.txt` 在注释中泄露了凭证 (Credentials) 或特定的软件版本  

### 是否存在 `sitemap.xml` 文件并泄露了隐藏的应用程序结构？
- [ ] 否 — `sitemap.xml` **不**存在  
- [ ] 是 — `sitemap.xml` 存在，但仅列出了面向公众的 URL  
- [ ] 是 — `sitemap.xml` 列出了未链接的端点或**可以**访问的仅限内部使用的资源  

### 安全和组织的元文件是否暴露了技术细节？
- [ ] 否 — 在根目录或 `.well-known/` 中未发现元文件  
- [ ] 是 — 存在 `security.txt` 或 `humans.txt`，但包含的是标准信息  
- [ ] 是 — 元文件泄露了内部主机名、开发人员身份或技术栈细节，这些信息**可以**辅助社会工程学 (Social Engineering) 或进一步的攻击  

### 是否存在旧版本或特定于框架的元文件？
- [ ] 否 — 未发现特定于框架的文件（例如 `info.php`、`composer.json`、`package.json`）  
- [ ] 是 — 存在 `README.md`、`CHANGELOG.txt` 或 `.DS_Store` 等文件，但已进行脱敏处理 (Sanitized)  
- [ ] 是 — 存在特定于框架或版本的文件，并**允许**进行精确的技术指纹识别 (Fingerprinting)

---

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

## WSTG-INFO-05 — 审查网页内容以查找信息泄露 (Information Leakage)

审查网页内容涉及对应用程序响应（包括 HTML、JavaScript 和 CSS 文件）进行手动和自动化检查，以识别无意中向用户暴露的敏感信息。攻击者会仔细检查客户端源代码中的开发人员注释、隐藏表单字段、内部服务器路径、硬编码 (Hardcoded) 的 API 密钥以及有助于进一步漏洞利用阶段的调试信息。这种泄露通常发生在开发人员在移至生产环境之前忘记剥离元数据 (Metadata) 或调试代码时，可能会揭示逻辑缺陷或后端基础设施细节。从攻击者的角度来看，这提供了一种低成本的方法来映射应用程序的内部结构并发现被忽视的入口点，而不会触发安全警报或与服务器的后端逻辑进行交互。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中* |

> *如果发现敏感的硬编码凭证 (Credentials)、私钥或内部环境变量，严重程度将变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**工具：** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### 源代码中是否存在包含敏感信息的开发人员注释？
- [ ] 否 — 未发现注释，或仅发现通用的非敏感注释  
- [ ] 是 — 存在注释，但不包含敏感逻辑或数据  
- [ ] 是 — 注释揭示了内部路径、SQL 查询或管理指令  
- [ ] 是 — 注释泄露了凭证或敏感的开发人员笔记 *(高)*  

### 隐藏的 HTML 输入字段是否包含敏感数据或状态信息？
- [ ] 否 — 未发现隐藏字段，或仅包含非敏感令牌 (Token)  
- [ ] 是 — 隐藏字段用于状态管理，但已加密或经过签名  
- [ ] 是 — 隐藏字段包含明文密码、用户角色或内部 ID  

### JavaScript 文件中是否存在硬编码的密钥、API 密钥或内部 IP 地址？
- [ ] 否 — JS 文件中未发现敏感字符串或密钥  
- [ ] 是 — JS 文件包含具有适当限制的公共 API 密钥  
- [ ] 是 — JS 文件包含未经保护的 API 密钥、内部 IP 或私有端点 (Endpoint)  
- [ ] 是 — JS 文件包含硬编码的管理凭证或私钥 *(紧急)*  

### 应用程序是否在源码中泄露了框架版本或内部文件路径？
- [ ] 否 — 框架信息和文件路径已缩小化 (Minified) 或混淆 (Obfuscated)  
- [ ] 是 — 框架名称和版本可见，但已修复补丁  
- [ ] 是 — 源代码中暴露了内部文件路径或服务器绝对路径  

### 是否由于缺乏缩小化或混淆而导致源代码易于泄露？
- [ ] 否 — 生产代码已完全缩小化并剥离了注释  
- [ ] 是 — 代码已缩小化，但注释仍保留在源码中  
- [ ] 是 — 源码映射 (Source maps，即 `.map` 文件) 可公开访问，允许完全重构源代码

---

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

## WSTG-INFO-07 — 映射应用程序的执行路径 (Map Execution Paths Through Application)

映射执行路径涉及系统性地识别 Web 应用程序中所有可访问的端点 (Endpoints)、功能流和决策分支。这一过程对于确保没有任何“隐蔽”功能——如遗留代码、调试接口 (Debug Interfaces) 或未记录的 API 路由——避开安全审查至关重要，因为这些功能通常缺乏现代安全控制。攻击者利用自动化爬行 (Spidering) 和手动探索相结合的方式来可视化应用程序的结构，在此过程中往往会发现诸如未经授权的管理访问或业务逻辑漏洞 (Business Logic Flaws) 等漏洞。通过将输入与特定的服务器端响应进行关联，测试人员可以精准定位需要进行针对性深入测试的敏感处理逻辑，以确保测试覆盖范围的稳健性。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 信息性 (Informational) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### 应用程序是否已通过自动化和手动爬行 (Crawling) 进行了完整的索引？
- [ ] 是 —— 所有可见和链接资源的完整映射**已完成**  
- [ ] 是 —— 自动化爬行**已完成**，但手动探索尚在进行中  
- [ ] 否 —— 应用程序**尚未**建立索引  

### 是否通过 JS 分析或目录暴力破解 (Brute-forcing) 识别出隐藏或未记录的端点？
- [ ] 否 —— 未通过侧信道分析发现未记录的路径  
- [ ] 是 —— 发现了隐藏路径，但未经授权**无法**访问  
- [ ] 是 —— 未记录的端点**可以**访问，并提供敏感功能 *(严重 / 高危)*  

### 执行路径是否可以通过非标准参数或请求头 (Headers) 进行更改？
- [ ] 否 —— 诸如 `debug`、`test` 或 `admin` 之类的参数**不**影响执行流  
- [ ] 是 —— 参数改变了输出，但**没有**绕过安全控制  
- [ ] 是 —— **应用了**路径更改参数，并允许绕过预期逻辑  

### 多步骤业务逻辑（例如多因素身份验证 (Multi-factor Auth)、结账）的流程是否已完整映射？
- [ ] 是 —— 多步骤过程中的所有状态和转换**均已识别**  
- [ ] 否 —— 复杂的有限状态机 (State Machine) 转换**无法**完整映射  

### API 文档文件 (Swagger/OpenAPI) 或客户端映射文件 (Source Maps) 是否暴露？
- [ ] 否 —— 没有公开暴露任何架构映射或文档文件  
- [ ] 是 —— 存在文档，但**受到**身份验证保护  
- [ ] 是 —— Swagger UI, OpenAPI JSON 或 JS Source Maps **已启用**且可公开访问

---

## WSTG-INFO-08 — Web 应用程序框架指纹识别 (Fingerprint Web Application Framework)

Web 应用程序框架指纹识别 (Fingerprinting) 涉及识别用于构建和提供应用程序服务的特定技术栈 (Technology stack) 和软件版本。攻击者通过分析 HTTP 响应头 (HTTP response headers)、框架特定的 Cookie、可预测的文件结构以及唯一的 JavaScript 伪像 (Artifacts)，来判断目标是否运行在 React、Angular、Django 或 Spring 等平台上。这一侦察 (Reconnaissance) 阶段至关重要，因为它允许测试人员将攻击面 (Attack surface) 缩小到该框架特有的已知漏洞 (CVE) 和常见配置缺陷。通过了解底层架构，攻击者可以从通用测试转向针对性极强的利用 (Exploit) 策略。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重程度** | 信息 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### HTTP 响应头是否泄露了框架名称或版本？
- [ ] 否 — `X-Powered-By` 或 `Server` 等响应头**不存在**或已进行脱敏处理 (Sanitized)  
- [ ] 是 — 框架名称存在，但版本**已隐藏**  
- [ ] 是 — 框架名称和版本**均已泄露**  

### 框架特定的 Cookie 或目录结构是否可识别？
- [ ] 否 — 常见的框架伪像（如 `.js` 路径、Cookie 名称）已混淆或重命名  
- [ ] 是 — 框架特定的 Cookie（如 `JSESSIONID`, `PHPSESSID`, `csrftoken`）**可**识别  
- [ ] 是 — 目录结构（如 `/wp-content/`, `_next/static/`）**可见**并确认了框架  

### 错误消息或源代码注释是否泄露了框架细节？
- [ ] 否 — 错误处理是通用的，且注释已被移除  
- [ ] 是 — 框架特定的堆栈跟踪 (Stack traces) **已启用**且在响应中可见  
- [ ] 是 — HTML 源代码包含识别框架的注释或元数据标签 (Metadata tags)  

### 识别出的框架版本是否存在已知漏洞？
- [ ] 否 — 识别出的框架已是最新版本，且**无已知**公开漏洞  
- [ ] 是 — 框架版本过旧，**可以**识别出特定的 CVE

---

## WSTG-INFO-09 — 指纹识别 (Fingerprint) Web 应用程序

指纹识别 Web 应用程序是识别特定框架 (Framework)、内容管理系统 (CMS) 或底层技术栈 (Technology Stack) 及其相关版本的过程。这一侦察 (Reconnaissance) 阶段至关重要，因为它允许攻击者将识别出的组件与已知漏洞 (CVE) 和公开漏洞利用程序 (Exploit) 进行映射，从而显著缩小攻击面 (Attack Surface)。信息通常通过 HTTP 响应头、独特的文件路径、Cookie 名称、HTML 源代码元数据以及静态资源的加密哈希来收集。通过准确确定技术栈，攻击者可以从通用的自动化扫描转向针对特定版本弱点的高度针对性漏洞利用。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重性** | 低 / 信息性 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### HTTP 响应头是否泄露了技术栈或版本号？
- [ ] 否 — 敏感响应头已被移除或包含通用值  
- [ ] 是 — 存在诸如 `X-Powered-By`、`Server` 或 `X-AspNet-Version` 的默认响应头  
- [ ] 是 — 响应头泄露了 Web 服务器或应用框架的特定过时版本  

### 是否可以通过独特的文件路径或命名约定识别应用框架或 CMS？
- [ ] 否 — 文件路径已混淆，且不会揭示底层技术  
- [ ] 是 — 常见的目录结构（例如 `/wp-content/`、`/_next/`、`/node_modules/`）可见  
- [ ] 是 — 诸如 `robots.txt`、`humans.txt` 或 `sitemap.xml` 等特定文件包含技术特定的元数据  

### HTML 源代码或静态资源中是否暴露了特定的版本号？
- [ ] 否 — 在注释或资源参数中未发现版本信息  
- [ ] 是 — 存在 HTML 注释或 Meta 标签（例如 `<meta name="generator" content="...">`）  
- [ ] 是 — 版本字符串被附加到 CSS/JS 文件名（例如 `jquery.js?v=1.12.4`）且无法禁用  

### 默认错误页面或管理界面是否揭示了软件环境？
- [ ] 否 — 应用程序对所有状态码均使用自定义的通用错误页面  
- [ ] 是 — 启用了 Web 服务器或框架的默认错误页面，并泄露了版本数据  
- [ ] 是 — 后台管理登录门户（例如 `/wp-admin`、`/phpmyadmin`）可访问且可识别  

### Cookie 是否揭示了会话管理框架？
- [ ] 否 — 会话 Cookie 使用通用或自定义名称  
- [ ] 是 — 使用了技术特定的 Cookie 名称（例如 `PHPSESSID`、`JSESSIONID`、`ASPSESSIONID`）

---

## WSTG-INFO-10 — 映射应用程序架构 (Map Application Architecture)

映射应用程序架构涉及识别支持 Web 应用程序的底层技术、基础设施组件和数据流路径。通过分析 HTTP 响应头 (HTTP response headers)、错误消息、Cookie 格式和文件扩展名，测试人员可以重建 Web 服务器、应用程序服务器、数据库和所使用的第三方集成的图表。这些知识对于定制后续攻击至关重要，因为它允许选择特定于平台的漏洞利用程序 (Exploit)，并识别潜在的瓶颈或配置错误的中间设备，如负载均衡器 (Load Balancers) 和 Web 应用防火墙 (WAF)。攻击者利用此结构图从通用扫描转向针对特定软件栈 (Software stack) 及其互连组件的定向攻击。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重程度** | 信息性 / 低* |

> *如果泄露了内部网络拓扑或私有 IP 地址，严重程度将变为中。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### 是否可以准确识别 Web 服务器和应用程序服务器技术？
- [ ] 否 — 由于有效的加固 (Hardening) 或混淆 (Obfuscation)，无法进行识别  
- [ ] 是 — 技术类型已知，但无法确定具体版本  
- [ ] 是 — 技术和具体版本通过响应头或文件特征完全泄露 *(信息性)*  

### 在通信路径中是否可以检测到中间设备（WAF、负载均衡器、代理）？
- [ ] 否 — 未检测到中间设备，或它们是完全透明的  
- [ ] 是 — 通过响应时间或行为怀疑其存在，但身份**未**确认  
- [ ] 是 — 通过特定的响应头或行为识别出具体产品（例如 Cloudflare, Nginx, F5）  

### 应用程序是否泄露了有关其内部目录结构或网络拓扑的信息？
- [ ] 否 — 内部路径和 IP 地址**未**泄露  
- [ ] 是 — 内部路径在错误消息或堆栈追踪 (Stack traces) 中泄露，但内部 IP **未**泄露  
- [ ] 是 — 内部目录结构和私有 IP 地址均被泄露  

### 是否可以从应用程序行为推断出后端数据库的类型和版本？
- [ ] 否 — 无法确定数据库详情  
- [ ] 是 — 通过错误消息或行为推断出数据库类型（例如 PostgreSQL, MSSQL）  
- [ ] 是 — 数据库类型和版本在响应体或响应头中明确泄露  

### 应用程序是否托管在可识别的云服务提供商或特定的第三方内容分发网络 (CDN) 上？
- [ ] 否 — 托管基础设施无法识别  
- [ ] 是 — 识别出云提供商（AWS, Azure, GCP），但未识别出具体服务  
- [ ] 是 — 特定云服务（例如 S3 buckets, Lambda, Azure Functions）已被映射且可访问

---

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

## WSTG-CONF-03 — 测试文件扩展名处理以获取敏感信息 (File Extension Handling)

文件扩展名处理测试涉及识别 Web 服务器或应用服务器是否通过提供应受限制或应执行的文件来泄露敏感信息。攻击者经常探测可能留在 Web 根目录下的备份文件 (Backup Files)、配置文件 (Configuration Files) 和源代码片段 (Source Code Fragments)（例如 `.bak`, `.old`, `.env`, `.inc`）。由于缺乏特定的处理程序映射 (Handler Mappings)，这些文件可能会以纯文本形式提供。该漏洞非常关键，因为它可能导致数据库凭据 (Database Credentials)、API 密钥 (API Keys) 和内部业务逻辑的泄露，从而便于对基础设施进行更深层次的利用。这些泄露通常发生在公共目录、上传文件夹中，或者通过服务器上配置错误的 MIME 类型 (MIME-type) 设置发生。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **测试状态** | 未执行 |
| **严重性** | 中 / 高* |

> *如果包含凭据的配置文件或完整源代码可被访问，则严重性变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### 敏感的备份或临时文件扩展名（例如 `.bak`, `.old`, `.swp`, `~`）是否可访问？
- [ ] 否 — 服务器对常见的备份扩展名返回 403 Forbidden 或 404 Not Found  
- [ ] 是 — 服务器允许下载备份文件，但其中**不包含**敏感数据  
- [ ] 是 — 服务器允许下载包含敏感数据或源代码的备份文件 *(高)*  

### 服务器是否暴露了环境或系统配置文件（Environment or System Configuration Files）（例如 `.env`, `.config`, `.yml`, `.ini`）？
- [ ] 否 — 受限的配置文件**无法**访问并返回 403/404  
- [ ] 是 — 敏感配置文件**可以**访问并泄露了内部密钥或凭据  

### 是否可以通过附加替代扩展名（例如 `.php.txt`, `.jsp.old`, `.aspx.bak`）来检索源代码？
- [ ] 否 — 无论如何操纵扩展名，应用程序代码都**不会**被渲染为纯文本  
- [ ] 是 — 源代码被泄露，因为服务器**没有**针对附加扩展名的处理程序  

### 管理目录或元数据（Metadata）目录（例如 `.git/`, `.svn/`, `.DS_Store`）是否禁止公共访问？
- [ ] 否 — 这些目录/文件在 Web 根目录下**不**存在  
- [ ] 是 — 由于服务器端的重写规则 (Rewrite Rules) 或权限设置，**无法**访问  
- [ ] 是 — 元数据目录**可以**访问，并允许进行完整的源代码重构  

### 服务器如何处理具有多重扩展名 (Multiple Extensions)（例如 `file.php.jpg`）的文件请求？
- [ ] 否 — 服务器正确地优先考虑内部扩展名的安全性或拦截该请求  
- [ ] 是 — 服务器将文件作为第一个扩展名处理，但**无法**绕过执行  
- [ ] 是 — 服务器忽略了最终扩展名，**可能**允许代码执行或源码泄露

---

## WSTG-CONF-04 — 审查旧备份及未引用文件中的敏感信息 (Review Old Backup and Unreferenced Files for Sensitive Information)

审查旧备份及未引用文件涉及识别 Web 服务器上被遗忘的、临时的或隐藏的、且不打算公开访问的文件。这些文件通常包括源代码备份 (`.zip`, `.bak`)、文本编辑器交换文件 (`.swp`, `~`) 或源码控制元数据 (Source Control Metadata) (`.git`, `.svn`)，它们可能会泄露敏感信息。攻击者使用自动化字典 (Wordlists) 和发现工具进行暴力破解 (Brute Force) 常用命名规范，旨在窃取数据库凭据、硬编码的 API 密钥或有助于进一步利用 (Exploit) 的逻辑。这种漏洞通常源于手动服务器维护或未能清理生产环境 Web 根目录 (Web Root) 的缺陷 CI/CD 流水线 (CI/CD Pipelines)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果发现配置文件、源代码存档或凭据，严重程度将变为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Web 服务器是否启用了目录列表 (Directory Listings)？
- [ ] 否 — 目录列表已被禁用，并返回 403 Forbidden 或自定义错误  
- [ ] 是 — 在非敏感目录上启用了目录列表  
- [ ] 是 — 在包含源代码或敏感文件的目录上启用了目录列表 *(严重)*  

### 具有常用扩展名（如 .bak, .old, .save）的备份文件是否可被发现？
- [ ] 否 — 未发现常用的备份扩展名，或已被服务器配置拦截  
- [ ] 是 — 存在备份文件，但不包含敏感信息  
- [ ] 是 — 配置文件或源代码的备份文件可以被访问 *(高)*  

### 源码控制目录（如 .git, .svn）或元数据是否暴露？
- [ ] 否 — 源码控制元数据不存在或已被正确拦截  
- [ ] 是 — 存在元数据，但无法重构完整的代码仓库 (Repository)  
- [ ] 是 — 可以通过暴露的元数据窃取整个源代码仓库  

### Web 根目录下是否存在应用程序的压缩存档（如 .zip, .tar.gz, .rar）？
- [ ] 否 — 通过暴力破解或枚举未发现敏感的压缩文件  
- [ ] 是 — 发现了压缩存档，但它们受密码保护或仅包含公开资源  
- [ ] 是 — 包含应用程序源码或数据的未加密压缩存档可被公众访问  

### 应用程序是否泄露了由文本编辑器或 IDE 创建的临时文件？
- [ ] 否 — 不存在类似于 `.swp`, `~` 或 `.DS_Store` 的临时文件  
- [ ] 是 — 存在非敏感的临时文件  
- [ ] 是 — 临时文件泄露了源代码片段或内部路径

---

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

## WSTG-CONF-06 — 测试 HTTP 方法

测试 HTTP 方法 (HTTP Methods) 涉及识别 Web 服务器和应用程序支持哪些谓词 (Verbs)，以确保仅暴露必要的功能。除了标准的 `GET` 和 `POST` 之外，服务器可能会无意中启用危险的方法，如 `PUT`、`DELETE` 或 `TRACE`，这可能允许攻击者上传恶意文件、删除现有内容或通过跨站追踪 (Cross-Site Tracing (XST)) 窃取会话 Cookie (Session Cookies)。渗透测试人员 (Pentesters) 通过检查诸如 `Allow` 或 `Public` 之类的响应头，并尝试使用方法覆盖 (Method Overriding) 技术或谓词篡改 (Verb Tampering) 来绕过限制，从而访问未经授权的功能。此项测试对于巩固攻击面 (Attack Surface) 以及防止可能导致服务器沦陷或未经授权数据篡改的配置错误至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中* |

> *如果 `PUT` 或 `DELETE` 允许未经授权的文件操作，或者 `TRACE` 促成了会话令牌 (Session Token) 窃取，严重程度将变为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### 整个应用程序是否禁用了 `PUT` 或 `DELETE` 等危险的 HTTP 方法？
- [ ] 是 — 仅启用了安全的方法 (GET, POST, HEAD)  
- [ ] 否 — 启用了 `PUT` 或 `DELETE` 但需要有效的身份验证 (Authentication)  
- [ ] 否 — 启用了 `PUT` 或 `DELETE` 且无需身份验证即可访问 *(紧急/Critical)*  

### 是否禁用了 `TRACE` 方法以防止跨站追踪 (XST)？
- [ ] 是 — `TRACE` 方法已禁用或返回 405 Method Not Allowed  
- [ ] 否 — `TRACE` 方法已启用，但不会反射敏感响应头  
- [ ] 否 — `TRACE` 方法已启用，且会反射 `Cookie` 或 `Authorization` 响应头 *(中危)*  

### 服务器是否支持 HTTP 方法覆盖 (HTTP Method Overriding)（例如 `X-HTTP-Method-Override`）？
- [ ] 否 — 服务器不处理方法覆盖响应头  
- [ ] 是 — 服务器处理覆盖响应头，但强制执行访问控制 (Access Controls)  
- [ ] 是 — 服务器处理覆盖响应头，且可能绕过访问控制  

### 服务器对任意或畸形的 HTTP 方法响应是否安全？
- [ ] 是 — 服务器返回 405 Method Not Allowed 或 501 Not Implemented  
- [ ] 否 — 对于诸如 `JEFF` 或 `TEST` 之类的任意方法，服务器返回 200 OK 或 500 Error  
- [ ] 否 — 使用任意方法允许绕过身份验证或授权 (Authorization) 过滤器  

### `OPTIONS` 方法是否受到限制，或配置为最小化信息泄露？
- [ ] 是 — `OPTIONS` 已禁用或返回极简的 `Allow` 响应头  
- [ ] 否 — `OPTIONS` 泄露了广泛的已启用方法，包括 DAV 或调试谓词

---

## WSTG-CONF-07 — 测试 HTTP 严格传输安全 (HTTP Strict Transport Security, HSTS)

HTTP 严格传输安全 (HTTP Strict Transport Security, HSTS) 是一种安全机制，允许 Web 服务器通知浏览器仅应通过 HTTPS 访问该服务器，从而防止协议降级攻击 (Protocol Downgrade Attack) 和 Cookie 劫持 (Cookie Hijacking)。通过强制执行加密连接，HSTS 可以缓解中间人攻击 (Man-in-the-Middle, MitM)，例如 `SSLStrip`。在这种攻击中，攻击者拦截初始的不安全 HTTP 请求，以阻止向 TLS 的转换。从攻击者的角度来看，如果缺失该响应头或 `max-age` 持续时间过短，则会在初始明文握手 (Cleartext Handshake) 期间提供拦截敏感会话令牌 (Session Token) 或凭证 (Credentials) 的机会。本测试重点在于验证所有敏感应用程序端点 (Endpoint) 中 `Strict-Transport-Security` 响应头是否存在、持续时间以及作用范围。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中* |

> *如果应用程序处理个人身份信息 (Personally Identifiable Information, PII)、会话令牌或凭证，则严重程度变为“中”，因为缺少 HSTS 会增加中间人攻击 (MitM) 的成功率。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### HTTPS 响应中是否存在 `Strict-Transport-Security` 头？
- [ ] 是 — 所有敏感端点上**均存在**该响应头  
- [ ] 是 — **存在**该响应头，但仅限于起始页 (Landing Page)  
- [ ] 否 — 应用程序完全**缺失**该响应头  

### `max-age` 指令设置的持续时间是否足够？
- [ ] 是 — `max-age` 设置为一年或更久（例如：`31536000`）  
- [ ] 是 — 已设置 `max-age` 但**过短**（例如：少于 6 个月）  
- [ ] 否 — `max-age` 指令**缺失**或设置为 `0` *(高危)*  

### HSTS 策略是否覆盖所有子域名？
- [ ] 是 — **已应用** `includeSubDomains` 指令  
- [ ] 否 — **缺失** `includeSubDomains` 指令，导致子域名容易受到降级攻击  
- [ ] 否 — 该功能不适用，因为不存在子域名  

### 应用程序是否注册了 HSTS 预加载 (Preloading)？
- [ ] 是 — **存在** `preload` 指令且域名已在 HSTS 预加载列表中  
- [ ] 是 — **存在** `preload` 指令但域名**尚未**列入预加载列表  
- [ ] 否 — **缺失** `preload` 指令，允许在首次访问时存在中间人攻击 (MitM) 的窗口  

### HSTS 响应头是否错误地通过明文 HTTP 发送？
- [ ] 否 — 响应头**仅**通过安全的 HTTPS 连接发送  
- [ ] 是 — 响应头通过 HTTP **发送**，这会被浏览器忽略并可能泄露配置细节

---

## WSTG-CONF-08 — 测试 RIA 跨域策略

富互联网应用 (Rich Internet Application, RIA) 跨域策略涉及 `crossdomain.xml` 和 `clientaccesspolicy.xml` 等文件，这些文件定义了 Web 客户端（特别是 Adobe Flash 和 Microsoft Silverlight）如何处理跨源请求。这些文件对安全至关重要，因为它们明确地覆盖了同源策略 (Same-Origin Policy, SOP)，决定了哪些外部域被允许与应用程序交互并读取其响应。过度宽松的配置（例如在 `domain` 属性中使用通配符）允许攻击者在第三方站点上托管恶意 RIA 对象，从而窃取敏感数据或代表已认证用户执行操作。渗透测试 (Pentesting) 人员通常在 Web 根目录下定位这些文件，以评估授予第三方源的信任级别，并识别潜在的跨源数据泄露。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果应用程序处理可以通过宽松策略窃取的敏感用户数据或会话标识符，严重程度将变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Web 根目录下是否存在跨域策略文件（`crossdomain.xml` 或 `clientaccesspolicy.xml`）？
- [ ] 否 — 根目录下未找到文件  
- [ ] 是 — 存在 `crossdomain.xml` (Flash)  
- [ ] 是 — 存在 `clientaccesspolicy.xml` (Silverlight)  
- [ ] 是 — 两种 RIA 策略文件都存在  

### `allow-access-from` 域属性是否仅限于受信任的源？
- [ ] 否 — 文件存在但未包含 `allow-access-from` 条目  
- [ ] 是 — 明确白名单化了特定的受信任域，且无法绕过  
- [ ] 是 — 域已加入白名单，但包括不受信任的第三方或子域  
- [ ] 是 — 使用了通配符 `*`，允许来自任何源的访问 *(严重/Critical)*  

### 策略是否允许通过 HTTP 对 HTTPS 资源进行不安全通信？
- [ ] 否 — 设置了 `secure="true"`（或为默认值），防止 HTTP 源访问 HTTPS 数据  
- [ ] 是 — 明确设置了 `secure="false"`，允许中间人攻击 (Man-in-the-Middle, MITM) 攻击者窃取加密数据  

### 跨域配置中的 HTTP 请求头是否过度宽松？
- [ ] 否 — `allow-http-request-headers-from` 已受限或未启用  
- [ ] 是 — 允许受信任域使用特定请求头  
- [ ] 是 — 请求头使用了通配符 `*`，允许攻击者从任何域发送自定义请求头  

### `site-control`（主策略）配置是否具有限制性？
- [ ] 否 — `permitted-cross-domain-policies` 设置为 `none` 或 `master-only`  
- [ ] 是 — 策略允许服务器上的其他文件定义自己的规则 (`all`)

---

## WSTG-CONF-09 — 文件权限测试 (Test File Permission)

测试文件权限涉及审计分配给 Web 服务器 (Web Server) 上文件和目录的访问控制 (Access Control) 级别，以确保敏感资源不会过度暴露给未经授权的用户或进程。配置不当的权限可能允许攻击者读取包含数据库凭据 (Database Credentials) 的敏感配置文件，查看专有源代码，或修改服务器端脚本以实现远程代码执行 (Remote Code Execution, RCE)。此漏洞通常发生在部署阶段，当时默认的“全局可读 (World-readable)”或“全局可写 (World-writable)”标志被保留在关键应用程序目录上，例如配置路径、日志文件夹或上传存储库。从攻击者的角度来看，发现一个安全性不当的文件通常是权限提升 (Privilege Escalation) 或整个系统失陷的主要诱因。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果敏感配置文件（如 `.env`、`web.config`）可读，或者可以对 Web 根目录 (Web Root) 进行写访问，严重程度将变为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**工具：** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### 敏感应用程序配置文件是否受到保护，防止未经授权的访问？
- [ ] 是 — 配置文件（如 `.env`, `config.php`）**无法**通过 Web 请求访问  
- [ ] 是 — 文件存在，但访问权**仅限**于授权的本地用户  
- [ ] 否 — 敏感配置文件**可被访问**并泄露凭据或机密信息  

### 攻击者能否修改 Web 根目录内的文件或目录？
- [ ] 否 — 所有 Web 根目录对于 Web 服务器用户均为**只读**  
- [ ] 是 — 存在可写目录，但脚本执行已**禁用**  
- [ ] 是 — 存在全局可写目录，并**允许**上传和执行脚本 *(严重)*  

### 是否由于权限松懈而导致版本控制元数据或备份文件暴露？
- [ ] 否 — `.git`, `.svn` 和备份文件（如 `.bak`, `~`）**不存在**或受保护  
- [ ] 是 — 存在元数据目录，但目录列表已**禁用**  
- [ ] 是 — 敏感元数据或备份**完全可访问**，从而导致源代码泄露  

### Web 服务器用户是否以最小权限原则 (Principle of Least Privilege) 运行？
- [ ] 是 — Web 服务器进程以**专用低权限**用户身份运行  
- [ ] 否 — Web 服务器进程以**不必要的权限**（如 `root` 或 `Administrator`）运行  

### 日志文件是否受到保护，防止未经授权的读取或修改？
- [ ] 是 — 日志文件访问**仅限**于管理用户  
- [ ] 是 — 日志文件可读，但不包含会话令牌 (Session Tokens) 或个人身份信息 (PII)  
- [ ] 否 — 日志文件**公开可读**，并泄露敏感交易数据或用户信息

---

## WSTG-CONF-10 — 子域名接管 (Subdomain Takeover)

当 DNS 条目（通常是 CNAME，但也偶尔包括 A 或 MX 记录）指向第三方云提供商或托管服务上已停用或不存在的资源时，就会发生子域名接管 (Subdomain Takeover)。攻击者通过寻找来自 AWS、GitHub 或 Azure 等提供商的特定错误消息（表明资源不再被占用）来识别这些“悬空”DNS 记录 (Dangling DNS records)。通过在提供商平台上注册该弃用的资源名称，攻击者可以获得对子域名的完全控制权，从而允许他们托管恶意内容、窃取作用域在根域名的会话 Cookie (Exfiltrate Session Cookies)，并绕过内容安全策略 (Content Security Policy) 或 CORS 保护。这种漏洞特别危险，因为它利用了与组织合法域名结构相关的信任，从而实现高影响力的网络钓鱼 (Phishing) 和数据窃取。


| 字段 | 取值 |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重* |

> *如果子域名可用于劫持来自根域名的会话 Cookie 或绕过 SSO/OAuth 重定向，严重程度将变为“严重”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**工具：** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### 应用程序是否通过子域名利用第三方托管或云服务？
- [ ] 否 — 所有子域名均指向组织控制的 IP 空间  
- [ ] 是 — 子域名指向第三方提供商（S3, GitHub Pages, Heroku 等）  

### 是否存在指向不存在资源的“悬空”DNS 记录？
- [ ] 否 — 所有识别出的 DNS 记录均解析为受控的活动资源  
- [ ] 是 — 存在指向提供商处**不再存在**的资源的 CNAME 或 A 记录  
- [ ] 是 — DNS 记录解析为 NXDOMAIN 或特定于提供商的错误页面 *（例如 "NoSuchBucket"）*  

### 识别出的悬空资源是否可以被成功申领？
- [ ] 否 — 提供商具有防止申领弃用名称的保护措施，或者需要进行账户验证  
- [ ] 是 — 该资源名称**可以**被任何用户在第三方平台上注册  

### 根域名是否使用了可能被攻破的“域名”作用域 (Domain-scoped) Cookie？
- [ ] 否 — Cookie 严格限定在特定的子域名作用域，或使用了 `Host-Only` 标志  
- [ ] 是 — 敏感 Cookie（会话 ID、JWT）的作用域设定为父域名，并且**可以**通过被劫持的子域名窃取  

### 该子域名是否可被用于绕过安全响应头 (Security Headers) 或身份验证流程？
- [ ] 否 — 识别出的子域名不存在安全依赖关系  
- [ ] 是 — 该子域名在 CSP 的 `script-src` 或 `connect-src` 指令白名单中  
- [ ] 是 — 该子域名是 OAuth 或 SSO 配置中的有效重定向 URI (Redirect URI)

---

## WSTG-CONF-11 — 测试云存储

云存储错误配置 (Cloud Storage Misconfigurations) 测试涉及识别和审计可公开访问的对象存储 (Object Storage) 服务，例如 Amazon S3、Azure Blobs 或 Google Cloud Storage。这些资源通常包含敏感数据，包括由于过度宽松的访问控制列表 (ACLs) 或身份与访问管理 (IAM) 策略而暴露的备份、源代码、个人身份信息 (PII) 或配置文件。攻击者通过对 JavaScript 文件、HTML 源代码进行侦察 (Reconnaissance) 以及暴力破解 (Brute-force) 技术来枚举存储桶 (Bucket) 名称，以寻找数据渗出 (Exfiltration) 或未经授权的文件修改入口点。利用此类漏洞可能导致重大影响，包括完整的数据泄露或利用受信任域名分发恶意文件的能力。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重* |

> *如果 PII、凭据或生产环境备份可被访问，或者启用了未经身份验证的写入访问，则严重程度变为“严重 (Critical)”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### 是否已识别并枚举云存储端点（存储桶/容器）？
- [ ] 否 —— 未使用或无法发现云存储端点  
- [ ] 是 —— 通过代码分析或流量监控识别出存储端点  
- [ ] 是 —— 通过命名常规暴力破解发现存储端点  

### 未经身份验证的用户能否列出云存储的内容？
- [ ] 否 —— 存储桶列表功能已**禁用**并返回 403 Forbidden 或 404 Not Found  
- [ ] 是 —— 列表功能已**启用**，但不存在敏感文件  
- [ ] 是 —— 列表功能已**启用**，且敏感文件名/元数据可见  

### 是否可以从识别出的存储桶中下载敏感文件？
- [ ] 否 —— 文件受 IAM 策略保护，或需要有效的身份验证  
- [ ] 是 —— 文件可访问，但需要难以被猜测的签名 URL (Signed URL)  
- [ ] 是 —— 敏感文件（备份、`.env`、PII）可**公开访问**并下载 *(严重)*  

### 是否允许未经身份验证的文件上传或修改？
- [ ] 否 —— **不可能**进行未经身份验证的上传或修改  
- [ ] 是 —— 需要经过身份验证的上传，但通过弱策略条件**可以**进行绕过  
- [ ] 是 —— **可以**进行未经身份验证的文件写入、覆盖或删除 *(严重)*  

### 存储端点上的跨源资源共享 (CORS) 策略是否具有限制性？
- [ ] 是 —— 跨源资源共享 (CORS) 已**禁用**或仅限于特定的受信任源 (Origin)  
- [ ] 否 —— CORS 已**启用**并使用通配符 (Wildcard) `*` 源，允许未经授权的基于 Web 的访问

---

## WSTG-CONF-12 — 内容安全策略 (Content Security Policy, CSP)

内容安全策略 (Content Security Policy, CSP) 是一种关键的深度防御机制，通过 HTTP 响应标头实现，旨在减轻跨站脚本 (Cross-Site Scripting, XSS)、点击劫持 (Clickjacking) 和数据注入 (Data injection) 等攻击风险。通过定义允许加载哪些动态资源，配置良好的 CSP 可以限制攻击者执行未授权脚本或将敏感数据外泄到外部域的能力。渗透测试人员会评估该策略是否存在过度宽泛的指令、是否使用了 `'unsafe-inline'` 等不安全的关键词，以及是否依赖于可能导致绕过 (Bypass) 的不安全内容分发网络 (CDNs)。配置错误或缺失 CSP 本身并不会直接产生漏洞，但由于未能提供二次保护层，会显著增加成功注入攻击的影响。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 低 / 中 (Low / Medium) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**工具：** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### 应用程序响应中是否存在内容安全策略 (CSP) 标头？
- [ ] 是 — `Content-Security-Policy` 标头**已存在**并**已强制执行**  
- [ ] 是 — `Content-Security-Policy-Report-Only` **已存在**用于测试  
- [ ] 否 — 响应中**不存在** CSP 标头  

### `script-src` 或 `default-src` 指令是否得到了妥善限制？
- [ ] 是 — 指令使用严格的白名单、Nonce 或 Hash，且**无法绕过**  
- [ ] 是 — 指令**已妥善配置**，但白名单包含已知的可绕过 CDNs  
- [ ] 否 — 指令使用通配符 (`*`) 或 `data:` 方案，导致**可能被绕过**  

### 该策略是否允许执行不安全的内联脚本或 eval？
- [ ] 否 — **不存在** `'unsafe-inline'` 和 `'unsafe-eval'`  
- [ ] 是 — **存在** `'unsafe-inline'`，但受到 `nonce` 或 `hash` 保护  
- [ ] 是 — **启用了** `'unsafe-inline'` 或 `'unsafe-eval'` 且无额外保护 *(高危)*  

### 应用程序是否通过 `frame-ancestors` 指令防御点击劫持？
- [ ] 是 — `frame-ancestors` 设置为 `'none'` 或 `'self'`  
- [ ] 是 — `frame-ancestors` **已启用**，但允许特定的受信任第三方域  
- [ ] 否 — `frame-ancestors` **缺失**，依赖于过时的 `X-Frame-Options` 或无任何防护  

### 是否存在报告 CSP 违规的机制？
- [ ] 是 — `report-uri` 或 `report-to` **已配置**并处于活动状态  
- [ ] 否 — 违规报告**已禁用**或**未配置**

---

## WSTG-CONF-13 — Path Confusion (路径混淆)

路径混淆 (Path Confusion) 源于不同的 Web 组件（如反向代理 (Reverse Proxy)、负载均衡器 (Load Balancer) 和后端应用服务器 (Backend Application Server)）在解析和解释 URL 路径 (URL Path) 时存在的差异。攻击者利用这些不一致性，通过注入特定字符（如分号、编码斜杠或点段 (Dot-segments)）来欺骗安全过滤器，并访问受限的端点或静态资源。这种漏洞可能导致关键的安全失败，包括身份验证绕过 (Authentication Bypass)、未授权数据访问以及 Web 缓存毒化 (Web Cache Poisoning)，因为前端可能会将安全规则应用于后端以不同方式解释的路径。成功的利用通常发生在技术栈中请求路由 (Request Routing) 和规范化 (Normalization) 逻辑发散的架构边界处。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果路径混淆导致身份验证绕过或敏感管理端点的访问控制失效，则严重程度为“高”。

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### 不同的架构层对路径分隔符（如 `;`、`#`、`?`）的解释是否一致？
- [ ] 是 — 所有层都以相同的方式规范化并解释路径分隔符  
- [ ] 否 — 存在不一致性，但**无法**用于访问受限资源  
- [ ] 否 — 差异允许攻击者绕过前端过滤器并到达后端逻辑，这是**可能**的  

### 是否可以使用路径遍历 (Path Traversal) 序列或编码字符绕过访问控制？
- [ ] 否 — 在安全检查之前始终应用规范化  
- [ ] 是 — 通过点段序列（例如 `/admin/..;/`）**可能**实现绕过  
- [ ] 是 — 通过 URL 编码字符（例如 `%2f`、`%2e%2e%2f`）**可能**实现绕过  

### 应用程序是否容易通过路径混淆受到 Web 缓存毒化 (Web Cache Poisoning) 的影响？
- [ ] 否 — 缓存逻辑不受路径歧义或额外路径信息的影响  
- [ ] 是 — 由于映射差异，恶意内容**可以**被缓存到合法路径中  
- [ ] 是 — 敏感信息**可以**通过 `RCD` (Relative Path Overwrite) 技术缓存到公共目录中  

### 服务器是否安全地处理“额外路径信息” (PathInfo)？
- [ ] 否 — 该功能已**禁用**或不存在  
- [ ] 是 — `PathInfo` 已处理且**不**会干扰安全过滤器  
- [ ] 否 — `PathInfo` 允许攻击者附加数据，从而混淆应用程序的路由逻辑

---

## WSTG-CONF-14 — 测试其他 HTTP 安全响应头配置错误

测试 HTTP 安全响应头配置错误 (HTTP security header misconfigurations) 涉及验证防御性响应头的存在及其实施的正确性，这些响应头为常见的 Web 攻击向量提供了必要的保护。虽然这些响应头并不能修复底层代码漏洞，但它们的缺失会显著增加中间人攻击 (MITM)、MIME 类型嗅探 (MIME-sniffing) 利用以及通过引用来源 (Referrer) 导致的意外跨源数据泄露的风险。从攻击者的角度来看，缺失安全响应头是环境加固程度较低的高可见性指标，通常会辅助更复杂的攻击链，如会话劫持 (Session hijacking) 或信息外泄 (Information exfiltration)。这些配置通常在服务器层级进行评估，并应一致地应用于所有响应类型，包括 API 终端节点 (API endpoints) 和静态资源。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### 安全响应头存在性审计
| 响应头 | 已部署 | 缺失 | 建议配置 |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` 或 `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (特定功能相关，例如：`geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### 安全响应头在应用程序范围内的强制执行情况如何？
- [ ] 是 — 响应头通过中央负载均衡器 (Load balancer) 或反向代理 (Reverse proxy) 全局应用，且**无法**被覆盖  
- [ ] 是 — 响应头已应用于主文档响应，但在 API 响应或静态资源中**缺失**  
- [ ] 否 — 响应头应用不一致，或在整个应用程序域名中**缺失**  

### Strict-Transport-Security (HSTS) 响应头配置是否正确？
- [ ] 是 — `max-age` 设置为较长时间（如 2 年）且 `includeSubDomains` 已**启用**  
- [ ] 是 — `max-age` 已设置，但 `includeSubDomains` 已**禁用**，导致子域名存在风险  
- [ ] 否 — HSTS 响应头**不存在**或 `max-age` 设置为 0，允许协议降级攻击 (Protocol downgrade attacks)  

### Referrer-Policy 是否防止了敏感 URL 参数的泄露？
- [ ] 是 — 策略设置为 `no-referrer` 或 `same-origin` *(最安全)*  
- [ ] 是 — 策略设置为 `strict-origin-when-cross-origin`  
- [ ] 否 — 策略**未设置**或设置为 `unsafe-url`，可能导致令牌 (Tokens) 或敏感个人身份信息 (PII) 在引用来源头中泄露  

### X-Content-Type-Options 响应头是否防止了 MIME 类型嗅探？
- [ ] 是 — `nosniff` 指令**存在**并已正确实施  
- [ ] 否 — 响应头**缺失**，可能允许浏览器将非可执行文件（如图像）作为脚本执行

---

## WSTG-IDNT-01 — 测试角色定义 (Test Role Definitions)

测试角色定义涉及识别和记录应用程序中定义的各种用户角色和权限级别，以确保它们在逻辑上是分离的，并遵循最小权限原则 (Principle of Least Privilege)。攻击者通过分析应用程序来映射高权限角色与标准用户角色，寻找权限边界中的任何模糊之处或未记录的管理功能。识别这些角色是发现权限提升 (Privilege Escalation) 路径的第一步，因为角色定义中的不一致通常会导致对敏感数据或管理功能的未经授权访问。此阶段通常发生在初始侦查和业务逻辑映射期间，重点关注用户配置文件、管理仪表板和 API 权限结构。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 低 / 中 (Low / Medium) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### 应用程序及其文档中是否明确定义并记录了角色？
- [ ] 是 — 角色已完整记录并遵循 **最小权限原则 (Least Privilege)**  
- [ ] 是 — 角色已记录，但包含 **重叠权限**  
- [ ] 否 — 不存在正式的角色文档或定义  

### 管理功能与标准用户功能之间是否有明确的分离？
- [ ] 是 — 管理功能与标准用户视图 **完全隔离**  
- [ ] 是 — 存在分离，但管理端点 (Endpoints) 是 **可预测或可猜测的**  
- [ ] 否 — 管理功能和标准功能在同一界面中 **混合**  

### 应用程序是否使用集中化机制进行基于角色的访问控制 (RBAC)？
- [ ] 是 — 已 **实现** 并一致强制执行集中化的 RBAC 或基于属性的访问控制 (ABAC)  
- [ ] 是 — 存在分散的控制，但在各模块间 **应用不一致**  
- [ ] 否 — 访问控制逻辑被 **硬编码 (Hardcoded)** 在单个页面或组件中  

### 系统中是否存在未记录或隐藏的角色？
- [ ] 否 — 所有发现的角色均与记录的功能相符  
- [ ] 是 — **存在** 隐藏或“影子”角色（例如：`super_admin`、`support`、`test`）  

### 低权限用户是否可以枚举 (Enumerate) 其他用户的权限或角色？
- [ ] 否 — 用户 **无法** 查看或枚举其他实体的角色/权限  
- [ ] 是 — 用户 **可以** 通过个人资料页面、元数据或 API 响应枚举角色 *(中危)*

---

## WSTG-IDNT-02 — 测试用户注册流程

测试用户注册流程涉及评估创建新身份的工作流，以确保其不会被滥用于未经授权的访问或自动化利用。此过程中的漏洞通常表现为缺乏身份验证（电子邮件/短信）、容易受到机器人大规模创建帐户的影响，或通过显示现有用户名的冗长错误消息进行信息泄露。攻击者利用这些缺陷执行用户枚举 (User Enumeration)，开展大规模垃圾邮件活动，或在帐户创建阶段通过篡改注册参数来获得权限提升 (Privilege Escalation)。此项测试对于保护应用程序的完整性以及防止攻击者向系统填充虚假或恶意帐户至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **测试状态** | 未执行 |
| **严重程度** | 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### 在新帐户激活前是否需要进行身份验证？
- [ ] 是 — 注册需要通过电子邮件或短信发送唯一的、有时间限制的令牌 (Token)  
- [ ] 是 — 需要验证，但令牌**强度弱**或**可预测**  
- [ ] 否 — 帐户在没有任何验证步骤的情况下**立即**激活  

### 注册流程是否防止用户枚举 (User Enumeration)？
- [ ] 是 — 应用程序对新邮件/用户名和现有邮件/用户名返回**完全相同**的响应  
- [ ] 否 — 错误消息（例如，“电子邮件已被使用”）**允许**攻击者映射已注册用户  
- [ ] 否 — 服务器响应的时间差异会**泄露**用户是否存在  

### 是否实施了反自动化控制以防止大规模注册？
- [ ] 是 — **存在**且**有效**的验证码 (CAPTCHA) 或工作量证明机制  
- [ ] 是 — 存在控制措施，但**可以**通过 API 终端或标头篡改进行绕过  
- [ ] 否 — 未对注册终端应用速率限制 (Rate Limiting) 或验证码  

### 注册参数是否可以被篡改以实现权限提升？
- [ ] 否 — 用户角色**严格**由服务器端逻辑分配  
- [ ] 是 — 请求中的 `role`、`admin` 或 `group_id` 等参数**可以**被修改以获得更高访问权限  

### 在注册阶段是否强制执行密码策略？
- [ ] 是 — 服务器端**强制执行**强密码要求  
- [ ] 否 — 系统**接受**弱密码（例如 “password123”）

---

## WSTG-IDNT-03 — 测试账户配置流程 (Test Account Provisioning Process)

账户配置流程 (Account Provisioning Process) 规定了如何在应用程序环境中创建、验证新身份并分配权限。攻击者通常以该工作流为目标，进行用于发送垃圾邮件或拒绝服务 (Denial-of-Service, DoS) 的自动化批量注册，绕过身份验证步骤以创建虚假账户，或在注册阶段操纵参数以授予自己更高的权限。漏洞通常表现在自助服务注册表单、仅限邀请的系统或管理配置控制台中，在这些地方，业务逻辑缺陷 (Business Logic Flaws) 可能导致未经授权的账户创建或权限提升 (Privilege Escalation)。从攻击者的角度来看，攻破配置流程可以获得一个合法的立足点，作为进一步漏洞利用 (Exploit)、数据外泄或社会工程学 (Social Engineering) 攻击的跳板。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果攻击者可以配置管理员账户或绕过全局注册限制，严重程度将变为严重 (Critical)。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### 自助注册是否受到严格控制或仅限于授权用户？
- [ ] 是 — 注册功能已**禁用**或需要手动管理员审批  
- [ ] 是 — 注册开放，但仅限于**授权**的电子邮件域名或邀请令牌 (Invitation Tokens)  
- [ ] 否 — 注册对任何用户**开放**，且没有域名或令牌限制  

### 在激活账户之前，是否需要稳健的身份验证机制（例如：电子邮件/短信）？
- [ ] 是 — 验证是**必需的**且**无法**被绕过  
- [ ] 是 — 验证是**必需的**，但可以通过参数篡改 (Parameter Tampering) 或可预测的令牌**绕过**  
- [ ] 否 — 提交后账户**立即激活**，无需任何验证  

### 用户在注册请求期间是否可以操纵角色或权限参数？
- [ ] 否 — 角色在**服务端分配**，不存在客户端参数  
- [ ] 是 — 存在角色参数，但由于服务端验证，操纵是**不可能的**  
- [ ] 是 — 角色/权限参数（例如：`role=admin`, `is_staff=true`）**可以**被修改以获得提升的访问权限 *(严重)*  

### 是否实施了速率限制 (Rate Limiting) 或反自动化控制以防止批量创建账户？
- [ ] 是 — 速率限制和验证码 (CAPTCHA) 已**启用**且有效  
- [ ] 是 — 存在控制措施，但可以通过请求头伪造 (Header Spoofing) 或验证码破解服务**绕过**  
- [ ] 否 — **未应用**任何速率限制或反自动化控制  

### 配置流程是否会泄露现有账户的信息（用户枚举）？
- [ ] 否 — 现有用户和不存在用户的响应消息是**一致的**  
- [ ] 是 — 不同的响应或响应时间差异**允许**对现有用户进行用户枚举 (User Enumeration)

---

## WSTG-IDNT-04 — 账户枚举 (Account Enumeration) 和可猜测用户账户测试

账户枚举 (Account Enumeration) 发生在应用程序通过 HTTP 响应的变化、错误消息或可测量的时间差异泄露用户账户是否存在时。攻击者通过系统地提交用户名列表来识别有效账户，从而利用这一行为进行撞库 (Credential Stuffing)、暴力破解 (Brute Force) 或社会工程学 (Social Engineering) 攻击。这种漏洞通常出现在登录入口、密码重置功能、注册表单以及处理用户特定标识符的 API 端点中。从攻击者的角度来看，成功的枚举通过将盲目的暴力破解尝试转变为针对已知有效身份的定向利用，显著缩小了攻击面。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### 身份验证错误消息在所有场景下是否一致？
- [ ] 是 — 对于无效用户名和无效密码均使用通用的错误消息  
- [ ] 是 — 消息类似，但在标点符号或大小写上**存在**细微差异  
- [ ] 否 — 错误消息明确指出“用户未找到”或“密码错误” *(存在漏洞)*  

### 密码重置或“忘记密码”流程是否泄露账户存在性？
- [ ] 否 — 无论电子邮件/用户名是否存在，应用程序均返回通用消息  
- [ ] 是 — 已部署控制措施，但通过响应长度或状态码**可以绕过**  
- [ ] 是 — 应用程序明确确认是否发送了重置邮件，或者账户**不存在**  

### 注册过程是否针对用户枚举进行了防护？
- [ ] 否 — 注册不会泄露信息，或使用 CAPTCHA/速率限制 (Rate Limiting) 来防止批量检查  
- [ ] 是 — 已部署控制措施，但通过时间差异或 API 响应差异**可以绕过**  
- [ ] 是 — 如果用户名或电子邮件**已被占用**，应用程序会立即通知用户  

### 在比较有效与无效用户名时，时间差异是否可测量？
- [ ] 否 — 由于采用了恒定时间比较，无论账户是否有效，响应时间均保持一致  
- [ ] 是 — 存在时间差异，但差异太小，无法通过网络可靠地测量  
- [ ] 是 — **存在**可测量的时间差异（例如，由于仅对有效用户执行密码哈希 (Password Hashing) 计算）  

### 应用程序是否对账户使用可预测或可猜测的命名约定？
- [ ] 否 — 账户标识符是随机的 (UUIDs) 或由用户定义且复杂的  
- [ ] 是 — 账户标识符遵循特定模式（例如 `emp001`, `emp002`），且枚举**是可能的**  
- [ ] 是 — 标识符是连续的整数，使得完整的数据库枚举**成为可能**

---

## WSTG-IDNT-05 — 测试薄弱或未强制执行的用户名策略 (Username Policy)

测试薄弱或未强制执行的用户名策略侧重于管理账户标识符创建的规则，以及它们对枚举 (Enumeration) 或欺骗 (Spoofing) 的敏感性。薄弱的策略通常允许可预测的序列、常见的字典词汇或反映内部员工 ID 的标识符，这会显著缩小暴力破解 (Brute Force) 和凭据填充 (Credential Stuffing) 攻击的搜索空间。从攻击者的角度来看，识别这些模式是进行大规模账户发现的第一步，通常通过分析注册错误消息、密码重置行为或面向公众的配置文件中的元数据来实现。确保强健的策略可以防止攻击者系统地映射用户群，并有助于防御针对性的网络钓鱼和自动化的身份验证绕过 (Authentication Bypass) 尝试。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### 用户名是否基于高度可预测或顺序的模式？
- [ ] 否 —— 用户名是随机的、用户定义的或高熵字符串  
- [ ] 是 —— 用户名遵循可预测的格式（例如 `user1001`，`user1002`），但身份验证 (Authentication) 机制是强健的  
- [ ] 是 —— 用户名是**严格顺序**的或遵循已知的企业格式，便于枚举  

### 应用程序是否对用户名强制执行最小长度和复杂度要求？
- [ ] 是 —— 在注册期间**强制执行**严格的长度和字符集策略  
- [ ] 是 —— 存在策略，但允许极短（例如 1-2 个字符）或过于简单的用户名  
- [ ] 否 —— **没有强制执行**关于用户名长度或字符类型的策略  

### 是否可以通过应用程序响应枚举有效的用户名？
- [ ] 否 —— 应用程序返回通用的错误消息，并对所有尝试表现出一致的响应时间  
- [ ] 是 —— 应用程序返回不同的错误消息（例如“用户名已被占用”），但**应用了**速率限制 (Rate Limiting)  
- [ ] 是 —— 应用程序通过注册错误、密码重置或时间差异**泄露**有效的用户名  

### 该策略是否防止注册常见的或保留的管理用户名？
- [ ] 是 —— 应用程序封禁了常用名称（例如 `admin`，`root`，`support`）和系统保留术语  
- [ ] 否 —— 用户**可以**使用敏感名称或管理名称注册账户  

### 用户名策略在所有入口点（API、移动端、Web）是否一致？
- [ ] 是 —— 策略在所有接口和版本中**一致应用**  
- [ ] 否 —— 遗留端点或特定 API 版本**未强制执行**相同的用户名约束

---

## WSTG-ATHN-01 — 测试通过加密通道传输的凭据

测试通过加密通道传输的凭据旨在确保敏感的身份验证数据（如用户名、密码和会话令牌 (Session Tokens)）在传输过程中受到保护，防止被截获。位于网络中的攻击者——例如通过公共 Wi-Fi 或受损内部网络进行的中间人攻击 (Man-in-the-Middle (MitM))——如果未严格执行传输层安全协议 (TLS/SSL)，可以使用数据包嗅探 (Packet Sniffing) 工具捕获明文凭据。这种漏洞通常出现在登录端点 (Login Endpoints)、密码重置表单和 API 身份验证标头 (Authentication Headers) 中，原因是应用程序未能强制执行 HTTPS 或从 HTTP 重定向不当。成功利用此漏洞可使攻击者获得用户账户的完全访问权限，导致用户数据被全面攻破，并可能在应用程序内部进行横向移动 (Lateral Movement)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**工具：** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### 整个身份验证过程是否强制执行 HTTPS？
- [ ] 是 — 已启用 HSTS 且所有流量均被严格强制通过 HTTPS 传输  
- [ ] 是 — 存在到 HTTPS 的重定向，但未启用 HSTS  
- [ ] 否 — 凭据**可以**通过未加密的 HTTP 提交  

### 登录页面和表单是否通过加密通道提供？
- [ ] 是 — 包含登录表单的页面是通过 HTTPS 交付的  
- [ ] 否 — 登录页面通过 HTTP 提供，允许表单操作劫持 (Form-Action Hijacking) 或凭据嗅探  

### 是否为所有敏感会话 Cookie 应用了 `Secure` 标志？
- [ ] 是 — 已为所有身份验证和会话 Cookie **应用**了 `Secure` 标志  
- [ ] 是 — 仅为部分 Cookie **应用**了 `Secure` 标志  
- [ ] 否 — **未应用** `Secure` 标志，导致 Cookie 可能通过未加密的请求泄露 (Exfiltrated)  

### 服务器是否支持弱 TLS 版本或不安全的密码套件 (Cipher Suites)？
- [ ] 否 — 仅**启用**了现代 TLS (1.2+) 和强密码套件  
- [ ] 是 — **支持**遗留版本 (TLS 1.0/1.1) 或弱密码算法 (RC4, DES, CBC)  

### 应用程序是否防止凭据通过未加密通道传输到第三方域名？
- [ ] 是 — 未向外部域名发送敏感数据，或所有外部调用均强制通过 HTTPS 传输  
- [ ] 否 — 身份验证标头或凭据通过 HTTP 发送到第三方端点（例如分析工具或单点登录 (SSO)）

---

## WSTG-ATHN-02 — 默认凭证测试 (Testing for Default Credentials)

默认凭证测试 (Testing for Default Credentials) 涉及识别仍在使用出厂设置或供应商提供的用户名和密码的管理接口、服务或应用程序组件。这种漏洞是攻击者的高优先级目标，因为它通常能以极小的代价直接导致系统完全被攻破、获取管理权限或实现远程代码执行 (Remote Code Execution, RCE)。它通常出现在商业现成软件 (Off-the-shelf software)、内容管理系统 (CMS) 平台、数据库管理工具以及物联网 (IoT) 设备或网络设备等集成硬件中。在初始侦察 (Reconnaissance) 和漏洞利用 (Exploitation) 阶段，攻击者通常通过将识别出的软件版本与公共默认密码数据库进行交叉对比，或使用自动化的凭证填充 (Credential Stuffing) 工具来进行利用。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **测试状态** | 未执行 |
| **严重程度** | 严重 / 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**工具：** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### 管理或维护接口是否暴露在互联网或不受信任的网络网段中？
- [ ] 否 —— 无法访问任何管理接口  
- [ ] 是 —— 存在接口，但受网络层控制（如 VPN、IP 白名单 (IP Allowlist)）限制  
- [ ] 是 —— 接口**已**公开访问且需要身份验证  

### 是否已针对所有识别出的组件更改了供应商提供的默认凭证？
- [ ] 是 —— 所有组件的默认凭证**均已更改**（最安全）  
- [ ] 是 —— 主应用程序的凭证**已更改**，但次要组件（如 CMS、数据库）仍保留默认设置  
- [ ] 否 —— 默认凭证在一个或多个可识别的接口上**处于活动状态**（严重）  

### 应用程序是否强制新账户或管理账户在首次登录时更改密码？
- [ ] 是 —— 在执行任何管理操作之前，更改密码是**强制性的**  
- [ ] 否 —— 应用程序**允许**无限期使用默认凭证  

### 是否激活了锁定机制或速率限制控制以防止自动化的默认凭证测试？
- [ ] 是 —— **已应用**严格的速率限制 (Rate Limiting) 或账户锁定 (Account Lockout)  
- [ ] 是 —— 存在控制措施，但通过标头篡改或 IP 轮转**可能实现**绕过  
- [ ] 否 —— **未启用**任何速率限制或锁定机制  

### 第三方插件、中间件或管理工具（如 phpMyAdmin, Tomcat Manager）是否使用唯一的凭证？
- [ ] 否 —— 环境中不存在第三方组件  
- [ ] 是 —— 所有第三方组件均使用唯一的非默认凭证  
- [ ] 否 —— 至少有一个第三方组件可通过已知的默认凭证进行**访问**

---

## WSTG-ATHN-03 — 弱锁定机制测试

账户锁定机制 (Account Lockout Mechanism) 是一种关键的纵深防御控制措施，旨在通过在指定次数的身份验证 (Authentication) 尝试失败后禁用账户，来缓解暴力破解 (Brute-Force) 和凭证填充 (Credential Stuffing) 攻击。如果没有稳健的锁定策略，攻击者可以使用自动化工具系统地猜测多个账户的密码（密码喷洒 (Password Spraying)），或针对单个高价值账户进行尝试直到成功。这种漏洞通常出现在缺乏速率限制 (Rate Limiting) 或账户级保护的登录门户、密码重置表单和 API 端点上。从攻击者的角度来看，弱锁定机制或缺乏锁定机制允许无限次的密码尝试，而过于激进的锁定机制则可能被利用，通过故意锁定合法用户的账户来造成针对这些用户的拒绝服务 (Denial of Service, DoS) 攻击。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果应用程序处理敏感的个人身份信息 (PII)、财务数据，或者管理账户在没有任何辅助控制的情况下容易受到暴力破解的影响，严重程度将变为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**工具：** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### 是否在达到设定的失败尝试次数后强制执行账户锁定机制？
- [ ] 是 — 账户在达到定义的、合理的阈值（例如 5-10 次尝试）后被锁定  
- [ ] 是 — 账户虽然被锁定，但仅在尝试次数过高后才执行  
- [ ] 否 — 账户无法被锁定，并允许无限次的暴力破解  

### 是否可以使用常见的规避技术绕过锁定机制？
- [ ] 否 — 锁定在服务端 (Server-side) 强制执行，不受客户端 (Client-side) 更改的影响  
- [ ] 是 — 可以通过轮换 IP 地址或使用代理池来绕过锁定  
- [ ] 是 — 可以通过操作如 `X-Forwarded-For` 或 `User-Agent` 等 HTTP 标头 (HTTP Headers) 来绕过锁定  
- [ ] 是 — 可以通过清除 Cookie 或启动新会话来绕过锁定  

### 锁定机制是否提供账户恢复或过期的路径？
- [ ] 是 — 账户在设定的时长后自动解锁，或通过邮件验证解锁  
- [ ] 是 — 账户需要管理员干预才能解锁  
- [ ] 否 — 锁定是永久性的，且没有明确的恢复路径，增加了拒绝服务 (DoS) 的风险  

### 应用程序在锁定期间是否区分有效和无效的用户名？
- [ ] 否 — 应用程序对现有用户和不存在用户的响应是相同的  
- [ ] 是 — 应用程序会泄露账户是否已被锁定，从而允许用户名枚举 (Username Enumeration)  

### 锁定机制是否容易受到拒绝服务 (DoS) 攻击？
- [ ] 否 — 验证码 (CAPTCHA) 或基于 IP 的速率限制等控制措施可防止大规模账户锁定  
- [ ] 是 — 攻击者可以通过提供错误的密码，系统地锁定所有已知用户

---

## WSTG-ATHN-04 — 绕过身份验证机制测试

绕过身份验证机制 (Bypassing Authentication Schema) 涉及识别和利用应用程序逻辑中的缺陷，这些缺陷允许攻击者在不提供有效凭证 (Credentials) 的情况下访问受保护的资源。这种漏洞通常发生在应用程序依赖客户端控制 (Client-side controls)、未能对每个请求强制执行服务端授权 (Server-side authorization)，或者在生产环境中暴露了管理“后门” (Backdoors) 和调试端点 (Debug endpoints) 的情况下。攻击者通过对敏感 URL 进行强制浏览 (Forced browsing)、操纵诸如 `authenticated=true` 之类的 HTTP 参数 (HTTP parameters) 或伪造请求头 (Spoofing headers) 来诱导应用程序认为会话 (Session) 已经建立，从而利用这些弱点。成功的利用会导致身份验证周界的全面瓦解，可能导致未经授权的数据外泄 (Data exfiltration) 或整个系统受损。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **测试状态** | 未执行 |
| **严重程度** | 严重 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**工具：** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### 是否可以在没有活动会话的情况下直接访问敏感内部页面？
- [ ] 否 — 直接访问会导致重定向至登录页面或返回 403/404 响应  
- [ ] 是 — 某些敏感页面可以访问，但提供的数据有限或不敏感  
- [ ] 是 — 敏感的管理页面或用户特定页面在没有身份验证的情况下**完全**可以访问 *(严重)*  

### 应用程序是否依赖客户端标志或参数来确定身份验证状态？
- [ ] 否 — 身份验证状态严格通过服务端会话状态进行管理  
- [ ] 是 — 存在客户端标志，但修改标志**不会**授予对受保护区域的访问权限  
- [ ] 是 — 修改参数（例如 `is_authenticated=true`, `user_role=admin`）**允许**绕过身份验证  

### 是否可以通过伪造或篡改特定的 HTTP 请求头来绕过身份验证？
- [ ] 否 — 诸如 `X-Forwarded-For`、`Referer` 或自定义请求头对身份验证没有影响  
- [ ] 是 — 篡改请求头（例如 `X-Custom-IP-Authorization`, `X-Remote-User`）**是可能的**，并能授予未经授权的访问  

### 是否存在规避标准登录流程的其他路径或开发工件 (Development Artifacts)？
- [ ] 否 — 所有受保护资源都由集中的、生产就绪的身份验证检查进行管控  
- [ ] 是 — 遗留端点、调试路由（例如 `/debug/login`）或遗忘的 API 版本在没有凭证的情况下**可以**访问  

### 应用程序是否未能针对高权限操作进行重新验证或校验会话？
- [ ] 否 — 高权限或敏感操作需要新的或有效的会话令牌  
- [ ] 是 — 一旦在某个端点发现绕过漏洞，就**可以**利用它在整个应用程序中执行管理操作

---

## WSTG-ATHN-05 — 脆弱的“记住密码”功能测试

“记住我” (Remember Me) 或持久化登录 (Persistent Login) 功能旨在通过在客户端 (Client Side) 存储令牌或凭据，在浏览器重启后保持用户的身份验证状态 (Authenticated State)。如果该功能实现不当——例如在 Cookie 中存储明文密码 (Plaintext Passwords)、弱哈希 (Weak Hashes) 或低熵令牌 (Low-entropy Tokens)——则会使用户面临通过跨站脚本攻击 (Cross-Site Scripting, XSS)、物理访问或网络拦截导致的账户接管 (Account Takeover) 风险。渗透测试人员 (Pentesters) 需评估这些持久化令牌的熵 (Entropy)、应用于存储机制的安全标志 (Security Flags) 以及会话 (Session) 的生命周期，以确保持久化访问的便利性不会绕过关键的安全控制措施，如多因素身份验证 (Multi-factor Authentication, MFA) 或会话失效 (Session Invalidation) 机制。攻击者通常通过收集这些长期有效的令牌，在无需原始凭据 (Credentials) 的情况下获取账户的未经授权访问权限。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果在持久化 Cookie 中存储了明文凭据或未加盐的哈希，或者该令牌绕过了多因素身份验证 (MFA)，则严重程度变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**工具：** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### 应用程序是否实现了“记住我”或“保持登录”功能？
- [ ] 否 — 应用程序中**不存在**此功能  
- [ ] 是 — 存在此功能，并使用加密强度高的不可预测令牌  
- [ ] 是 — 存在此功能，但使用可预测或低熵令牌 *(中)*  
- [ ] 是 — 存在此功能，且存储了明文凭据或 Base64 编码的密码 *(高)*  

### 持久化 Cookie 是否正确应用了安全标志？
- [ ] 是 — 已**应用** `HttpOnly`、`Secure` 和 `SameSite` 标志  
- [ ] 是 — 已应用部分标志，但**缺失** `HttpOnly`  
- [ ] 是 — 已应用部分标志，但在 HTTPS 环境下**缺失** `Secure`  
- [ ] 否 — 持久化 Cookie 未**应用**任何安全标志  

### 在手动注销后，“记住我”令牌是否仍然有效？
- [ ] 否 — 注销后，令牌立即在服务端失效  
- [ ] 是 — 令牌仍然有效，但执行敏感操作时需要密码  
- [ ] 是 — 令牌仍然有效，且允许在**无需**重新身份验证的情况下获得完整的账户访问权限 *(中)*  

### 修改密码后，持久化会话是否终止？
- [ ] 是 — 用户更改密码时，所有持久化令牌均被撤销  
- [ ] 否 — 在**执行**密码重置/更改后，“记住我”令牌仍然有效 *(高)*  

### 在后续访问中，持久化令牌是否绕过了多因素身份验证 (MFA)？
- [ ] 否 — 即使存在“记住我”令牌，仍需要 MFA  
- [ ] 是 — MFA 被绕过，但令牌与特定的设备指纹 (Device Fingerprint) 绑定  
- [ ] 是 — 仅通过任何设备上的持久化 Cookie 即可**完全绕过** MFA *(高)*

---

## WSTG-ATHN-06 — 浏览器缓存弱点测试 (Testing for Browser Cache Weakness)

浏览器缓存弱点（Browser cache weakness）发生在敏感信息被存储在本地浏览器缓存中，且可被能够访问同一台物理机器的未经授权用户检索时。由于缺乏适当的缓存控制响应头 (Cache control headers)，导致潜在的敏感数据（如个人信息、账户详情或会话标识符）在用户登出或关闭会话后仍然存在。攻击者或共享/公共终端的后续用户可以通过浏览浏览器历史记录、使用“后退”按钮或检查本地缓存文件来窃取 (Exfiltrate) 缓存的响应。确保实施严格的指令，如 `Cache-Control: no-store` 和 `Pragma: no-cache`，对于任何处理经过身份验证的内容的应用程序至关重要，以确保敏感数据绝不会被写入磁盘。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中* |

> *如果应用程序经常从共享/公共自助服务机 (Kiosks) 访问，或者包含高度敏感的个人可识别信息 (PII) / 受保护健康信息 (PHI) 数据，则严重程度变为“中”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**工具：** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### 在经过身份验证或敏感的页面上是否存在适当的缓存控制响应头？
- [ ] 是 — `Cache-Control: no-store, no-cache` 和 `Pragma: no-cache` **存在**且**配置正确**  
- [ ] 是 — 存在部分响应头，但**缺失** `no-store`，允许磁盘缓存  
- [ ] 否 — 未**应用**任何缓存相关的响应头，依赖浏览器默认行为  

### 应用程序是否对用户特定数据使用 `private` 指令？
- [ ] 是 — 为所有个性化内容**启用**了 `Cache-Control: private`，以防止代理缓存 (Proxy Caching)  
- [ ] 否 — 内容被标记为 `public` 或缺少 `private` 指令，使其可被中间代理**缓存**  

### 成功登出后，是否可以通过浏览器“后退”按钮访问敏感信息？
- [ ] 否 — 浏览器提示重新身份验证，或者页面**未从**本地缓存加载  
- [ ] 是 — 在会话终止后，敏感信息通过后退按钮仍然**可见**  

### 敏感的非 HTML 文件（例如 PDF、CSV 导出、JSON API 响应）是否受到缓存保护？
- [ ] 否 — 应用程序不生成或处理敏感的非 HTML 文件  
- [ ] 是 — 对所有敏感文件下载和 API 终端节点均**应用**了严格的缓存控制响应头  
- [ ] 否 — 敏感下载或 API 响应被**存储**在本地浏览器缓存或临时目录中  

### 应用程序是否使用 `Expires: 0` 或过去的时间日期来防止使用过期数据？
- [ ] 是 — `Expires` 响应头被设置为 `0` 或历史日期，**确保**浏览器将内容视为立即过期  
- [ ] 否 — 敏感页面的 `Expires` 响应头**缺失**或被设置为未来日期

---

## WSTG-ATHN-07 — 弱密码策略测试 (Testing for Weak Password Policy)

弱密码策略 (Weak Password Policy) 测试涉及评估应用程序在账户创建和凭证更新期间强制执行的复杂度、长度和熵 (Entropy) 要求。不充分的策略会使账户容易受到自动化字典攻击 (Dictionary Attacks)、暴力破解 (Brute-force) 尝试和凭证填充 (Credential Stuffing) 的影响，从而直接威胁到用户账户的机密性。这些漏洞通常存在于注册端点、密码重置工作流和管理员用户管理面板中。从攻击者的角度来看，宽松的策略显著降低了使用常用单词列表或基于模式的破解方式来成功获取凭证所需的计算成本和时间。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 低* |

> *如果应用程序缺乏账户锁定 (Account Lockout) 或速率限制 (Rate Limiting) 机制来防止自动化猜解，则严重程度变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**工具：** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### 应用程序是否强制执行最小密码长度？
- [ ] 是 — 最小长度为 12 个字符以上 *(最安全)*  
- [ ] 是 — 最小长度在 8 到 11 个字符之间  
- [ ] 是 — 最小长度**少于** 8 个字符  
- [ ] 否 — 未强制执行最小长度限制  

### 是否强制执行复杂度要求（大写字母、小写字母、数字、符号）？
- [ ] 是 — 必须包含多种字符类型，且**无法绕过**  
- [ ] 是 — 建议使用多种字符类型，但**未**强制执行  
- [ ] 否 — 未应用任何复杂度要求  

### 应用程序是否封禁通用/弱密码或包含用户特定数据的密码？
- [ ] 是 — **无法**使用已知的弱密码（例如 "password123"）和基于用户名的密码  
- [ ] 是 — 通用密码被封禁，但**可以**使用用户特定数据（如用户名、电子邮件）  
- [ ] 否 — 接受任何符合长度/复杂度要求的字符串  

### 是否可以通过客户端修改绕过复杂度或长度要求？
- [ ] 否 — 策略在**服务端 (Server-side)** 严格执行  
- [ ] 是 — 策略通过 JavaScript 执行，且**可以**通过拦截请求绕过  

### 密码策略是否在不泄露实现细节的情况下清晰地告知用户？
- [ ] 是 — 策略在输入期间可见，并提供通用的失败消息  
- [ ] 是 — 策略可见，但失败消息泄露了特定的正则表达式 (Regex) 或逻辑缺陷  
- [ ] 否 — 策略**不可见**，迫使用户通过反复试验来发现

---

## WSTG-ATHN-08 — 测试弱安全问题答案 (Testing for Weak Security Question Answer)

测试弱安全问题答案涉及评估用于验证用户身份的基于知识的身份验证 (Knowledge-based authentication, KBA) 机制，通常在密码恢复或多因素身份验证 (Multi-factor authentication, MFA) 流程中使用。这一点至关重要，因为安全问题通常依赖于静态、非机密的信息，这些信息很容易通过开源情报 (Open-source intelligence, OSINT) 或社会工程学 (Social engineering) 获取，从而导致未经授权的账户接管 (Account takeover)。这些漏洞通常出现在“忘记密码”或“账户解锁”模块中，用户在这些模块中提供预设问题的答案。从攻击者的角度来看，这是自动化暴力破解 (Brute-forcing) 或针对性研究的主要目标，因为许多用户对常见问题（如“您母亲的婚前姓氏是什么？”或“您就读的高中是哪所？”）提供的答案是可预测的或可公开验证的。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **测试状态** | 未执行 |
| **严重性** | 中 / 高* |

> *如果安全问题是在没有进一步验证的情况下进行完整密码重置和账户接管的唯一要求，则严重性变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**工具：** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### 应用程序是否实施了用于身份验证或密码恢复的安全问题？
- [ ] 否 — 安全问题**未被用于**身份验证或恢复  
- [ ] 是 — 安全问题作为可选的二次因子**被使用**  
- [ ] 是 — 安全问题被作为主要/唯一的恢复机制**使用** *(高风险)*  

### 安全问题是从固定列表中选择的还是用户自定义的？
- [ ] 否 — 问题完全由用户自定义并要求高熵  
- [ ] 是 — 问题是固定的，但包括非 OSINT 可发现的选项  
- [ ] 是 — 问题来自常见的、OSINT 可发现的主题固定列表 *(存在漏洞)*  

### 是否对安全问题答案尝试应用了速率限制或账户锁定？
- [ ] 是 — **已应用**严格的速率限制 (Rate Limiting) 和账户锁定 (Account Lockout) 以防止暴力破解  
- [ ] 是 — **已应用**速率限制，但**可能**通过 IP 轮换或标头操纵绕过  
- [ ] 否 — 答案尝试**未实施速率限制**  

### 应用程序是否对答案内容强制执行复杂度或校验？
- [ ] 是 — 校验机制可防止常用词汇并确保答案的复杂度/唯一性  
- [ ] 是 — 存在基础校验，但**可以**通过通用词表或字典攻击 (Dictionary attacks) 绕过  
- [ ] 否 — **未进行校验**；**允许**简单或单字符答案  

### 是否可以通过逻辑缺陷绕过安全问题挑战？
- [ ] 否 — 恢复流程严格要求有效的答案才能继续  
- [ ] 是 — 恢复流程**可以**通过操纵 HTTP 响应代码或会话参数绕过  
- [ ] 是 — 答案在页面的源代码或隐藏字段中反映

---

## WSTG-ATHN-09 — 测试弱密码修改或重置功能

密码修改和重置机制是身份验证管理的关键组件，如果实现不当，攻击者将能够接管用户账户。这些漏洞通常涉及可预测的重置令牌 (Token)、在暴力破解 (Brute Force) 尝试期间缺乏账户锁定机制、令牌传输不安全，或者在未验证当前密码的情况下允许修改密码。攻击者通过拦截或猜测恢复链接、利用主机头注入 (Host Header Injection) 重定向重置邮件，或利用会话固定 (Session Fixation) 在密码修改后维持访问权限来利用这些缺陷。确保这些过程需要严格的验证并使用加密随机性，对于维护账户完整性和防止未经授权的访问至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 紧急 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**工具：** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### 在标准修改过程中，设置新密码是否需要当前密码？
- [ ] 是 — **需要**并验证当前密码  
- [ ] 是 — **需要**当前密码，但可以通过参数污染 (Parameter Pollution) 或篡改绕过  
- [ ] 否 — **不要求**输入当前密码，允许通过会话劫持 (Session Hijacking) 接管账户 *(高)*  

### 密码重置令牌是否具有加密安全性且不可预测？
- [ ] 是 — 令牌具有高熵、随机性且**不可预测**  
- [ ] 是 — 生成了令牌，但显示出低熵或可预测的模式（例如基于时间戳）  
- [ ] 否 — 令牌**可预测**或基于静态用户数据，如 Base64 编码的电子邮件/ID *(紧急)*  

### 密码重置链接是否可以通过主机头注入被重定向？
- [ ] 否 — 应用程序在生成链接时忽略或验证了 `Host` 头  
- [ ] 是 — 应用程序使用 `Host` 或 `X-Forwarded-Host` 头构建链接，但验证**存在**  
- [ ] 是 — 链接**可以**通过请求头篡改重定向到攻击者控制的域名 *(高)*  

### 重置令牌是否具有有限的生命周期并能立即失效？
- [ ] 是 — 令牌很快过期，并在使用或密码更改后**立即**失效  
- [ ] 是 — 令牌在较长时间后过期（例如 24 小时以上）或允许**多次使用**  
- [ ] 否 — 令牌**永不过期**，或在密码成功更改后仍然有效  

### 密码重置和修改端点是否存在速率限制或锁定机制？
- [ ] 是 — **应用了**严格的速率限制 (Rate Limiting) 和验证码 (CAPTCHA)，以防止枚举和暴力破解  
- [ ] 是 — 存在有限的控制，但可以通过 IP 轮换或请求头欺骗进行绕过  
- [ ] 否 — **未应用**速率限制，允许进行批量账户枚举或令牌暴力破解

---

## WSTG-ATHN-10 — 测试备选渠道中的弱身份验证 (Testing for Weaker Authentication in Alternative Channel)

测试备选渠道中的弱身份验证涉及识别和分析账户访问的辅助路径——例如移动 API (API)、密码恢复工作流、帮助台接口或遗留子域名——这些路径可能未执行与主 Web 门户相同的安全严谨性。这些备选渠道通常缺乏健壮的多因素身份验证 (Multi-factor authentication, MFA)、严格的速率限制 (Rate Limiting) 或复杂的密码要求，从而形成了一个破坏整个身份验证系统的“最薄弱环节”。攻击者专门针对这些被忽视的入口点进行撞库攻击 (Credential Stuffing)，或通过利用不同接口在处理身份验证时的差异来绕过 (Bypass) MFA。确保所有身份验证表面的一致性对于防止未经授权的访问并维护用户会话和数据的完整性至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### 是否存在并识别了备选身份验证渠道？
- [ ] 否 — 仅存在单一身份验证渠道  
- [ ] 是 — 识别出多个渠道（例如：移动应用 API、桌面客户端、遗留门户、SOAP 服务）  

### 备选渠道是否执行与主渠道等同的安全策略？
- [ ] 是 — 所有渠道均执行**完全相同**的密码复杂度和 MFA 要求  
- [ ] 是 — 渠道执行了密码复杂度要求，但 MFA **不是必需的**或**可以**被绕过  
- [ ] 否 — 备选渠道的身份验证要求**显著薄弱**  

### 速率限制和账户锁定在所有渠道中是否保持一致？
- [ ] 是 — 速率限制和锁定在所有端点上均**一致执行**  
- [ ] 是 — 存在控制措施，但在特定渠道（如移动 API）上**可能实现**绕过  
- [ ] 否 — 速率限制或锁定**未应用于**辅助身份验证路径  

### 是否可以通过切换到备选渠道绕过 MFA？
- [ ] 否 — MFA 是强制性的，无论入口点如何都**无法**被绕过  
- [ ] 是 — Web 门户要求 MFA，但移动或遗留 API **未强制执行**  

### 密码恢复或“忘记密码”流程是否比标准登录更薄弱？
- [ ] 否 — 恢复流程使用强力的带外验证 (Out-of-band verification) 且不泄露信息  
- [ ] 是 — 恢复流程使用弱安全问题，这些问题**可以**被轻易猜测或研究出来  
- [ ] 是 — 恢复流程易受枚举 (Enumeration) 攻击，或者**不需要**与标准登录相同级别的证明

---

## WSTG-ATHN-11 — 多因素身份验证 (MFA) 测试

多因素身份验证 (Multi-Factor Authentication, MFA) 测试用于评估第二层安全屏障的稳固性，该屏障旨在即便在主要凭证泄露的情况下也能防止未经授权的访问。攻击者经常尝试通过识别未强制执行 MFA 的端点 (Endpoints) 来绕过它，例如旧版 API 版本、移动端后端或密码重置工作流。利用手段通常涉及篡改服务器响应、暴力破解 (Brute Force) 短效代码 (OTP)，或利用竞态条件 (Race Conditions) 和会话管理缺陷，使用户在未提供第二个因素的情况下转换为已认证状态。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 紧急 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### MFA 是否在所有身份验证门户中一致强制执行？
- [ ] 是 — 所有 Web、移动端和基于 API (API) 的登录尝试均要求 MFA  
- [ ] 是 — 但在特定端点未强制执行 MFA（例如：旧版 `/api/v1/login` 或特定移动端门户）  
- [ ] 否 — 应用程序中**未实现** MFA *(紧急)*  

### 是否可以通过直接 URL 导航或响应篡改绕过 MFA 验证步骤？
- [ ] 否 — 直接导航或参数篡改**无法**绕过挑战  
- [ ] 是 — **可以**在未完成 MFA 挑战的情况下直接导航到内部仪表板 URL  
- [ ] 是 — **可以**通过篡改服务器响应（例如：将 `401 Unauthorized` 更改为 `200 OK`）来获取访问权限  

### MFA 代码验证过程是否受到保护以防御暴力破解攻击？
- [ ] 是 — 在多次 OTP 尝试失败后，**应用了**严格的速率限制 (Rate Limiting) 或账户锁定  
- [ ] 是 — 存在速率限制，但**可以**通过 IP 轮换或请求头篡改绕过  
- [ ] 否 — 未强制执行速率限制，且**可以**对代码进行自动暴力破解 (Brute Force)  

### 应用程序在 MFA 转换期间是否维持安全的会话状态？
- [ ] 是 — 高权限会话令牌 (Session Tokens) **仅在**成功完成 MFA 后发放  
- [ ] 否 — 在完成 MFA **之前**即发放了功能齐全的会话 Cookie (Session Cookie)，允许访问某些功能  
- [ ] 否 — 应用程序使用静态标识符或可预测的会话转换，且**可以**被劫持  

### 备选 MFA 因素（短信、电子邮件、备份代码）是否存在可被利用的漏洞？
- [ ] 否 — 所有因素均使用安全、不可预测且有时限的值  
- [ ] 是 — 备份代码是可预测的或**可以**被枚举  
- [ ] 是 — 可以滥用“重新发送代码”功能执行短信/电子邮件轰炸，或泄露部分联系方式

---

## WSTG-ATHZ-01 — 目录遍历与文件包含测试 (Testing Directory Traversal File Include)

当应用程序使用用户可控的输入来构建文件或目录路径，且未进行充分的验证或过滤 (Sanitization) 时，就会产生目录遍历 (Directory Traversal) 和文件包含 (File Inclusion) 漏洞。攻击者通过注入诸如 `../` 之类的序列来利用这些缺陷，从而跳出预定目录，可能访问到敏感的系统文件、配置数据或应用程序源代码。在涉及本地文件包含 (Local File Inclusion, LFI) 或远程文件包含 (Remote File Inclusion, RFI) 的更严重情况下，攻击者可以通过包含恶意脚本或利用日志投毒 (Log Poisoning) 和 PHP 封装器 (PHP Wrappers) 来实现远程代码执行 (Remote Code Execution, RCE)。这些漏洞通常存在于用于动态内容加载、模板引擎 (Template Engines) 或图像获取端点 (Endpoints) 的参数中，这些端点的服务端逻辑负责处理文件系统路径。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**工具：** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### 参数是否接收文件名或路径用于服务端处理？
- [ ] 否 — 似乎没有参数与文件系统交互  
- [ ] 是 — 存在参数，但使用了严格的文件标识符白名单 (Allowlist)  
- [ ] 是 — 参数接收直接的文件名或路径  

### 输入是否经过过滤以防止目录遍历序列？
- [ ] 是 — 输入已根据严格的白名单进行验证，遍历序列**不可行**  
- [ ] 是 — 输入通过删除 `../` 序列进行了过滤，但**可能**存在递归绕过 (Recursive Bypass)  
- [ ] 否 — 路径相关的输入**未应用**任何过滤或验证  

### 是否可以通过遍历序列访问受限目录之外的文件？
- [ ] 否 — 应用程序或操作系统阻止了对定义范围之外文件的访问  
- [ ] 是 — **可以**访问 Web 根目录 (Web Root) 内的文件  
- [ ] 是 — **可以**访问敏感系统文件（例如：`/etc/passwd`，`C:\Windows\win.ini`）*（严重）*  

### 服务器是否允许包含远程 URL (RFI)？
- [ ] 否 — 远程文件包含在服务器/应用程序级别已**禁用**  
- [ ] 是 — **可以**包含远程文件，但执行**不可行**  
- [ ] 是 — **可以**在服务器上包含并执行远程文件*（严重）*  

### 是否可以使用编码或特殊字符绕过过滤器？
- [ ] 否 — 过滤器能有效处理各种编码和空字节 (Null Bytes)  
- [ ] 是 — **可能**通过 URL 编码、双重 URL 编码或 16 位 Unicode 实现绕过  
- [ ] 是 — **可能**通过空字节注入 (Null Byte Injection, `%00`) 或文件系统封装器（例如：`php://filter`）实现绕过

---

## WSTG-ATHZ-02 — 绕过授权方案测试 (Testing for Bypassing Authorization Schema)

绕过授权方案 (Bypassing Authorization Schema) 发生在应用程序未能强制执行访问控制 (Access Controls) 时，导致攻击者能够访问其预期权限之外的资源或功能。这种漏洞通常表现为水平越权 (Horizontal Privilege Escalation)，即攻击者访问属于同级别其他用户的数据；或者垂直越权 (Vertical Privilege Escalation)，即低权限用户获得了管理员权限。攻击者通过操纵请求参数（如资源 ID）、修改会话令牌 (Session Tokens) 或利用 `X-Original-URL` 等 HTTP 标头 (HTTP Headers) 来绕过基于路径的限制，从而利用这些缺陷。确保健全的授权机制对于维护数据机密性以及防止可能危及整个系统的未经授权的管理操作至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 高 (High) / 严重 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**工具：** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### 同一角色的用户之间是否防止了水平越权？
- [ ] 是 — 授权检查**已应用于**每个资源请求，且**无法绕过**  
- [ ] 是 — 授权检查**已应用于**，但可以通过不安全的直接对象引用 (IDOR) 或参数篡改 (Parameter Tampering) 绕过  
- [ ] 否 — 用户只需更改资源 ID **即可**访问属于其他用户的数据 *(高)*  

### 低权限用户是否防止了垂直越权？
- [ ] 否 — 应用程序只有一个权限级别  
- [ ] 是 — 低权限用户**无法**访问管理功能  
- [ ] 是 — 管理功能在 UI 中已隐藏，但通过直接 URL **可以**访问  
- [ ] 否 — 低权限用户通过操纵角色或权限**可以**执行管理操作 *(严重)*  

### 是否可以使用 HTTP 谓词篡改来绕过授权？
- [ ] 否 — 无论使用何种 HTTP 方法，都会强制执行访问控制  
- [ ] 是 — 更改方法（例如从 `GET` 更改为 `POST` 或 `PUT`）**会绕过**授权检查  

### 管理端点或受限端点是否可以在没有任何身份验证的情况下访问？
- [ ] 否 — 所有敏感端点都需要有效的会话和适当的权限  
- [ ] 是 — 如果攻击者知道直接 URL，则可以访问某些管理端点  
- [ ] 是 — **可以**未经身份验证 (Unauthenticated) 访问敏感功能 *(严重)*  

### HTTP 标头是否允许规避基于路径的授权？
- [ ] 否 — 应用程序**不容易**受到基于标头的绕过攻击  
- [ ] 是 — `X-Original-URL`、`X-Rewrite-URL` 或 `X-Forwarded-For` 等标头**可被用于**绕过访问控制

---

## WSTG-ATHZ-03 — 权限提升测试 (Testing for Privilege Escalation)

权限提升 (Privilege Escalation) 发生在攻击者利用漏洞获取本应属于更高权限级别或不同身份用户的资源或功能访问权时。在垂直提权 (Vertical Escalation) 中，普通用户尝试访问管理功能；而水平提权 (Horizontal Escalation) 则涉及访问属于具有相同权限级别的另一用户的数据。这些缺陷通常表现在实现不当的访问控制列表 (ACLs)、不安全直接对象引用 (IDOR) 或会话 (Session) 或身份令牌中的参数篡改 (Parameter Tampering) 中。从攻击者的角度来看，这通常是通过操纵数字 ID、修改 Cookie 或 JWTs 中基于角色的参数，或直接调用缺乏服务端授权检查 (Authorization Checks) 的隐藏 API 端点来实现的。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**工具：** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### 应用程序是否实现了多个权限级别或多租户 (Multi-tenancy)？
- [ ] 否 — 应用程序为单用户或单角色系统  
- [ ] 是 — 已定义多个角色或租户，**需要**进行授权测试  

### 是否可能发生水平提权（同级访问）？
- [ ] 否 — 授权检查**已应用**于所有资源标识符和会话所有者  
- [ ] 是 — **可以**通过 IDOR 或参数篡改访问其他用户的数据  
- [ ] 是 — **可能**发生数据泄露，但**无法**修改其他用户的数据  

### 是否可能发生垂直提权（低权限到高权限）？
- [ ] 否 — 管理端点严格执行基于角色的访问控制 (RBAC)  
- [ ] 是 — 低权限用户**可以**通过直接 URL 访问方式访问管理功能  
- [ ] 是 — **可以**通过操纵角色相关的 Header、Cookie 或 JWT Claims 访问管理功能  

### 是否可以通过批量赋值 (Mass Assignment) 或参数污染 (Parameter Pollution) 修改用户角色或权限？
- [ ] 否 — 基于角色的字段严格只读，用户**无法**修改  
- [ ] 是 — 在个人资料更新或注册时，通过包含隐藏参数（例如 `role=admin`）**可以**提升用户权限  

### 授权检查是否在所有 API 版本和 HTTP 方法中一致强制执行？
- [ ] 是 — 控制措施在所有版本和方法中**一致应用**  
- [ ] 否 — 旧版本 API（例如 `/v1/`）或特定 HTTP 方法（例如 `PUT`, `DELETE`, `PATCH`）**绕过了**授权  
- [ ] 否 — 管理功能在 UI 中被隐藏，但在 API 级别对所有用户**开放**

---

## WSTG-ATHZ-04 — 不安全的直接对象引用测试 (Testing for Insecure Direct Object References)

不安全的直接对象引用 (Insecure Direct Object References, IDOR) 发生在应用程序根据用户提供的输入直接访问对象，而未执行授权检查以验证请求者的权限时。此漏洞严重影响机密性和完整性，因为它允许攻击者查看、修改或删除属于其他用户或系统的数据。它通常表现在指向内部数据库键、文件名或账户标识符的 URL 参数、POST 正文数据或 JSON 键中。从攻击者的角度来看，利用 (Exploit) 涉及操纵这些标识符——通常通过递增整数或对 UUID 进行模糊测试 (Fuzzing)——以访问其授权范围之外的资源。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 高 (High) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**工具：** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### 应用程序是否在请求参数中使用直接对象标识符？
- [ ] 否 — 应用程序使用间接引用或加密映射 *(最安全)*  
- [ ] 是 — 应用程序使用不可预测的标识符（例如：长 UUID 或哈希值）  
- [ ] 是 — 应用程序使用可预测的标识符（例如：递增整数或用户名）  

### 对于每个涉及对象标识符的请求，是否都执行了服务器端授权？
- [ ] 是 — 服务器端检查**无法**针对任何标识符被绕过  
- [ ] 是 — 存在服务器端检查，但**可以**通过参数污染 (Parameter Pollution) 或标头篡改绕过  
- [ ] 否 — **未应用**任何授权检查来验证所请求对象的所有权  

### 攻击者是否可以访问或修改属于其他用户的对象（水平越权 / Horizontal IDOR）？
- [ ] 否 — **无法**访问其他用户的数据  
- [ ] 是 — **可以**查看其他用户的数据，但**无法**进行修改  
- [ ] 是 — **可以**同时查看和修改其他用户的数据 *(危急)*  

### 是否可以访问管理级或系统级对象（垂直越权 / Vertical IDOR）？
- [ ] 否 — **无法**访问更高权限的对象  
- [ ] 是 — **可以**访问管理配置或系统文件  

### 应用程序是否通过搜索、元数据或其他端点泄露对象标识符？
- [ ] 否 — 标识符**未被**意外泄露  
- [ ] 是 — **可以**通过二级端点 (API) 或公开个人资料枚举其他用户/对象的标识符

---

## WSTG-ATHZ-05 — OAuth 弱点测试 (Testing for OAuth Weaknesses)

OAuth 弱点涵盖了在实现 OAuth 2.0 或 OpenID Connect (OIDC) 协议时的各种缺陷，通常会导致完整的账户接管 (Account Takeover) 或未经授权的数据访问。这些漏洞通常表现在授权流 (Authorization Flow) 过程中，特别是涉及 `redirect_uri` 的验证、`state` 参数的熵 (Entropy) 及其验证，以及访问令牌 (Access Token) 或 ID 令牌 (ID Token) 的安全处理。攻击者通过拦截授权码 (Authorization Code)、通过开放重定向 (Open Redirect) 将令牌重定向到攻击者控制的域名，或利用跨站请求伪造 (CSRF) 将受害者的会话链接到攻击者的账户来利用这些弱点。由于 OAuth 通常作为主要的身份验证机制，单一的错误配置就可能损害整个用户身份系统的完整性。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 紧急 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**工具：** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### `state` 参数是否已实现并经过验证以防止 CSRF？
- [ ] 是 — `state` 是强制性的，每个会话唯一，并在服务器端经过严格验证  
- [ ] 是 — 存在 `state` 但在不同会话之间是静态的或可预测的  
- [ ] 是 — 存在 `state` 但应用程序在回调时**未**对其进行验证  
- [ ] 否 — 授权请求中**未使用** `state` 参数 *(紧急)*  

### `redirect_uri` 是否针对白名单 (Whitelist) 进行了严格验证？
- [ ] 是 — 仅接受与预注册白名单完全匹配的项  
- [ ] 是 — 应用了验证，但**可以通过**路径遍历 (Path Traversal) 或子域名操纵绕过  
- [ ] 是 — 应用了验证，但**可以通过**参数污染 (Parameter Pollution) 或片段注入 (Fragment Injection) 绕过  
- [ ] 否 — 应用程序**允许**在 `redirect_uri` 参数中使用任意 URL *(紧急)*  

### `response_type` 是否可以被操纵以改变流程？
- [ ] 否 — 应用程序仅接受预期的流（例如 `code`）并拒绝其他流  
- [ ] 是 — 从 `code` 切换到 `token`（隐式流/Implicit Flow）**是可能的**，并且会导致令牌在 URL 中泄露  
- [ ] 是 — 混合流 (Hybrid Flows) 或未经授权的 `response_type` 值被**启用**并处理  

### 访问令牌或授权码是否通过 Referer 标头或浏览器历史记录泄露？
- [ ] 否 — 令牌/授权码被安全处理，且敏感页面使用了适当的 `Referrer-Policy`  
- [ ] 是 — 令牌/授权码通过 `Referer` 标头泄露到第三方域名  
- [ ] 是 — 由于不安全的缓存或 GET 请求，令牌/授权码在浏览器历史记录中**可见**  

### 应用程序是否针对 OAuth 范围 (Scope) 执行“最小权限原则 (Least Privilege Principle)”？
- [ ] 是 — 应用程序仅请求功能所需的最小范围  
- [ ] 否 — 应用程序默认请求过度的或管理权限范围  
- [ ] 否 — 用户在同意阶段**无法**审查或选择退出特定的范围

---

## WSTG-SESS-01 — 会话管理方案测试 (Testing for Session Management Schema)

会话管理方案 (Session Management Schema) 测试侧重于分析应用程序如何处理会话标识符 (Session Identifier) 的生命周期和结构，以确保它们能够抵御预测和劫持。攻击者通过捕获多个会话令牌 (Session Token) 进行统计分析，寻找模式、低熵 (Low Entropy) 或可预测的增量，从而允许他们为其他用户伪造有效的会话。该方案中的弱点通常表现为会话固定 (Session Fixation) 漏洞或使用随机性不足的值，这些值通常出现在 HTTP Cookie、URL 参数 (URL Parameters) 或自定义标头实现中。破坏会话方案是一种高影响力的攻击，它使攻击者无需获取受害者的主要凭据即可完全访问其已认证状态。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 高 (High) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**工具：** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### 会话标识符是否足够复杂且随机？
- [ ] 是 — 令牌通过了统计随机性测试（高熵）且**无法**绕过  
- [ ] 是 — 令牌很长，但包含可预测的模式或静态部分  
- [ ] 否 — 令牌是顺序生成的、基于可预测的时间戳，或者具有**极低**的熵 *(严重)*  

### 会话标识符在何处传输和存储？
- [ ] 标识符存储在带有 `Secure` 和 `HttpOnly` 标志的 Cookie 中 *(最安全)*  
- [ ] 标识符存储在 Cookie 中，但缺少 `Secure` 或 `HttpOnly` 标志  
- [ ] 标识符**仅**通过 URL 参数或 `GET` 请求传输 *(高风险)*  

### 应用程序在身份验证成功后是否签发新的会话标识符？
- [ ] 是 — 登录后立即**生成**一个新的、唯一的会话 ID  
- [ ] 否 — 登录前后会话 ID 保持不变 *(可能存在会话固定攻击)*  

### 会话标识符是否与特定的 IP 地址或 User-Agent 绑定以防止重用？
- [ ] 是 — 如果客户端指纹发生显著变化，会话将失效  
- [ ] 否 — 会话**可以**在不同的 IP 地址或设备上重用，而无需额外验证  

### 退出登录或超时后，会话标识符是否在服务端正确失效？
- [ ] 是 — 退出登录后，服务端会话状态被**完全销毁**  
- [ ] 否 — 即使客户端 Cookie 已删除，会话在服务器上仍然有效  
- [ ] 否 — 会话**永不过期**或超时期限过长

---

## WSTG-SESS-02 — Cookie 属性测试

会话管理 (Session Management) 通常依赖 HTTP Cookie 来保持状态，这使得这些 Cookie 的安全属性成为攻击者的主要目标。通过省略或错误配置诸如 `HttpOnly`、`Secure` 和 `SameSite` 等属性，应用程序会使会话令牌暴露于通过跨站脚本攻击 (Cross-Site Scripting, XSS) 被窃取、在未加密通道中被拦截或跨站请求伪造 (Cross-Site Request Forgery, CSRF) 攻击的风险中。渗透测试人员通过检查这些标志来确定攻击者是否可以从浏览器的文档对象模型 (Document Object Model, DOM) 中提取会话标识符，或强制浏览器在跨源请求中发送这些标识符。正确的属性配置是一项基础的深度防御 (Defense-in-Depth) 措施，旨在确保敏感令牌仅限于授权的、安全的上下文中。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **测试状态** | 未执行 |
| **严重程度** | 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**工具：** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Cookie 标志实施分析
| 属性 | 存在 | 缺失 |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### `HttpOnly` 标志是否应用于敏感会话 Cookie？
- [ ] 是 — 已应用于**所有**敏感会话 Cookie *（最安全）*  
- [ ] 是 — 仅应用于部分会话 Cookie  
- [ ] 否 — **未应用**该标志，允许通过客户端脚本进行访问 *（高危）*  

### 对于通过 HTTPS 传输的 Cookie，是否强制执行了 `Secure` 标志？
- [ ] 是 — 已应用于**所有**通过 TLS 传输的 Cookie  
- [ ] 否 — **未应用**该标志，允许 Cookie 通过明文 HTTP 发送  

### 是否使用 `SameSite` 属性来缓解 CSRF 风险？
- [ ] 是 — 为会话 Cookie 设置为 `Strict` 或 `Lax`  
- [ ] 是 — 设置为 `None` 但同时带有 `Secure` 标志  
- [ ] 否 — 属性**缺失**，或在**没有** `Secure` 的情况下设置为 `None`  

### `Domain` 和 `Path` 属性是否限制在必要的最小范围内？
- [ ] 是 — 作用域限定为**特定**子域名和应用程序路径  
- [ ] 否 — 作用域过**宽**（例如：设置为父域名或根目录 `/`），增加了攻击面  

### 敏感会话 Cookie 是否通过 `Expires` 或 `Max-Age` 属性正确过期？
- [ ] 是 — 设置了适当的过期日期  
- [ ] 否 — Cookie 是持久性的且生存周期过**长**  
- [ ] 否 — Cookie 仅限会话，但**缺乏**服务器端超时强制执行

---

## WSTG-SESS-03 — 会话固定 (Session Fixation)

会话固定 (Session Fixation) 发生在应用程序在用户成功通过身份验证 (Authentication) 后未能作废或轮换会话标识符 (Session Identifier) 的情况下，这允许攻击者将已知的会话令牌 (Session Token) 强制加载给受害者。如果应用程序在登录后保留了身份验证前的会话 ID (Session ID)，攻击者可以向受害者提供包含固定会话 ID 的特制链接，随后劫持已验证的会话。此漏洞通常表现在登录表单中，或者通过 URL 参数和 Cookie 接受的会话标识符。从攻击者的角度来看，利用过程包括通过恶意链接或响应头注入 (Header Injection) 漏洞“固定”会话，并等待受害者提供凭据，从而有效地绕过窃取活动令牌的需要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### 身份验证成功后，会话标识符是否发生变化？
- [ ] 是 — 已签发新的会话标识符，并且旧的标识符已作废 *(最安全)*  
- [ ] 是 — 已签发新的会话标识符，但旧的标识符仍然有效  
- [ ] 否 — 身份验证前后的会话标识符保持不变 *(严重)*  

### 应用程序是否接受通过 URL 参数提供的会话标识符？
- [ ] 否 — 会话 ID 仅通过 Cookie 进行管理  
- [ ] 是 — 应用程序接受 URL 中的会话 ID，但在登录时会将其忽略或轮换  
- [ ] 是 — URL 中的会话 ID 被接受，并在身份验证后继续保留  

### 攻击者定义的会话 ID 是否可以强制注入应用程序？
- [ ] 否 — 应用程序拒绝用户提供的无效或不存在的会话 ID  
- [ ] 是 — 应用程序接受并使用 Cookie 中提供的任意 ID 初始化会话  
- [ ] 是 — 应用程序接受并使用 URL 参数中提供的任意 ID 初始化会话  

### 身份验证前的会话是否已正确终止？
- [ ] 是 — 匿名会话在登录事件发生时被完全销毁  
- [ ] 否 — 匿名会话未被销毁，允许潜在的会话收集 (Session Harvesting)  

### 会话 Cookie 是否受到保护以防止客户端注入？
- [ ] 是 — 已应用 `HttpOnly` 和 `Secure` 标志，以防止基于脚本的会话固定  
- [ ] 否 — 未应用相关标志，允许通过跨站脚本攻击 (Cross-Site Scripting (XSS)) 设置会话 ID

---

## WSTG-SESS-04 — 测试暴露的会话变量 (Testing for Exposed Session Variables)

当敏感的会话标识符 (Session Identifiers) 或与状态相关的数据通过不安全的通道（如 URL 查询字符串 (URL Query Strings)、Referer 头部 (Referer Headers) 或系统日志）传输时，就会发生会话变量暴露。这种暴露显著增加了会话劫持 (Session Hijacking) 的风险，因为标识符可能会被缓存在浏览器历史记录中，被中间代理 (Intermediate Proxies) 记录，或者通过 Referer 头部泄露给第三方站点。渗透测试人员 (Pentesters) 通过监控所有出站请求并检查应用程序日志或服务器配置以查找疏忽导致的数据泄露来识别这些暴露。在现实场景中，攻击者通过从共享机器中收集有效的会话令牌 (Session Tokens) 或分析流量模式来利用这些泄露，从而绕过身份验证 (Authentication) 并获得对用户账户的未经授权访问。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果暴露的变量是允许立即接管账户的会话令牌，则严重程度变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**工具：** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### 会话标识符是否在 URL 查询字符串中传输？
- [ ] 否 — URL 查询字符串中**不存在**会话标识符  
- [ ] 是 — 存在标识符，但会话是短期的或低风险的  
- [ ] 是 — URL 查询字符串中**确实存在**唯一的会话 ID *(严重)*  

### 应用程序是否通过 Referer 头部泄露会话令牌？
- [ ] 否 — `Referrer-Policy` 防止了泄露，或者不存在第三方链接  
- [ ] 是 — 会话令牌**确实**通过 Referer 头部传输到了第三方域名  

### 会话变量是否存储在服务器端或应用程序日志中？
- [ ] 否 — 会话变量已被掩码处理或排除在日志之外  
- [ ] 是 — 会话变量**确实**以明文形式记录在应用程序或 Web 服务器日志中  

### 应用程序是否对更改状态的操作使用 GET 请求？
- [ ] 否 — 应用程序对敏感操作使用 `POST` 或其他非幂等方法  
- [ ] 是 — 使用了 `GET` 请求，导致会话数据被缓存在浏览器历史记录和中间代理中  

### 是否配置了 `Cache-Control` 头部以防止缓存会话相关数据？
- [ ] 是 — 敏感响应中**应用了** `Cache-Control: no-store`（或类似指令）  
- [ ] 是 — 已实施控制，但通过浏览器的极端情况**可能实现**绕过  
- [ ] 否 — 包含会话数据的敏感响应**可以**被浏览器缓存

---

## WSTG-SESS-05 — 跨站请求伪造测试

跨站请求伪造 (CSRF) 是一种漏洞，攻击者借此诱导受害者的浏览器在受害者当前已登录的另一个网站上执行非预期的操作。此漏洞利用了浏览器自动将“环境凭证”（如会话 Cookie (Session cookies) 或授权标头 (Authorization headers)）附加到传出请求的行为。攻击者通常通过在第三方网站上托管恶意脚本或隐藏表单，针对更改状态的操作（如更改密码、更新电子邮件地址或执行金融转账）发起攻击。成功的漏洞利用 (Exploit) 可能导致在用户不知情或未经同意的情况下实现全账户接管 (Account Takeover) 或未经授权的数据修改。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果受漏洞影响的操作允许账户接管、权限提升 (Privilege Escalation) 或未经授权的金融交易，严重程度将变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**工具：** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### 更改状态的请求是否实现了抗 CSRF 令牌 (Anti-CSRF tokens)？
- [ ] 是 — 所有更改状态的操作都需要唯一的、密码学强度高的令牌  
- [ ] 是 — 存在令牌，但每个会话**不是**唯一的，或者令牌是可预测的  
- [ ] 否 — **未**实现抗 CSRF 令牌  

### 服务器端对抗 CSRF 令牌的校验是否健壮？
- [ ] 是 — 服务器严格校验令牌，**无法**绕过  
- [ ] 是 — 执行了校验，但**可以**通过删除令牌参数来绕过  
- [ ] 是 — 执行了校验，但**可以**通过提供空白或伪造令牌来绕过  
- [ ] 是 — 执行了校验，但令牌**未**与用户会话绑定  

### 应用程序是否依赖于易被绕过的 CSRF 防护方法？
- [ ] 否 — 应用程序不单纯依赖弱标头或来源检查  
- [ ] 是 — 防护完全依赖于 `Referer` 或 `Origin` 标头，这些标头**可以**被伪造或剥离  
- [ ] 是 — 防护依赖于检查 `X-Requested-With` 标头，这**可以**通过跨源资源共享 (CORS) 配置错误来绕过  

### 会话 Cookie 是否配置了 `SameSite` 属性？
- [ ] 是 — 所有与会话相关的 Cookie 均将 `SameSite` 设置为 `Strict` 或 `Lax`  
- [ ] 否 — **缺失** `SameSite` 属性，默认为浏览器特定行为  
- [ ] 否 — `SameSite` 被显式设置为 `None`，且未配置 `Secure` 标志或额外的缓解措施  

### 是否可以通过更改 HTTP 请求方法来绕过 CSRF 防护？
- [ ] 否 — 无论使用何种 HTTP 方法，防护均强制执行  
- [ ] 是 — 令牌仅在 `POST` 请求上校验，但该操作**可以**通过 `GET` 执行  
- [ ] 是 — 更改方法（例如更改为 `PUT` 或 `DELETE`）会绕过令牌检查

---

## WSTG-SESS-06 — 注销功能测试 (Testing for Logout Functionality)

注销功能 (Logout functionality) 是一项关键的安全控制，旨在终止用户的会话 (Session) 并使客户端和服务器端关联的会话标识符失效。未能正确实现注销会导致会话固定 (Session Fixation) 或劫持攻击，因为在用户“注销”后访问该机器的攻击者可能会重复使用仍处于活动状态的会话令牌 (Session Token)。渗透测试人员 (Pentesters) 通过检查浏览器是否清除了会话 Cookie、服务器端会话状态是否被显式销毁，以及通过返回按钮导航是否允许访问缓存的敏感数据来评估这一点。一个安全的注销实现应确保一旦触发该操作，应用程序将不再授权任何使用旧会话令牌发起的后续请求。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **测试状态** | 未执行 |
| **严重程度** | 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**工具：** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### 是否存在且可访问的功能性注销机制？
- [ ] 是 — 注销按钮存在并触发终止请求  
- [ ] 否 — 注销按钮**缺失**或**未**触发终止事件  

### 会话标识符是否在服务器端失效？
- [ ] 是 — 服务器拒绝所有使用旧会话令牌的后续请求  
- [ ] 否 — 服务器在注销后**继续接受**旧会话令牌  

### 应用程序在注销时是否清除了浏览器中的会话 Cookie？
- [ ] 是 — Cookie 被过期日期或空值覆盖  
- [ ] 是 — Cookie 仍然存在，但在服务器上**不再有效**  
- [ ] 否 — Cookie 保留在浏览器中并**保留**其原始值  

### 注销后是否可以通过浏览器“返回”按钮访问经过身份验证的敏感内容？
- [ ] 否 — `Cache-Control: no-store` 或类似的标头防止在注销后查看敏感页面  
- [ ] 是 — 敏感页面被缓存，并且在会话终止后仍可以通导航**查看**

---

## WSTG-SESS-07 — 测试会话超时 (Testing Session Timeout)

会话超时测试评估应用程序是否在预定义的非活动期间或总时长后有效地终止用户会话。此项控制对于降低会话劫持 (Session Hijacking) 的风险至关重要，特别是在共享工作站或攻击者通过网络拦截或跨站脚本攻击 (XSS) 获取会话标识符 (Session Identifier) 的场景中。渗透测试人员分析在一段时间非活动后触发的空闲超时 (Idle Timeouts) 以及限制会话总寿命（无论活动与否）的绝对超时 (Absolute Timeouts)。从攻击者的角度来看，无限期或过长的会话为维持未经授权的访问并绕过重新身份验证的需求提供了更长的机会窗口。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **测试状态** | 未执行 |
| **严重程度** | 中 (Medium) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### 应用程序是否实现了空闲会话超时？
- [ ] 是 — 会话在一段较短的非活动时间（例如 15-30 分钟）后过期  
- [ ] 是 — 会话会过期，但超时时长**过长**（例如 > 24 小时）  
- [ ] 否 — 在非活动期间会话**无限期保持活跃**  

### 会话超时是否在服务端 (Server-side) 强制执行？
- [ ] 是 — 无论客户端状态如何，服务端都会拒绝带有过期令牌 (Token) 的请求  
- [ ] 否 — 超时**仅**通过客户端 JavaScript 或元刷新 (Meta-refresh) 强制执行  

### 应用程序是否实现了绝对会话超时？
- [ ] 是 — 无论活动与否，会话都会在固定的最大时长后终止  
- [ ] 否 — 只要有持续的活动，会话**就可以**无限期延长  

### 超时后会话标识符是否在服务端失效？
- [ ] 是 — 一旦达到超时阈值，会话令牌**不可**重复使用  
- [ ] 否 — 即使客户端已重定向到登录页面，会话令牌在服务端**仍然有效**  

### 是否可以使用自动心跳请求 (Heartbeat Requests) 无限期地保持会话活跃？
- [ ] 否 — 绝对超时或其他二级控制措施**阻止**了无限期的会话延长  
- [ ] 是 — 定期的后台请求（例如 AJAX）**可以**无限期地维持会话

---

## WSTG-SESS-08 — 会话拼图 (Session Puzzling)

会话拼图 (Session Puzzling)，也称为会话变量重载 (Session Variable Overloading)，发生在应用程序在不同模块中将同一个会话变量用于多种用途，或者在工作流转换期间未能正确清除会话状态时。攻击者通过在一种上下文中填充会话变量——例如未经身份验证的个人资料预览或多步注册——然后导航到受保护的区域来利用此行为，而应用程序会错误地信任这些预先存在的变量。这可能导致关键影响，包括身份验证绕过 (Authentication Bypass)、权限提升 (Privilege Escalation) 或对敏感数据的未经授权访问，其原理是诱骗应用程序相信会话处于比实际情况更高的权限状态。这种漏洞在复杂的有状态 (Stateful) 应用程序中最为常见，因为这些程序的会话管理逻辑在不同的功能组件之间应用不一致。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### 应用程序是否在不同模块中将相同的会话变量名称用于不同的目的？
- [ ] 否 — 变量名称是唯一的，或者作用域是隔离的  
- [ ] 是 — 存在共享变量名，但模块之间会清除状态  
- [ ] 是 — 存在共享变量名，且状态**未**被清除  

### 未经身份验证的操作能否影响在已验证身份的上下文中使用的会话变量？
- [ ] 否 — 会话变量仅在成功进行身份验证 (Authentication) **后**才初始化  
- [ ] 是 — 会话变量可以在登录前设置，但**不会**影响登录后的状态  
- [ ] 是 — 在预身份验证期间设置的会话变量在身份验证后**被**信任 *(严重)*  

### 当权限级别发生变化时，应用程序是否重新初始化会话标识符及其关联变量？
- [ ] 是 — 会话被完全重置，并**颁发**了新的 ID  
- [ ] 部分 — 颁发了新的 ID，但某些会话变量仍**持续存在**  
- [ ] 否 — 会话 ID 和所有变量**保持**不变  

### 是否可以通过非特权账户操纵会话变量来获取管理权限？
- [ ] 否 — 用户**无法**修改权限变量  
- [ ] 是 — 变量可以被修改，但应用程序会根据后端数据库对其进行**验证**  
- [ ] 是 — 变量可以被修改，且应用程序隐式**信任**会话状态 *(严重)*  

### 在完成特定工作流（例如密码重置、结账）后，会话状态是否立即被清除？
- [ ] 是 — 工作流的会话变量在完成后被销毁  
- [ ] 否 — 工作流变量**持续存在**，并可以在应用程序的其他区域重复使用

---

## WSTG-SESS-09 — 会话劫持测试 (Testing for Session Hijacking)

会话劫持 (Session Hijacking) 发生在攻击者捕获、预测或固定有效的会话标识符 (SID) 以获得对用户活动会话的未经授权访问时。此漏洞通常表现为传输层安全不足、缺乏 Cookie 安全标志或可预测的 SID 生成算法，从而允许攻击者完全绕过身份验证 (Authentication)。从攻击者的角度来看，成功的利用可以在不需要受害者凭据的情况下完全冒充受害者，这通常通过网络嗅探、跨站脚本攻击 (XSS) 或会话固定 (Session Fixation) 攻击来实现。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **测试状态** | 未执行 |
| **严重程度** | 高 (High) / 严重 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**工具：** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### 会话标识符在传输过程中是否受到保护？
- [ ] 是 — `Secure` 标志已**启用**且 HTTP 严格传输安全 (HSTS) 已严格强制执行  
- [ ] 是 — `Secure` 标志已**启用**但 HSTS 已**禁用**  
- [ ] 否 — 未应用 `Secure` 标志，允许通过未加密通道拦截 SID *(高)*  

### 会话标识符是否防止了客户端脚本访问？
- [ ] 是 — 所有与会话相关的 Cookie 均已**应用** `HttpOnly` 标志  
- [ ] 否 — 未应用 `HttpOnly` 标志，允许通过 XSS 窃取 SID *(严重)*  

### 应用程序是否实现了与客户端属性的会话绑定？
- [ ] 是 — 会话已绑定至客户端属性（IP/User-Agent）且**无法**重复使用  
- [ ] 是 — 存在会话绑定，但可能通过请求头伪造 (Header Spoofing) 进行**绕过**  
- [ ] 否 — 不存在会话绑定；SID 从任何来源使用均**有效**  

### 会话标识符是否具有足够的随机性以防止预测？
- [ ] 是 — SID 使用加密安全伪随机数生成器 (CSPRNG)  
- [ ] 否 — SID 长度不足或遵循**可预测**的序列/模式  

### 是否对并发会话进行了管理以限制攻击窗口？
- [ ] 否 — 应用程序严格强制每个用户仅限一个活动会话  
- [ ] 是 — 多个并发会话已**启用**但受到监控  
- [ ] 是 — 跨不同设备/位置**可能**存在无限并发会话

---

## WSTG-SESS-10 — 测试 JSON Web Tokens (JWT)

JSON Web Tokens (JWT) 常用于无状态会话管理（Session Management）和身份传播（Identity Propagation），但其安全性完全依赖于签名算法（Signing Algorithms）的正确实现和严格的声明验证（Claim Verification）。攻击者经常尝试通过利用 “none” 算法缺陷、暴力破解（Brute Force）弱 HS256 密钥，或执行算法切换攻击（Algorithm-switching attacks，即使用非对称公钥作为对称 HMAC 密钥）来绕过身份验证。这些漏洞通常体现在会话 Cookie 或授权头（Authorization headers）中，可能允许攻击者在不破坏令牌加密完整性的情况下，通过修改 `role` 或 `user_id` 等声明来提升权限（Escalate privileges）或冒充（Impersonate）任何用户。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**工具：** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### 服务器是否接受 "none" 算法？
- [ ] 否 — 服务器**拒绝**在标头（Header）中使用 `alg: none` 的令牌  
- [ ] 是 — 服务器**接受**使用 `alg: none` 且签名为空的令牌 *(严重)*  

### JWT 签名如何验证以及如何防止暴力破解？
- [ ] 是 — 使用了强非对称密钥（Asymmetric keys，如 RS256/ES256）或高熵（High-entropy）的 HMAC 密钥  
- [ ] 是 — 使用了 HS256，但由于熵值低或使用了默认值，密钥**可以**被暴力破解  
- [ ] 否 — **未应用**签名验证，或者**可以**通过算法切换（从 RS256 切换到 HS256）绕过验证  

### 后端是否严格强制执行标准声明（exp, nbf, iat）？
- [ ] 是 — 存在 `exp`（过期时间）且**无法**被绕过  
- [ ] 是 — 存在 `exp`，但服务器在验证期间**未强制**执行  
- [ ] 否 — **缺失**或忽略了过期声明，从而允许无限期的会话重放（Session Replay）  

### 该实现是否防止了基于标头的密钥注入 (kid, jku, x5u)？
- [ ] 是 — 标头已经过清理（Sanitized），仅**允许**受信任的内部密钥/来源  
- [ ] 否 — `kid` (Key ID) 标头容易受到目录遍历（Directory Traversal）或 SQL 注入 (SQL Injection) 攻击，从而指向已知文件  
- [ ] 否 — `jku` 或 `x5u` 标头**可以**被指向攻击者控制的 URL 以加载恶意密钥

---

## WSTG-SESS-11 — 测试并发会话 (Testing for Concurrent Sessions)

并发会话 (Concurrent Sessions) 测试旨在评估应用程序是否允许单个用户帐户在不同的浏览器、设备或 IP 地址上维持多个同时进行的活动会话。如果未能限制并发会话，攻击者在利用被劫持的会话令牌 (Session Tokens) 或泄露的凭据 (Compromised Credentials) 时，可以在不提醒合法用户或不终止现有会话的情况下获得更长的攻击窗口。这种行为通常通过从多个来源进行身份验证并监控会话稳定性来评估，往往会暴露会话管理逻辑中的弱点或缺乏以安全为核心的业务规则。从攻击者的角度来看，缺乏并发控制允许在初始入侵后实现隐蔽的持久化 (Persistence)，并有助于开展并行的自动化攻击。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### 应用程序策略是否限制了并发会话？
- [ ] 是 — 每个用户仅**允许**一个会话；新的登录会使旧会话失效（最安全）  
- [ ] 是 — **强制执行**固定数量的并发会话且不能超过该限制  
- [ ] 否 — **可以**建立无限数量的并发会话  

### 当达到会话限制时，应用程序如何响应？
- [ ] 最早的活动会话**被自动终止**（会话固定 (Session Fixation)/劫持 (Hijacking) 缓解措施）  
- [ ] 在授权新会话之前，系统会**提示**用户终止现有会话  
- [ ] 新的登录尝试**被阻止**，直到从另一台设备手动注销为止  
- [ ] 未采取任何措施 — 多个会话同时保持**启用**状态  

### 应用程序是否为用户提供会话管理界面？
- [ ] 是 — 用户**可以**查看活动会话（IP、设备、时间）并远程终止它们  
- [ ] 是 — 用户**可以**查看活动会话，但**不能**远程终止它们  
- [ ] 否 — 用户**无法**查看或管理与其帐户关联的其他活动会话  

### 当检测到并发登录活动时，是否通知用户？
- [ ] 是 — 针对来自新设备或位置的登录，**触发**告警（电子邮件、短信或应用内通知）  
- [ ] 否 — 建立并发会话时**不提供**任何通知

---

## WSTG-INPV-01 — 反射型跨站脚本攻击 (Reflected Cross Site Scripting, XSS)

反射型跨站脚本攻击 (Reflected Cross Site Scripting, XSS) 发生在应用程序在未经充分验证或编码的情况下，将不可信的数据包含在 HTTP 响应中，导致有效负载 (Payload) 在受害者的浏览器上下文中执行。攻击者通过社会工程学 (Social engineering) 手段，通常是通过精心构造的 URL 或表单发送恶意 Payload，以劫持用户会话 (User sessions)、窃取敏感 Cookie 或代表用户执行未经授权的操作。这种漏洞常见于搜索参数、错误消息以及任何将输入直接反射回用户界面的端点 (Endpoint)。漏洞利用依赖于受害者与恶意链接的交互，这使其成为一种非持久性但针对特定管理员或已认证用户非常有效的攻击向量。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **测试状态** | 未执行 |
| **严重程度** | 高 (High) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**工具：** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### 用户提供的输入是否在响应正文中反射？
- [ ] 否 — 输入**从未**反射回用户  
- [ ] 是 — 输入已反射，但经过正确编码，无法绕过 (Bypass)  
- [ ] 是 — 输入在**没有任何**编码或清理 (Sanitization) 的情况下被反射  

### 是否实施了上下文感知的输出编码 (Context-aware output encoding)？
- [ ] 是 — 根据具体的反射上下文**应用**了正确的编码（HTML、属性、JavaScript）  
- [ ] 是 — 已**应用**编码，但对于特定上下文而言不足（例如，在 `<script>` 标签内使用 HTML 编码）  
- [ ] 否 — **未应用**编码  

### 输入验证或 Web 应用防火墙 (WAF) 过滤器是否可以被绕过？
- [ ] 否 — 过滤器有效地拦截了所有常见及混淆的 XSS Payload  
- [ ] 是 — 存在过滤器，但使用字符编码、非标准标签或多语义负载 (Polyglots) **可以实现绕过**  
- [ ] 否 — 未部署任何过滤器或验证机制  

### 反射输入的执行上下文是什么？
- [ ] 安全 — 输入在不可执行的位置反射（例如，在标准的 `<div>` 或 `<span>` 标签内）  
- [ ] 风险 — 输入在 HTML 属性内部或标签的 `src`/`href` 属性中反射  
- [ ] 关键 — 输入直接在 `<script>` 块、事件句柄 (Event handlers) 或模板字面量 (Template literals) 中反射  

### 是否存在现代浏览器安全响应头以减轻 XSS 影响？
- [ ] 是 — 已**启用** `Content-Security-Policy` (CSP)，并配有严格的 script-src  
- [ ] 是 — 已**启用** `Content-Security-Policy` (CSP)，但包含 `unsafe-inline` 或弱白名单  
- [ ] 否 — 未提供 CSP 或过时的 `X-XSS-Protection` 响应头

---

## WSTG-INPV-02 — 存储型跨站脚本攻击 (Stored Cross Site Scripting, XSS)

存储型跨站脚本攻击 (Stored Cross Site Scripting, XSS)，也称为持久型 XSS (Persistent XSS)，发生在应用程序接收来自用户的数据并将其存储在持久性数据库、文件系统或其他存储介质中，而没有进行充分的验证或编码时。当受害者随后导航到检索并显示这些未经处理的数据的页面时，恶意脚本将在其浏览器上下文中以应用程序的源 (Origin) 执行。这种漏洞特别危险，因为它不需要受害者点击专门构建的链接；任何查看受影响页面的用户都会成为目标，可能导致会话劫持 (Session Hijacking)、未经授权的操作或凭据收集 (Credential Harvesting)。攻击者通常针对评论区、用户个人资料、留言板和管理日志，在这些地方输入的内容会持续地呈现给其他用户或管理员。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**工具：** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### 是否存在存储用户输入以便稍后向其他用户显示的入口点？
- [ ] 否 — 应用程序不存储用户可控的输入用于后续渲染  
- [ ] 是 — 输入已存储（例如个人资料、评论），但**未**在 HTML 上下文中渲染  
- [ ] 是 — 输入已存储并渲染给其他用户或管理员  

### 在渲染存储的数据时，是否应用了输出编码或清理 (Sanitization)？
- [ ] 是 — **已应用**上下文感知的输出编码，且**无法**绕过 *(最安全)*  
- [ ] 是 — **已应用**清理（例如 `DOMPurify`），但由于配置错误**可以**绕过  
- [ ] 否 — 数据**未经**编码或清理直接渲染到 DOM 中 *(高危)*  

### 是否可以使用替代攻击载荷 (Payload) 绕过输入验证或清理？
- [ ] 否 — 过滤器安全地处理各种编码、非标准标签和事件处理器  
- [ ] 是 — 使用 HTML 实体编码或特定标签（例如 `<svg>`, `<img>`）**可以**实现绕过  
- [ ] 是 — 通过多语义载荷 (Polyglot Payloads) 或基于变异的 XSS (mXSS) **可以**实现绕过  

### 内容安全策略 (Content Security Policy, CSP) 是否提供了第二层防御？
- [ ] 是 — **启用**了严格的 CSP，并防止了内联脚本和未经授权来源的执行  
- [ ] 是 — **启用**了 CSP 但配置错误（例如 `unsafe-inline` 或宽泛的 `script-src`）  
- [ ] 否 — 应用程序**未启用** CSP  

### 是否可能实现“存储型 XSS 到 RCE”或其他高影响的链式攻击（盲 XSS (Blind XSS)）？
- [ ] 否 — 影响仅限于客户端浏览器上下文  
- [ ] 是 — 存储型 XSS **可以**用于针对内部面板中的管理员（盲 XSS）  
- [ ] 是 — 存储型 XSS **可以**与其他漏洞链接（例如 CSRF、敏感数据外泄）

---

## WSTG-INPV-03 — HTTP 谓词篡改 (HTTP Verb Tampering) 测试

HTTP 谓词篡改 (HTTP Verb Tampering) 利用了 Web 服务器和应用程序框架在根据使用的 HTTP 方法授权访问特定资源时的缺陷。攻击者尝试通过使用 `HEAD`、`PUT`、`OPTIONS` 甚至后端可能处理不一致的任意非标准字符串来替换 `GET` 或 `POST` 等标准方法，从而绕过安全限制。这种漏洞通常发生在安全配置（例如 Java EE web.xml 过滤器或 .NET 授权规则）明确列出了允许的方法但未能考虑其他方法时，或者使用了“按方法拒绝”而不是“拒绝所有”的方法时。成功的漏洞利用可能导致对管理功能的未经授权访问、数据修改或有关服务器内部配置的信息泄露。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果可以在没有身份验证 (Authentication) 的情况下执行管理功能或数据修改 (PUT/DELETE)，严重程度将变为高。

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### 应用程序是否响应非标准或任意 HTTP 方法？
- [ ] 否 — 应用程序拒绝除明确要求的方法之外的所有方法  
- [ ] 是 — 应用程序响应标准方法 (OPTIONS, TRACE)，但**无法**绕过安全控制  
- [ ] 是 — 应用程序接受任意字符串（例如 FOO, TEST）并将其作为 `GET` 或 `POST` 请求进行处理  

### 是否可以通过更改 HTTP 方法来绕过身份验证/授权 (Authorization)？
- [ ] 否 — 无论 HTTP 谓词如何，安全控制都会全局应用  
- [ ] 是 — 已设置控制措施，但使用 `HEAD` 方法**可能**实现绕过  
- [ ] 是 — 安全控制**未应用**于替代方法，允许对受限端点进行未经授权的访问  

### 服务器上是否启用了 `PUT` 或 `DELETE` 等危险方法？
- [ ] 否 — 危险方法已**禁用**或返回 405 Method Not Allowed  
- [ ] 是 — 已启用方法，但需要有效的、高权限的身份验证  
- [ ] 是 — 方法已**启用**，并允许未经授权的文件上传或资源删除 *(紧急 - Critical)*  

### `OPTIONS` 方法是否泄露了关于允许谓词的敏感信息？
- [ ] 否 — `OPTIONS` 已**禁用**或返回通用响应  
- [ ] 是 — `OPTIONS` 返回 `Allow` 标头，但不暴露受限的内部方法  
- [ ] 是 — `OPTIONS` 泄露了仅限内部或管理使用的方法，有助于进一步漏洞利用 (Exploit)  

### 是否启用了 `TRACE` 方法，从而可能导致跨站跟踪 (Cross-Site Tracing, XST)？
- [ ] 否 — `TRACE` 和 `TRACK` 方法已**禁用**  
- [ ] 是 — `TRACE` 已**启用**并回显 HTTP 标头，包括敏感的 Cookie 或令牌 (Token)

---

## WSTG-INPV-04 — HTTP 参数污染 (HTTP Parameter Pollution, HPP) 测试

HTTP 参数污染 (HTTP Parameter Pollution, HPP) 发生在应用程序接收到多个同名的 HTTP 参数，并以不一致或不安全的方式处理它们时。通过提供重复的参数，攻击者可以操纵应用程序的内部逻辑，从而可能绕过 Web 应用防火墙 (Web Application Firewalls, WAF)、输入验证过滤器或访问控制机制。该漏洞高度依赖于底层的 Web 服务器或应用程序框架，这些框架可能会选择重复参数中的第一个、最后一个或拼接后的版本。利用 (Exploit) 行为通常针对将参数传递给后端应用程序接口 (Application Programming Interface, API)、数据库查询或 URL 重定向机制的端点，造成的影响从跨站脚本攻击 (Cross-Site Scripting, XSS) 到未经授权的数据泄露不等。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **测试状态** | 未执行 |
| **严重性** | 中 |

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### 应用程序如何处理同名的多个 HTTP 参数？
- [ ] 否 — 重复的参数被服务器拒绝 (Rejected) 或忽略
- [ ] 是 — 服务器一致地仅选择第一个或最后一个出现的参数
- [ ] 是 — 服务器对值进行级联 (Concatenates)，可能允许注入攻击

### HPP 是否可被利用来绕过 WAF 或输入过滤器等安全控制措施？
- [ ] 否 — 安全控制措施能够正确检查参数的所有出现
- [ ] 是 — WAF 或过滤器仅检查第一个出现的参数，允许恶意的第二个参数通过
- [ ] 是 — 级联允许攻击者将有效载荷 (Payload) 拆分到多个参数中以规避检测

### 应用程序是否在未进行适当清理的情况下在响应中反射受污染的参数？
- [ ] 否 — 参数在 UI 中反射之前已进行清理 (Sanitized) 或编码
- [ ] 是 — 污染导致内容被反射，但已应用清理措施
- [ ] 是 — 污染导致内容被反射且未应用清理措施，从而导致 XSS 或逻辑错误

### 传递给内部 API 调用或后端系统的参数是否容易受到污染攻击？
- [ ] 否 — 后端请求使用安全、非动态的构建方法
- [ ] 是 — HPP 允许攻击者在后端请求结构中注入或覆盖参数

### 当参数在 GET 与 POST 请求中受到污染时，应用程序是否表现出不同的行为？
- [ ] 否 — 所有 HTTP 方法的行为保持一致
- [ ] 是 — 仅 GET 参数容易受到污染
- [ ] 是 — 仅 POST (body) 参数容易受到污染

---

## WSTG-INPV-05 — SQL 注入 (SQL Injection)

SQL 注入 (SQL Injection) 发生在用户提供的输入在未经过适当参数化或过滤的情况下被纳入 SQL 查询中，从而允许攻击者操纵查询逻辑。成功利用此漏洞可能导致未经授权的数据泄露、身份验证绕过、数据修改，在某些情况下还可以通过 `xp_cmdshell` 或 `LOAD_FILE()` 等数据库功能执行远程代码执行 (Remote Code Execution)。该漏洞常见于登录表单、搜索功能、REST API 参数、HTTP 标头以及任何从用户控制的输入构建动态 SQL 查询的端点。从攻击者的角度来看，识别注入点 (Injection Point) 是全面攻破数据库以及在基础设施内进行潜在横向移动 (Lateral Movement) 的主要入口。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **测试状态** | 未执行 |
| **严重程度** | 严重 (Critical) |

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**工具：** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### 用户可控的参数是否使用安全的数据库访问方法进行处理？
- [ ] 是 — 应用程序完全使用对象关系映射 (ORM) 或参数化查询 (Parameterized Queries)，且**无法**绕过  
- [ ] 是 — 使用了参数化查询，但通过边缘情况（如 `ORDER BY` 子句）**可以**绕过  
- [ ] 否 — 在**没有**参数化的情况下，使用原始字符串拼接来构建 SQL 查询  

### 身份验证机制是否可以通过 SQL 注入被规避？
- [ ] 否 — 登录参数**不存在**注入漏洞  
- [ ] 是 — 使用基于恒真式 (Tautology) 的有效载荷 (Payload)（例如 `' OR 1=1 --`）**可以**绕过身份验证  

### 是否可以通过带内 (In-band) 或带外 (Out-of-band) 技术进行数据泄露？
- [ ] 否 — 未发现数据泄露  
- [ ] 是 — 基于联合查询 (UNION-based) 或基于错误 (Error-based) 的泄露**是可能的**  
- [ ] 是 — 盲注 (Blind)（基于布尔值或基于时间）泄露**是可能的**  
- [ ] 是 — 带外 (DNS/HTTP) 泄露**是可能的**  

### 应用程序功能是否存在二阶 SQL 注入 (Second-order SQL Injection) 漏洞？
- [ ] 否 — 存储的数据在检索用于后续查询时得到了安全处理  
- [ ] 是 — 用户输入被存储，随后在**未经**验证的情况下被用于不安全的 SQL 查询  

### 数据库服务账户权限是否被限制在所需的最低限度？
- [ ] 是 — 服务账户拥有**最小权限 (Least Privilege)**（例如，仅限于特定的表/操作）  
- [ ] 否 — 服务账户拥有高权限（例如，`DROP TABLE`、`FILE` 权限，或**启用了** `xp_cmdshell`）

---

## WSTG-INPV-06 — LDAP 注入测试

LDAP 注入 (LDAP Injection) 发生在应用程序将用户提供的数据合并到轻型目录访问协议 (Lightweight Directory Access Protocol, LDAP) 过滤器中，而没有进行适当的清理或转义 (Sanitization or Escaping) 时，这使得攻击者能够操纵查询逻辑。通过注入星号、括号和逻辑运算符等特殊字符，攻击者可以修改搜索过滤器以绕过身份验证机制 (Authentication mechanisms) 或窃取敏感目录信息 (Exfiltrate sensitive directory information)，包括用户名、组成员身份和组织属性。这种漏洞通常出现在与活动目录 (Active Directory) 或 OpenLDAP 对接的功能中，如公司目录搜索、员工门户或单点登录 (SSO) 系统。从攻击者的角度来看，当应用程序抑制直接查询输出时，成功利用通常涉及盲注技术 (Blind techniques)，以便逐位枚举 (Enumerate) 目录结构或属性值。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**工具：** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### 应用程序是否利用 LDAP 进行身份验证或目录搜索？
- [ ] 否 — 应用程序架构中**未使用** LDAP  
- [ ] 是 — 使用了 LDAP，且所有输入都经过严格清理或参数化  
- [ ] 是 — 使用了 LDAP，且用户输入在**未进行**适当转义的情况下被拼接进过滤器中  

### 用户输入中的 LDAP 元字符是否经过适当的转义或过滤？
- [ ] 是 — `(`, `)`, `&`, `|`, `*`, 和 `\` 等元字符 (Meta-characters) **不允许**使用或已被正确转义  
- [ ] 否 — 元字符**可以**被注入，以改变 LDAP 过滤器的结构  

### 是否可以通过注入绕过身份验证机制？
- [ ] 否 — 登录逻辑**不易受**查询操纵的影响  
- [ ] 是 — **可以**在用户名或密码字段中使用逻辑“或” (`|`) 或通配符 (`*`) 注入来绕过身份验证  

### 是否可以从目录服务中进行盲数据窃取？
- [ ] 否 — 搜索结果受限，且未观察到时间或布尔差异  
- [ ] 是 — **可以**通过基于布尔的盲注 (Boolean-based blind injection) 技术逐位枚举目录属性  
- [ ] 是 — 由于查询操纵，完整的目录记录**被反映**在应用程序响应中  

### 次要应用程序组件（例如邮件程序、SSO）是否容易受到基于 LDAP 的属性注入攻击？
- [ ] 否 — LDAP 属性在所有组件中均得到安全处理  
- [ ] 是 — 攻击者**可以**通过注入修改其自身属性（如电子邮件、组成员身份），以提升权限或重定向通信

---

## WSTG-INPV-07 — XML 注入 (XML Injection)

XML 注入 (XML Injection) 发生在应用程序将用户提供的数据合并到 XML 文档或流中，且未对 XML 元字符 (Meta-characters) 进行适当的验证、净化或转义时。通过注入特定的 XML 标签或结构元素，攻击者可以操纵文档逻辑、绕过身份验证机制，或干扰后端处理的数据完整性。这种漏洞常见于基于 SOAP 的 Web 服务、接收 XML 的 REST API、SAML 断言 (SAML assertions) 以及动态生成 XML 配置文件或报告的应用程序。从攻击者的角度来看，其目标是脱离预定的数据上下文以改变文档结构，从而可能导致未经授权的权限提升 (Privilege escalation) 或执行非预期的业务逻辑 (Business logic)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 高 (High) / 中 (Medium) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**工具：** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### 应用程序是否接受并处理来自用户的 XML 格式输入？
- [ ] 否 — 应用程序**不**处理 XML 输入  
- [ ] 是 — 通过 API (SOAP/REST) 处理 XML 输入  
- [ ] 是 — 通过文件上传（SVG, DOCX 等）处理 XML  

### XML 元字符（例如：`<`, `>`, `&`, `'`, `"`）是否经过适当的转义或净化？
- [ ] 是 — 所有元字符都经过了**适当的**转义或净化 *(最安全)*  
- [ ] 是 — 应用了部分过滤，但**可以通过**编码绕过  
- [ ] 否 — 原始输入在**未经过**净化的情况下直接嵌入到 XML 结构中  

### 是否对所有 XML 输入强制执行服务端架构验证 (Schema validation - XSD/DTD)？
- [ ] 是 — **启用**了严格的架构验证，可防止结构性更改  
- [ ] 是 — **启用**了验证，但**不能**防止数据字段内的标签注入 (Tag Injection)  
- [ ] 否 — 架构验证已**禁用**或未实现  

### 攻击者能否注入新的 XML 标签以改变文档结构或逻辑？
- [ ] 否 — 无论输入如何，结构都保持不变  
- [ ] 是 — 可以注入标签，但**不能**改变业务逻辑  
- [ ] 是 — **可以**进行标签注入，并允许绕过逻辑或修改数据 *(高危)*  

### 是否可以使用 CDATA 部分 (CDATA sections) 或 XML 注释 (XML comments) 操纵 XML 解析器？
- [ ] 否 — 解析器能正确处理或拒绝 CDATA/注释标记  
- [ ] 是 — **可以**注入 `<![CDATA[...]]>` 或 `<!-- ... -->` 以隐藏输入或绕过过滤器

---

## WSTG-INPV-08 — 服务器端包含注入 (SSI Injection) 测试

当应用程序在将用户输入并入由服务器服务器端包含 (Server-Side Includes, SSI) 引擎处理的 HTML 文件之前，未能对其进行清洗或过滤时，就会发生服务器端包含注入 (SSI Injection)。攻击者利用此漏洞注入 SSI 指令（例如 `<!--#exec cmd="id" -->`），以执行任意 Shell 命令 (Shell commands)、读取本地文件或窃取环境变量。此漏洞通常存在于提供 `.shtml`、`.shtm` 或 `.stm` 等旧版扩展名文件的应用程序中，或者 Web 服务器被显式配置为在标准 HTML 文件中解析 SSI。成功利用该漏洞可授予攻击者与 Web 服务器进程相同的权限，通常导致整个系统失陷或敏感数据泄露。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Web 服务器是否支持并处理 SSI 指令？
- [ ] 否 — 服务器**不支持** SSI 或 `.shtml` 等文件扩展名**已禁用**  
- [ ] 是 — SSI **已启用**，但用户输入**未**在处理后的页面中反射  
- [ ] 是 — SSI **已启用**，且用户输入**已**在处理后的页面中反射  

### 是否可以通过 `#exec` 指令执行任意系统命令？
- [ ] 否 — `#exec` 指令**已禁用**或受服务器配置限制  
- [ ] 是 — 通过 `#exec cmd` 或 `#exec cgi` 有效负载 (Payload) **可以执行**命令  

### 是否可以窃取敏感文件或环境变量？
- [ ] 否 — `#include` 或 `#config` 等指令**已禁用**  
- [ ] 是 — 通过 `#include virtual` **可以读取**本地文件（例如 `/etc/passwd`）  
- [ ] 是 — 通过 `#printenv` 或 `#echo` **可以窃取**服务器环境变量  

### 输入验证或 WAF 控制对 SSI Payload 是否有效？
- [ ] 是 — **应用了**严格的输入验证且**无法绕过**  
- [ ] 是 — **应用了**验证/Web 应用防火墙 (WAF)，但使用字符编码或不同的 SSI 语法**可以绕过**  
- [ ] 否 — 对 SSI 相关字符（`<`, `!`, `#`, `-`, `"`）不存在验证或 WAF 防护

---

## WSTG-INPV-09 — XPath 注入 (XPath Injection) 测试

当应用程序使用用户提供的信息来构建针对 XML 数据的 XPath 查询，从而允许攻击者干扰查询逻辑时，就会发生 XPath 注入 (XPath Injection)。通过注入特定的语法，如单引号、括号和逻辑运算符，攻击者可以导航 XML 文档结构、绕过身份验证 (Authentication) 或外泄敏感数据节点。这种漏洞常见于 Web 服务、配置管理界面以及依赖 XML 数据存储而非传统 SQL 数据库 (SQL Databases) 的遗留系统。从攻击者的角度来看，这通常利用基于布尔 (Boolean-based) 或基于错误 (Error-based) 的技术来系统地映射 XML 树并从后端检索隐藏值。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**工具：** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### 用于构建 XPath 查询的用户输入是否经过适当的净化 (Sanitized) 或参数化 (Parameterized)？
- [ ] 是 — 输入**未**用于 XPath 查询，或已严格参数化  
- [ ] 是 — **已应用**强健的输入验证/编码，且**无法**绕过  
- [ ] 是 — **已应用**部分验证，但使用编码字符**可能**实现绕过  
- [ ] 否 — 用户输入被直接拼接进 XPath 查询 *(严重)*  

### 应用程序是否通过错误消息泄露 XML/XPath 结构细节？
- [ ] 否 — 显示通用错误页面，且**未**泄露 XML 细节  
- [ ] 是 — 错误消息揭示了 XML 处理的存在，但**未**泄露路径细节  
- [ ] 是 — 详细的错误消息揭示了 XPath 语法、节点名称或文件路径  

### 是否可以使用 XPath 逻辑绕过身份验证或授权 (Authorization) 逻辑？
- [ ] 否 — 身份验证逻辑**不**依赖于 XML/XPath，或已安全实现  
- [ ] 是 — 可以使用如 `' or 1=1 or '` 的基于逻辑的有效负载 (Payload) 绕过登录或权限检查  

### 是否可以通过盲注 (Blind Injection) 枚举 XML 文档结构或数据？
- [ ] 否 — 应用程序**不容易**受到基于布尔或基于时间 (Time-based) 的推断攻击  
- [ ] 是 — 节点名称和数据**可以**利用基于布尔的推断进行外泄 (Exfiltrate)  
- [ ] 是 — **可以**进行完整的 XML 树遍历和数据提取

---

## WSTG-INPV-10 — IMAP SMTP 注入 (IMAP SMTP Injection)

当 Web 应用程序在将用户提供的数据纳入发送至邮件服务器的命令之前，未能对其进行适当过滤时，就会产生 IMAP 和 SMTP 注入 (IMAP SMTP Injection) 漏洞。通过注入回车和换行 (Carriage Return and Line Feed, CRLF) 字符，攻击者可以终止预定命令并附加任意邮件指令，例如添加额外的收件人（抄送/密送，CC/BCC）或修改邮件正文。这些缺陷最常见于 Web 到邮件网关、联系表单以及通过 IMAP4 或 SMTP 等协议直接与邮件服务器通信的邮件客户端。成功利用此漏洞可导致未经授权的垃圾邮件转发 (Relaying)、敏感邮件内容外泄，在某些情况下，甚至会导致邮件服务器完整性的全面受损。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### 应用程序是否处理用户提供的输入以构建 IMAP 或 SMTP 命令？
- [ ] 否 — 应用程序**不**通过用户输入与邮件服务器交互  
- [ ] 是 — 应用程序与邮件服务器交互，但使用了安全的 API 或库  
- [ ] 是 — 应用程序使用用户受控的参数构建原始邮件命令  

### 是否对输入进行了验证以防止注入回车 (`\r`) 和换行 (`\n`) 序列？
- [ ] 是 — CRLF 字符被**严格**过滤、编码或拒绝  
- [ ] 是 — 输入根据严格的字符白名单 (Allowlist) 进行验证  
- [ ] 否 — CRLF 字符**未**被过滤，允许命令终止和注入  

### 攻击者能否注入额外的邮件标头，如 `Bcc:` 或 `Cc:`？
- [ ] 否 — 由于严格的输入限制，**无法**修改标头  
- [ ] 是 — **可以**通过 CRLF 序列注入额外的标头  
- [ ] 是 — 外部域的邮件转发**已启用**且可被利用 *(严重)*  

### 应用程序是否允许操纵 IMAP 命令以访问未经授权的邮箱？
- [ ] 否 — 应用程序**不**使用 IMAP，或将邮箱访问严格限制为已验证身份的用户  
- [ ] 是 — IMAP 命令注入**是可能的**，但仅限于元数据枚举 (Metadata Enumeration)  
- [ ] 是 — IMAP 命令注入允许读取或删除其他用户的电子邮件  

### 后端邮件服务器是否存在命令流水线 (Command Pipelining) 漏洞？
- [ ] 否 — 邮件服务器拒绝批处理命令或单个会话中的多个命令  
- [ ] 是 — 邮件服务器处理通过单个输入字段注入的多个命令

---

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

## WSTG-INPV-12 — 命令注入 (Command Injection)

命令注入 (Command Injection) 发生在应用程序将不安全的、由用户提供的数据（如表单输入、HTTP 标头 (HTTP Headers) 或 Cookies）传递给系统外壳 (System Shell) 时。此漏洞允许攻击者在服务器上执行任意操作系统命令，通常具有 Web 应用程序进程的权限。从攻击者的角度来看，利用该漏洞的方法是使用分号、管道符或反引号等 Shell 元字符 (Shell Metacharacters) 将恶意命令链接到预期的执行流中。其影响通常是灾难性的，会导致全系统沦陷、未经授权的数据外传或在内网中进行横向移动 (Lateral Movement)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **测试状态** | 未执行 |
| **严重程度** | 严重 (Critical) |

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**工具：** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### 应用程序是否调用系统级命令或 Shell 可执行文件？
- [ ] 否 — 应用程序**不**与操作系统 Shell 交互  
- [ ] 是 — 应用程序使用内部 API 或高级库执行系统任务  
- [ ] 是 — 应用程序通过 `system()`、`exec()` 或 `popen()` 等系统调用直接调用 Shell 命令  

### 在传递给系统调用之前，是否对用户输入进行了验证或清理？
- [ ] 是 — **应用了**严格的白名单 (Allow-listing) 和输入验证  
- [ ] 是 — 使用了清理 (Sanitization) 或转义，但**可能被绕过**  
- [ ] 否 — 用户输入在**未经**验证的情况下直接传递给 Shell  

### 是否可以使用 Shell 元字符来操纵命令执行流？
- [ ] 否 — 元字符被正确过滤或转义  
- [ ] 是 — **可能实现**带内 (In-band) 执行，即命令输出反映在响应中  
- [ ] 是 — **可能实现**盲 (Blind) 执行，即没有输出反映在响应中  

### 是否可以从主机进行带外 (Out-of-band, OOB) 数据外传？
- [ ] 否 — 服务器没有出站连接或带外 (OOB) 信号（DNS/HTTP）失败  
- [ ] 是 — **可能**通过基于 DNS 或 HTTP 的带外 (OOB) 信号进行数据外传  

### 应用程序进程是否以提升的系统权限运行？
- [ ] 否 — 应用程序使用专用的低权限服务帐户运行  
- [ ] 是 — 应用程序以 `root`、`Administrator` 或高权限用户身份运行 *（严重）*

---

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

## WSTG-INPV-14 — 潜伏型漏洞测试 (Testing for Incubated Vulnerabilities)

潜伏型漏洞 (Incubated Vulnerabilities)，也称为持久型或二阶漏洞 (Second-order Vulnerabilities)，当恶意负载 (Payload) 被应用程序以“冷”状态存储，随后在不同的上下文 (Context) 中被执行或处理时，就会发生此类漏洞。这种攻击模式通常针对后端系统、管理控制台或自动报告工具，这些工具从共享数据库或文件系统中检索数据，而没有进行充分的重新验证或输出编码 (Output Encoding)。渗透测试人员 (Pentesters) 必须追踪用户输入的生命周期，从入口点（即“孵化器”）到该数据最终被其他组件或辅助应用程序呈现或利用的每个位置。成功的利用通常会导致高影响的后果，例如通过存储型跨站脚本攻击 (Stored XSS) 劫持管理员会话，或者通过内部报表模块中的二阶 SQL 注入 (Second-order SQL Injection) 进行数据外泄。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**工具：** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### 是否存在用户控制的输入被存储以供其他用户或进程后续检索的端点 (Endpoints)？
- [ ] 否 — 应用程序**不**存储用户输入以供后续使用  
- [ ] 是 — 输入**被**存储在数据库字段、日志文件或配置设置中  

### 在将数据写入持久化存储层之前，是否应用了输入验证或清理 (Sanitization)？
- [ ] 是 — 在入口点**应用了**严格的白名单 (Allow-list) 验证  
- [ ] 是 — **应用了**通用清理，但**可能**通过编码或非标准字符绕过  
- [ ] 否 — 输入以原始、未经验证的形式存储  

### 存储的数据在管理或辅助界面中呈现时，是否经过了正确的编码或清理？
- [ ] 是 — 在所有呈现数据的地方都**应用了**上下文感知输出编码  
- [ ] 是 — 在某些位置**应用了**编码，但在其他位置（如内部管理员仪表板）**缺失**  
- [ ] 否 — 数据在没有任何编码或清理的情况下呈现，允许执行负载 (Payload)  

### 存储在数据库中的负载 (Payload) 是否会影响后端批处理过程或带外 (Out-of-band / OOB) 监控系统？
- [ ] 否 — 后端进程使用安全的解析方法或参数化查询  
- [ ] 是 — 存储的负载**可以**在后端逻辑中触发交互（例如，CSV 注入 (CSV Injection)、日志中的命令注入 (Command Injection)）  
- [ ] 是 — 存储的负载**可以**向攻击者控制的服务器触发带外 (OOB) 请求

---

## WSTG-INPV-15 — HTTP 响应拆分与请求走私 (HTTP Splitting Smuggling)

HTTP 响应拆分 (HTTP Splitting) 和 HTTP 请求走私 (HTTP Request Smuggling) 漏洞源于前端代理 (Frontend Proxies) 和后端服务器 (Backend Servers) 在解释和处理 HTTP 请求边界时存在差异，特别是涉及 `Content-Length` 和 `Transfer-Encoding` 头部时。通过构造具有歧义的请求，攻击者可以向后端“走私”一个隐藏的请求，或者注入 CRLF 序列 (CRLF sequences) 来拆分响应，从而导致缓存投毒 (Cache Poisoning)、请求劫持 (Request Hijacking) 或安全控制绕过 (Security Control Bypass)。这些缺陷通常出现在使用反向代理 (Reverse Proxies)、负载均衡器 (Load Balancers) 或内容分发网络 (CDNs) 且解析逻辑不一致的复杂环境中。从攻击者的角度来看，这允许重定向用户流量、窃取敏感会话令牌 (Session Tokens)，以及在其他用户会话的上下文中执行未授权操作。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 高 (High) / 严重 (Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**工具：** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### 环境是否容易受到由于 CL.TE 或 TE.CL 不一致导致的请求走私攻击？
- [ ] 否 — 前端和后端服务器处理请求边界的方式一致  
- [ ] 是 — 存在不一致，但由于基础设施缓解措施，无法利用  
- [ ] 是 — 可以进行 CL.TE 或 TE.CL 走私，允许隐藏请求到达后端 *(高)*  
- [ ] 是 — 可以进行 TE.TE（双重编码/混淆）以绕过前端过滤器 *(严重)*  

### 是否可以通过头部中的 CRLF 注入实现 HTTP 响应拆分？
- [ ] 否 — 应用程序正确清理了所有头部输入中的 CRLF 序列  
- [ ] 是 — 头部中反映了 CRLF 序列，但响应拆分不可行  
- [ ] 是 — 可以进行 CRLF 注入，允许头部注入或缓存投毒  

### 是否可以使用走私请求绕过安全控制 (WAF/ACLs)？
- [ ] 否 — 安全控制同时应用于外部请求和走私请求  
- [ ] 是 — 走私请求可以绕过前端 WAF 规则或基于 IP 的 ACL  

### 是否可能劫持其他用户的会话或重定向其流量？
- [ ] 否 — 请求/响应流是隔离的，无法交叉  
- [ ] 是 — 可以进行请求走私，并允许捕获其他用户的请求 *(严重)*  
- [ ] 是 — 可以进行响应拆分，并允许浏览器端缓存投毒或跨站脚本 (XSS)

---

## WSTG-INPV-16 — HTTP 请求走私 (HTTP Request Smuggling)

HTTP 请求走私 (HTTP Request Smuggling, HRS) 发生在 HTTP 服务器链（如负载均衡器和后端 Web 服务器）对请求边界的解析不一致时，通常是由于冲突的 `Content-Length` 和 `Transfer-Encoding` 标头引起的。攻击者利用这种去同步 (Desynchronization) 现象将一个条目“走私”到后端服务器的请求缓冲区中，从而有效地在下一个合法用户的请求前添加一个由攻击者控制的分段。这种技术可以导致严重的攻击，包括会话劫持 (Session Hijacking)、绕过安全控制以及缓存投毒 (Cache Poisoning)。利用过程通常通过基于时间的分析 (Timing-based analysis) 或通过观察后续请求发送到服务器时后端返回的异常响应来进行。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **测试状态** | 未执行 |
| **严重程度** | 严重 (Critical) / 高 (High) |

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**工具：** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### 前端和后端服务器对 `Transfer-Encoding: chunked` 标头的处理是否一致？
- [ ] 是 — 两台服务器对分块编码的处理完全相同，去同步**不可行**  
- [ ] 否 — 服务器对标头的解析不同，但已应用清理/规范化 (Sanitization/Normalization)  
- [ ] 否 — CL.TE (Content-Length/Transfer-Encoding) 去同步**可行** *(严重)*  
- [ ] 否 — TE.CL (Transfer-Encoding/Content-Length) 去同步**可行** *(严重)*  

### 攻击者是否可以使用走私的请求对服务器或客户端缓存进行投毒？
- [ ] 否 — 缓存已**禁用**或不易受到投毒攻击  
- [ ] 是 — 走私的请求**可以**通过缓存投毒将用户重定向到恶意域名或注入脚本  

### 是否可以通过请求拼接捕获其他用户的请求或会话令牌 (Session Tokens)？
- [ ] 否 — 走私**不**会导致跨用户数据泄露  
- [ ] 是 — 走私允许将下一个用户的请求附加到攻击者控制的 `POST` 正文中，使数据外泄 (Exfiltration) **可行**  

### 基础设施是否使用 HTTP/2，并且是否容易受到 H2.CL 或 H2.TE 去同步的影响？
- [ ] 否 — 应用程序仅使用 HTTP/1.1，或者 HTTP/2 的实现是安全的且没有降级  
- [ ] 是 — 发生了 HTTP/2 到 HTTP/1.1 的明文降级 (Cleartext downgrades)，且去同步**可行**  

### 畸形标头 (Malformed headers)（例如：带空格的 `Transfer-Encoding:  chunked`）是否得到了安全处理？
- [ ] 是 — 畸形标头在所有层级均被拒绝或规范化  
- [ ] 否 — 由于对畸形或混淆标头 (Obscured headers) 的处理不一致，TE.TE 去同步**可行**

---

## WSTG-INPV-17 — 主机头注入测试 (Testing for Host Header Injection)

主机头注入 (Host Header Injection) 发生在应用程序信任用户提供的 `Host` 标头并将其纳入服务器端逻辑（如链接生成或重定向）且缺乏充分验证时。攻击者通过操纵此标头来促进网络缓存投毒 (Web Cache Poisoning)，或通过将敏感令牌重定向到外部域来实现密码重置投毒 (Password Reset Poisoning)，或绕过依赖于标头路由的安全控制。成功利用该漏洞允许攻击者窃取会话数据、执行账户接管 (Account Takeover) 或将不知情的用户重定向到恶意基础设施。此漏洞最常见于负载均衡环境或根据传入请求上下文动态生成绝对 URL 的应用程序中。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **测试状态** | 未执行 |
| **严重性** | 中 / 高* |

> *如果主机头被用于在电子邮件中生成敏感链接（如密码重置），则严重性变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**工具：** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### 应用程序是否在链接、重定向或脚本中回显了 `Host` 标头的值？
- [ ] 否 — 应用程序使用硬编码的基础 URL 或经过严格验证的配置  
- [ ] 是 — `Host` 标头被回显，但针对白名单进行了严格验证  
- [ ] 是 — `Host` 标头被回显，且**可以**通过畸形值（例如 `expected.com:attacker.com`）进行绕过  
- [ ] 是 — `Host` 标头在**没有任何**验证的情况下被回显  

### 应用程序或基础设施是否处理替代的主机相关标头 (Alternative host-related headers)？
- [ ] 否 — 类似于 `X-Forwarded-Host`、`X-Host` 或 `Forwarded` 的标头被忽略  
- [ ] 是 — 处理了替代标头，但**不会**覆盖内部逻辑  
- [ ] 是 — 替代标头**可以**用于覆盖主要的 `Host` 标头值  

### 能否操纵 `Host` 标头来投毒系统生成的电子邮件（例如密码重置）？
- [ ] 否 — 电子邮件链接使用服务器配置中静态、可信的域名  
- [ ] 是 — `Host` 标头影响电子邮件链接，但验证**无法**被绕过  
- [ ] 是 — `Host` 标头**可以**被操纵，从而将密码重置令牌重定向到攻击者控制的域 *(严重)*  

### 应用程序是否容易受到通过 `Host` 标头进行的网络缓存投毒 (Web Cache Poisoning)？
- [ ] 否 — 不存在缓存机制，或者 `Host` 标头是缓存键 (Cache Key) 的一部分  
- [ ] 是 — 存在缓存，但它针对未包含在键中的输入 (Unkeyed inputs) 严格验证或忽略了 `Host` 标头  
- [ ] 是 — 存在缓存，且**可以**通过注入的 `Host` 标头进行投毒，从而向其他用户提供恶意内容  

### 服务器是否接受用于虚拟主机 (Virtual Host) 路由的任意 `Host` 标头？
- [ ] 否 — 服务器对无法识别的 `Host` 标头返回错误或默认页面  
- [ ] 是 — 服务器接受任何 `Host` 标头，这对于侦察 (Reconnaissance) 或内部服务枚举 (Internal service enumeration) 很有用

---

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

## WSTG-INPV-19 — 服务器端请求伪造 (SSRF) 测试

服务器端请求伪造 (Server-Side Request Forgery, SSRF) 发生在攻击者能够诱导 Web 应用程序向内部或外部资源发出未经授权的请求时。通过操纵预期接收 URL、主机名或 IP 地址的参数，攻击者可以绕过防火墙和访问控制列表 (ACL) 等网络层控制，以探测内部网络段、访问云元数据服务 (IMDS) 或与易受攻击的内部 API 和数据库进行交互。这种漏洞通常出现在涉及图像抓取、PDF 生成、Webhooks 或通过 URL 进行文件上传的功能中。从攻击者的角度来看，SSRF 是一个强大的原语 (Primitive)，用于跳转到隔离环境、窃取敏感配置数据，或通过与 Redis 或 Memcached 等内部服务交互来潜在地实现远程代码执行 (Remote Code Execution)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**工具：** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### 应用程序是否处理用户提供的 URL 或主机名？
- [ ] 否 — 没有接收外部 URL 或主机名的功能  
- [ ] 是 — 存在该功能，但输入根据白名单 (Allow-list) **经过严格验证**  
- [ ] 是 — 存在该功能且接收用户提供的 URL  

### 是否对请求的目标应用了验证控制或黑名单？
- [ ] 是 — 强制执行受信任域名/IP 的严格**白名单** (Allow-list)  
- [ ] 是 — 使用了**黑名单** (Deny-list)（例如，封禁 `127.0.0.1`, `169.254.169.254`）  
- [ ] 否 — **未对输入进行验证**或过滤  

### 是否可以通过编码、重定向或 DNS 重绑定绕过过滤器？
- [ ] 否 — 过滤器健壮，并能安全地处理重定向/DNS 解析  
- [ ] 是 — 使用 URL 编码或替代 IP 格式（十六进制、八进制）**可以绕过**  
- [ ] 是 — 通过指向内部目标的 3xx HTTP 重定向 (Redirects) **可以绕过**  
- [ ] 是 — 通过 DNS 重绑定 (DNS Rebinding) 攻击解析为内部 IP **可以绕过**  

### 应用程序能否访问内部元数据服务或本地接口？
- [ ] 否 — 本地网络和云元数据端点**不可达**  
- [ ] 是 — **可以访问**本地主机 (`127.0.0.1`) 或回环 (Loopback) 服务  
- [ ] 是 — **可以访问**云元数据服务（例如 AWS/GCP/Azure IMDS）*(严重)*  

### SSRF 响应的性质是什么？
- [ ] 全盲 (Blind) — 不向用户返回任何响应或元数据  
- [ ] 半盲 (Partial) — 向用户返回响应元数据（响应头、大小、时间耗时）  
- [ ] 完全 (Full) — 内部资源的完整响应体**被反射**给用户

---

## WSTG-INPV-20 — 批量赋值 (Mass Assignment)

批量赋值 (Mass Assignment)，也称为过度提交 (Overposting) 或参数的不安全反序列化 (Insecure Deserialization of parameters)，发生在 Web 应用程序在没有进行适当属性过滤的情况下，将传入的 HTTP 请求参数自动绑定到内部对象属性时。这种漏洞在现代 MVC 框架和 RESTful API 中非常普遍，其中 JSON、XML 或表单编码的数据被直接反序列化为应用程序端的数据模型。攻击者通过在请求中注入额外的、未列出的参数（例如 `isAdmin`、`role` 或 `account_balance`）来利用此行为，从而修改开发人员本意不希望暴露给用户控制的敏感字段。成功利用此漏洞通常会导致权限提升 (Privilege Escalation)、未授权的数据修改或绕过关键业务逻辑工作流。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 中* |

> *如果可以修改管理属性、权限级别或计费字段，则严重程度为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### 应用程序是否对传入的请求参数使用自动绑定？
- [ ] 否 — 参数被手动映射到特定的变量或安全对象  
- [ ] 是 — 使用了自动绑定，但强制执行了严格的**白名单**或数据传输对象 (DTO)  
- [ ] 是 — 使用了自动绑定，且敏感的内部属性**可以**被修改  

### 是否可以成功注入管理或权限相关的属性？
- [ ] 否 — 注入的参数被忽略、剥离或导致验证错误  
- [ ] 是 — 额外的参数被接受，但**不会**影响后端对象状态  
- [ ] 是 — 注入诸如 `is_admin`、`role` 或 `permissions` 等参数并**成功**提升权限 *(危急)*  

### 是否可以通过过度提交修改敏感的业务逻辑或财务字段？
- [ ] 否 — `price`、`balance` 或 `status` 等字段已受到保护，防止批量赋值  
- [ ] 是 — 通过将敏感业务属性添加到 JSON 或表单请求体中，**可以**对其进行修改  

### 应用程序是否泄露了有助于参数猜测的内部对象结构？
- [ ] 否 — 响应仅包含预期的公共字段，且文档受到限制  
- [ ] 是 — 内部对象字段在 GET 响应中可见，使参数发现成为**可能**  
- [ ] 是 — API 文档（Swagger/OpenAPI）明确列出了**可以**被赋值的内部属性

---

## WSTG-ERRH-01 — 不当错误处理测试 (Testing for Improper Error Handling)

当 Web 应用程序通过其错误响应泄露敏感技术细节（如堆栈跟踪 (Stack Traces)、数据库架构信息或内部文件路径）时，就会发生不当错误处理 (Improper Error Handling)。攻击者通过提交畸形输入、访问不存在的资源或诱导服务端异常来触发这些错误，从而映射应用程序的底层架构并识别潜在的进一步利用向量。这种信息泄露 (Information Leakage) 通常是更严重攻击的前奏，通过向攻击者提供精确的环境规范，辅助其进行 SQL 注入 (SQL Injection) 或路径遍历 (Path Traversal) 等攻击。安全的实现方式必须提供通用的用户友好消息，同时仅在安全的服务端日志中记录详细的诊断信息。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 低 / 中 (Low / Medium) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### 应用程序是否针对未处理的异常使用了通用的全局错误处理器？
- [ ] 是 — 通用错误页面已**启用**且**未泄露**技术细节  
- [ ] 是 — 使用了自定义错误页面，但**可能**通过特定 HTTP 头泄露信息  
- [ ] 否 — 默认服务器错误页面（例如 Tomcat, IIS, Nginx）**可见**  

### 是否可以通过畸形输入或边界情况枚举技术信息？
- [ ] 否 — 错误得到了优雅处理，并带有用于日志记录的唯一引用 ID  
- [ ] 是 — 响应中**泄露**了堆栈跟踪或数据库查询语句  
- [ ] 是 — 响应中**泄露**了内部文件路径、环境变量或服务器版本字符串  

### API 响应是否泄露了详细的错误对象或调试信息？
- [ ] 否 — API 返回标准化的错误代码和经过清理的 JSON 消息  
- [ ] 是 — API 在响应体中返回了详细的调试对象或完整的异常细节  
- [ ] 是 — JSON 响应的 `details` 或 `exception` 字段中包含了堆栈跟踪  

### 当错误发生时，应用程序的行为是否存在差异（基于时间或基于内容）？
- [ ] 否 — 无论错误类型如何，响应特征和时间消耗都是一致的  
- [ ] 是 — 可以通过差异化响应来枚举有效与无效的状态（例如：用户枚举 (User Enumeration)）

---

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

## WSTG-CRYP-01 — 弱传输层安全测试 (Testing for Weak Transport Layer Security)

弱传输层安全测试涉及分析 SSL/TLS 的实现和配置，以确保数据在传输过程中的机密性 (Confidentiality) 和完整性 (Integrity)。攻击者利用过时协议（如 SSLv2, TLS 1.0）、弱密码套件 (Cipher Suites，如 NULL、RC4 或匿名套件) 以及无效或签名强度弱的证书等配置错误来执行中间人 (Man-in-the-Middle, MitM) 攻击。此项测试通常针对建立加密通信的应用程序 Web 服务器或负载均衡器 (Load Balancer) 端点。成功的利用可使攻击者拦截敏感数据、劫持用户会话，或通过协议降级 (Protocol Downgrade) 或流量解密将连接降级至不安全状态。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 中* |

> *如果传输敏感数据（个人身份信息 (PII)、凭据、会话令牌 (Session Tokens)），严重程度为“高”；对于一般性信息流量，严重程度为“中”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### 是否支持过时或不安全的 TLS/SSL 协议？
- [ ] 否 — 仅**启用**了 TLS 1.2 和 TLS 1.3  
- [ ] 是 — **启用**了 TLS 1.0 或 TLS 1.1  
- [ ] 是 — **启用**了 SSLv2 或 SSLv3 *(危急)*  

### 服务器是否接受弱密码套件或不安全的密码套件？
- [ ] 否 — 仅**支持**强效、现代的算法（如 AES-GCM, ChaCha20）  
- [ ] 是 — **支持**弱加密算法（如 3DES, RC4）或低位长度的算法  
- [ ] 是 — **支持** NULL、匿名 (ADH) 或出口级 (Export) 密码套件 *(危急)*  

### 服务器证书是否有效且受信任？
- [ ] 是 — 证书有效，与域名匹配，且由**受信任的 CA** 签名  
- [ ] 否 — 证书已**过期**或使用了**弱签名算法**（如 SHA-1）  
- [ ] 否 — 证书为**自签名**或存在**域名不匹配**  

### 服务器是否支持安全重新协商 (Secure Renegotiation) 并防止降级攻击？
- [ ] 是 — **支持**安全重新协商且**启用**了 TLS Fallback SCSV  
- [ ] 否 — **不支持**安全重新协商，可能允许 MitM 注入  
- [ ] 否 — 服务器易受 **POODLE** 或 **ROBOT** 攻击  

### 是否正确实现了 HTTP 强制传输安全 (HTTP Strict Transport Security, HSTS)？

| 标头 | 存在 | 缺失 |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] 是 — 存在 HSTS 标头，具有较长的 `max-age` 且**启用**了 `includeSubDomains`  
- [ ] 是 — 存在 HSTS 标头但缺失 `includeSubDomains` 或 `max-age` **过短**  
- [ ] 否 — **不存在** HSTS 标头

---

## WSTG-CRYP-02 — Testing for Padding Oracle (填充预言机测试)

填充预言机 (Padding Oracle) 漏洞发生在应用程序的解密过程中泄露了关于解密密文的填充 (Padding) 是否有效的信息。通过观察这些侧信道 (Side-channel) 响应——如不同的 HTTP 状态码、特定的错误消息或响应时间差异——攻击者可以在不知道加密密钥的情况下解密数据，并可能重新加密任意明文。这种漏洞通常出现在使用密码块链接 (CBC) 模式和 PKCS#7 填充的分组密码 (Block Cipher) 的加密会话 Cookie、身份验证令牌 (Authentication Token) 或 URL 参数中。利用此类漏洞会导致机密性 (Confidentiality) 完全丧失，并可能通过构造伪造的加密块导致完整性 (Integrity) 受损。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 高 / 关键* (High / Critical*) |

> *如果漏洞存在于会话管理 (Session Management)、身份令牌或 PII 加密中，严重程度为“关键”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### 敏感数据是否使用了 CBC 模式的分组密码？
- [ ] 否 —— 应用程序使用了认证加密 (Authenticated Encryption) (AES-GCM/ChaCha20-Poly1305)  
- [ ] 是 —— 应用程序使用了分组密码，但**不需要**填充 (例如流密码 (Stream Cipher) 或 CTR 模式)  
- [ ] 是 —— 应用程序使用了 CBC 模式且**应用了**填充  

### 服务器是否通过侧信道泄露了填充的有效性？
- [ ] 否 —— 服务器返回统一的错误消息和一致的响应时间  
- [ ] 是 —— 服务器针对填充错误返回不同的 HTTP 状态码 (例如 200 与 500)  
- [ ] 是 —— 服务器返回特定的错误消息 (例如 "Invalid Padding"、"Decryption failed")  
- [ ] 是 —— 服务器在解密失败期间表现出可测量的响应时间差异  

### 是否可以对密文进行自动化解密？
- [ ] 否 —— 侧信道不够稳定，无法利用  
- [ ] 是 —— 密文**可以**被部分解密 (最后几个块)  
- [ ] 是 —— 使用 `PadBuster` 等工具**可以实现**完整的明文恢复  

### 攻击者是否可以执行位翻转或伪造有效的密文？
- [ ] 否 —— 完整性检查 (HMAC) 在解密**之前**已完成验证  
- [ ] 是 —— 不存在完整性检查，但载荷 (Payload) 构造不可行  
- [ ] 是 —— **可以**构造任意密文，从而导致权限提升 (Privilege Escalation) 或数据注入 (Data Injection)

---

## WSTG-CRYP-03 — 测试通过未加密通道传输的敏感信息

通过明文 HTTP 等未加密通道传输敏感数据，会使信息面临通过中间人攻击 (Man-in-the-Middle (MITM)) 被拦截和修改的风险。当 Web 应用程序未能跨所有端点强制执行传输层安全协议 (Transport Layer Security (TLS)) 时，通常会出现此类漏洞，特别是在遗留组件、登录表单或从 HTTP 到 HTTPS 的初始过渡期间。从攻击者的角度来看，这允许在受损或不可信的网络段（如公共 Wi-Fi）上对身份验证凭据、会话令牌 (Session Token) 和个人可识别信息 (Personally Identifiable Information (PII)) 进行被动嗅探。确保所有敏感数据都封装在强大的 TLS 隧道内，对于维护用户交互的机密性和完整性以及防止会话劫持 (Session Hijacking) 至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 中 |

> *如果身份验证凭据、会话标识符或 PII 通过明文传输，严重程度将变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### 应用程序是否允许通过未加密的 HTTP 与敏感端点进行通信？
- [ ] 否 — 所有应用程序流量都严格强制通过 HTTPS *(最安全)*  
- [ ] 是 — 非敏感页面可以通过 HTTP 访问，但敏感端点**不可以**  
- [ ] 是 — 敏感端点（登录、结账、个人资料）**可以**通过未加密的 HTTP 访问 *(高风险)*  

### 是否存在从 HTTP 到 HTTPS 的全局重定向？
- [ ] 是 — 应用程序对所有 HTTP 请求执行 301/302 重定向到 HTTPS  
- [ ] 是 — 仅针对特定的敏感端点执行重定向  
- [ ] 否 — **未应用**从 HTTP 到 HTTPS 的重定向  

### 是否实现了 HTTP 严格传输安全 (HTTP Strict Transport Security (HSTS))？
- [ ] 是 — HSTS 已**启用**，具有较长的 `max-age` 且包含 `includeSubDomains` 和 `preload` 指令  
- [ ] 是 — HSTS 已**启用**，但缺少 `includeSubDomains` 或 `preload` 指令  
- [ ] 否 — 应用程序服务器上**未启用** HSTS  

### 敏感 Cookie 是否已防止明文传输？

| Cookie 名称 | 存在 Secure 标志 | 缺失 Secure 标志 |
|-------------|:-------------------:|:------------------:|
| `SessionID` |                     |                    |
| `AuthToken` |                     |                    |
| `CSRF-Token`|                     |                    |

### 应用程序是否通过未加密通道向第三方 API 传输敏感数据？
- [ ] 否 — 所有外部 API 调用均通过加密 (HTTPS) 隧道执行  
- [ ] 是 — 一些非敏感的遥测数据通过 HTTP 发送  
- [ ] 是 — API 密钥、PII 或凭据**已**通过未加密的 HTTP 发送到第三方

---

## WSTG-CRYP-04 — 弱加密原语测试 (Testing for Weak Cryptographic Primitives)

弱加密算法原语 (Weak Cryptographic Primitives) 涉及使用过时或不安全的算法，例如 MD5、SHA-1、DES 或 RC4，这些算法容易受到碰撞攻击 (Collision Attacks)、频率分析或暴力破解 (Brute Force) 解密。这些弱点通常出现在应用程序后端的密码哈希 (Hashing)、静态数据加密、会话令牌 (Session Token) 生成或数字签名验证中。从攻击者的角度来看，识别出这些原语可以破坏数据的完整性和机密性，例如通过破解拦截到的哈希以获取明文密码或伪造身份验证令牌。现代应用程序应转而使用符合行业标准、抗碰撞且高熵的原语，如 Argon2、Bcrypt 或 AES-GCM，以确保针对密码分析 (Cryptanalysis) 的强大防护。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### 敏感数据存储是否使用了诸如 MD5 或 SHA-1 等已弃用的哈希算法？
- [ ] 否 — 应用程序使用 Argon2 或 Bcrypt 等现代抗碰撞算法  
- [ ] 是 — 使用了已弃用的算法，但**仅**用于非安全敏感的完整性检查  
- [ ] 是 — 密码或敏感令牌**使用了**已弃用的算法 *(高)*  

### 当前是否采用了 DES、3DES 或 RC4 等弱对称加密密码？
- [ ] 否 — 使用了 GCM 或 CBC 等安全模式下的 AES (128位或更高)  
- [ ] 是 — 为了向后兼容**启用了**遗留密码，但它们**不是**默认设置  
- [ ] 是 — 遗留密码是保护静态或传输中数据的主要方法  

### 应用程序是否实现了“自行开发” (Roll-your-own) 或非标准的加密逻辑？
- [ ] 否 — 应用程序严格遵守标准的、经过同行评审的加密库  
- [ ] 是 — 存在自定义封装，但底层原语**属于**行业标准  
- [ ] 是 — 对敏感数据**应用了**私有加密算法或自定义逻辑 *(严重)*  

### 加密盐 (Salts) 和初始化向量 (IVs) 是否按照最佳实践进行管理？
- [ ] 是 — 盐对每个用户都是唯一的，且每次操作的 IV 均不可预测  
- [ ] 是 — 使用了盐，但在所有记录中并**不唯一**  
- [ ] 否 — 盐是静态的、硬编码的或缺失的，且 IV 在多个会话中被**复用**  

### 非对称密钥 (RSA, Diffie-Hellman) 的位长度是否符合现代标准？
- [ ] 是 — RSA/DH 密钥为 2048 位或更高，或者使用了椭圆曲线加密 (ECC)  
- [ ] 否 — 密钥小于 2048 位，但绕过并非易事  
- [ ] 否 — 密钥为 1024 位或更小，使其**容易受到**因子分解或计算攻击的影响

---

## WSTG-BUSL-01 — 测试业务逻辑数据验证 (Test Business Logic Data Validation)

业务逻辑数据验证 (Business Logic Data Validation) 测试的重点是应用程序识别和拒绝在业务领域上下文中虽然语法正确但逻辑无效的数据的能力。攻击者通过提交非预期的值——如购物车中的负数数量、未来日期事件的过去日期或极大整数——来绕过约束并操纵应用程序的状态。这些漏洞通常存在于多步工作流 (Multi-step workflows)、结账流程和账户管理模块中，在这些模块中，服务端逻辑未能验证用户提交数据的上下文完整性。成功利用 (Exploit) 可能会导致经济损失、完整性受损以及对受限功能或数据的未经授权访问。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**工具：** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### 应用程序是否对数值输入实施了逻辑范围限制？
- [ ] 是 — 应用程序正确拒绝了不合适的负值、零或过大的数值 *(最安全)*  
- [ ] 是 — 应用程序拒绝了某些无效范围，但仍容易受到整数溢出 (Integer Overflow) 或边界绕过的影响  
- [ ] 否 — 应用程序接受任何数值，无论其业务上下文如何 *(危急)*  

### 服务端验证检查在应用程序的所有层中是否一致？
- [ ] 是 — 验证一致地应用于 API/服务层和数据库层  
- [ ] 是 — 验证已应用于 UI/前端，但可以通过直接 API 请求进行绕过  
- [ ] 否 — 未应用验证或验证不一致，导致逻辑数据损坏  

### 是否可以通过操纵多步工作流中的数据来破坏业务逻辑？
- [ ] 否 — 状态在服务器上安全维护，且前一步骤的数据不可被篡改  
- [ ] 是 — 验证发生在早期步骤中，但最终提交时未重新验证数据的完整性  
- [ ] 是 — 可以通过跳过步骤或提交乱序数据来实现工作流绕过  

### 应用程序是否妥善处理了绕过业务约束的“边缘情况 (Edge Case)”数据？
- [ ] 是 — 约束稳固，能够处理异常但语法有效的输入（例如：闰年、时区偏移）  
- [ ] 是 — 处理了某些边缘情况，但特定组合可能导致非预期行为  
- [ ] 否 — 边缘情况直接导致逻辑失败，例如免费购买或未经授权的状态变更

---

## WSTG-BUSL-02 — 测试伪造请求的能力 (Test Ability to Forge Requests)

在业务逻辑 (Business Logic) 上下文中伪造请求涉及操作应用程序定义的参数，以执行超出预期工作流或授权级别的操作。攻击者针对多步骤过程（如结账系统或账户注册），其中状态通过客户端参数维护，而服务器未能根据受信任的后端来源重新验证这些参数。通过更改隐藏字段、JSON 值或 REST 参数（如价格、数量或内部状态标志），攻击者可以绕过支付要求、提升权限 (Privilege Escalation) 或触发非预期的状态更改。这种漏洞通常出现在有状态的 Web 表单或 API 中，其中服务器在交易过程中隐式信任客户端对业务状态的表示。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重* |

> *如果伪造的请求导致经济损失、未经授权的管理操作或完全的账户接管 (Account Takeover)，则严重程度变为“严重”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### 应用程序是否根据服务器端的“单一事实来源”(Source of Truth) 验证交易参数？
- [ ] 是 — 所有敏感参数（价格、角色、ID）都根据数据库进行验证，且无法绕过  
- [ ] 是 — 应用了验证，但可以通过参数污染 (Parameter Pollution) 或备选编码绕过  
- [ ] 否 — 应用程序信任客户端值来确定交易成本或结果 *(严重)*  

### 业务关键值（如数量、金额）是否可以修改为未经授权的值？
- [ ] 否 — 负值、零值或过高的值会被服务器拒绝  
- [ ] 是 — 服务器接受修改后的值，但仅限于有限范围内  
- [ ] 是 — 可以提交负值或零值，导致经济损失或逻辑绕过  

### 是否使用隐藏字段或 API 参数在多步骤过程中维护状态？
- [ ] 否 — 状态通过服务器端会话 (Session) 或签名令牌 (Token) 安全地维护  
- [ ] 是 — 状态通过客户端传递，但完整性检查（HMAC/签名 (Signatures)）已启用且健全  
- [ ] 是 — 状态通过客户端传递，且完整性检查被禁用或可以被剥离  

### 是否可以伪造请求以跳过工作流中的强制性中间步骤？
- [ ] 否 — 服务器对每项交易执行严格的操作顺序  
- [ ] 是 — 步骤可以跳过，但最终操作仍需要来自先前步骤的有效数据  
- [ ] 是 — 可以直接伪造最终的“提交”或“完成”请求，以绕过授权 (Authorization) 或支付步骤

---

## WSTG-BUSL-03 — 完整性检查测试 (Test Integrity Checks)

完整性检查测试 (Integrity Check Testing) 侧重于验证应用程序是否确保数据在流经各种业务流程或在客户端与服务器之间传输时，保持未经篡改且可信。攻击者通过篡改 HTTP 请求 (HTTP Requests)、隐藏字段或 cookies 中的关键参数（如产品价格、数量、用户权限或交易状态）来攻击此类逻辑。如果应用程序缺乏服务端校验 (Server-side verification) 或强健的完整性控制机制，如哈希 (Hashing) 或 HMAC (Hash-based Message Authentication Codes)，攻击者就可以操纵这些值以实施欺诈、提升自身权限 (Privilege Escalation) 或绕过预设的业务约束。对于任何处理金融交易、敏感状态转换或输出作为下一阶段可信输入的多步骤工作流应用程序，此项测试都至关重要。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果完整性失效导致资金窃取、未经授权的权限提升或持久记录的修改，严重程度将变为高。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### 敏感业务参数是否暴露于客户端篡改？
- [ ] 否 — 敏感值仅在服务端进行管理  
- [ ] 是 — 价格、数量或状态标识等值**已**暴露，但经过加密  
- [ ] 是 — 值以明文形式暴露在隐藏字段、URL 参数或 cookies 中  

### 客户端控制的参数是否应用了完整性机制（例如 HMAC、校验和）？
- [ ] 是 — **已应用**强健的完整性检查，并在每次请求时进行验证  
- [ ] 是 — **已应用**完整性检查，但使用了弱/可预测的算法或硬编码的密钥  
- [ ] 否 — 未对敏感参数**应用**完整性检查  

### 攻击者能否绕过或重新计算完整性检查？
- [ ] 否 — 签名验证被严格执行，绕过 (Bypass) **不可行**  
- [ ] 是 — 可以通过省略签名参数来绕过签名验证  
- [ ] 是 — 签名**可以**被重新计算，因为密钥或逻辑已泄露/强度不足  

### 应用程序是否针对可信源执行数据的后端重新验证？
- [ ] 是 — 服务端针对数据库重新验证所有值，绕过**不可行**  
- [ ] 是 — 服务端执行部分验证，但在特定边缘案例下绕过**可行**  
- [ ] 否 — 服务端直接信任客户端提供的数据，未进行后端验证 *（严重）*  

### 是否可以篡改多步骤流程的状态？
- [ ] 否 — 会话状态 (Session State) 在服务端管理，无法操纵  
- [ ] 是 — 状态信息通过客户端传递，**可以**通过篡改来跳过步骤或重复操作

---

## WSTG-BUSL-04 — 流程耗时测试 (Test for Process Timing)

流程耗时攻击 (Process Timing Attacks) 涉及测量应用程序处理特定请求所需的时间，从而推断有关内部状态或数据是否存在等敏感信息。通过分析响应时间中毫秒级的差异（通常由提前退出条件逻辑、数据库查询或非恒定时间加密比较引起），攻击者可以进行侧信道分析 (Side-channel Analysis)，以枚举有效用户名、验证密码片段或确认特定记录的存在。这些漏洞通常出现在身份验证端点 (Authentication Endpoints)、密码重置表单和基于输入有效性进行后端逻辑分支的资源密集型 API 过滤器中。从攻击者的角度来看，即使应用程序向用户返回完全相同的通用错误消息，这些耗时差异也可以充当“布尔预言机 (Boolean Oracle)”，揭示后端数据的真实情况。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **测试状态** | 未执行 |
| **严重程度** | 中 (Medium) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**工具：** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### 应用程序对于有效与无效的资源请求是否表现出不同的响应时间？
- [ ] 否 — 无论输入是否有效，响应时间均完全一致  
- [ ] 是 — 存在可忽略的耗时差异，但在统计学上**不**显著  
- [ ] 是 — 存在可**观察**到的持续耗时差异，并提供了一个可靠的预言机  

### 是否实现了恒定时间比较函数或人工延迟以规范化响应时间？
- [ ] 是 — 对所有敏感比较操作均**应用**了恒定时间 (Constant-time) 算法 *(最安全)*  
- [ ] 是 — **启用**了人工延迟或抖动 (Jitter)，但底层逻辑仍为非恒定时间  
- [ ] 否 — 未**应用**任何耗时缓解措施，允许直接测量逻辑分支  

### 攻击者能否使用统计分析从网络噪音中分离出耗时信号？
- [ ] 否 — 网络抖动 (Network Jitter) 和应用程序噪音导致耗时分析**无法实现**  
- [ ] 是 — 在样本量足够的情况下，可以从噪音中**分离**出耗时信号  
- [ ] 是 — 耗时差值 (Timing Delta) 足够大，**不**需要进行统计规范化  

### 是否可以通过登录或密码重置功能进行账号枚举 (Account Enumeration)？
- [ ] 否 — 无法通过耗时确定账户是否存在  
- [ ] 是 — 耗时差异允许远程攻击者**枚举**有效的用户名或电子邮件  

### 资源密集型操作（如文件处理、复杂查询）是否通过耗时泄露数据是否存在？
- [ ] 否 — 无论是否找到记录，资源消耗均保持一致  
- [ ] 是 — 通过测量后端处理时长，**可以**推断出敏感记录的存在

---

## WSTG-BUSL-05 — 测试功能使用次数限制 (Test Number of Times a Function Can Be Used Limits)

此项测试旨在评估应用程序是否针对单个用户、会话 (Session) 或 IP 地址，就特定操作或功能的执行频率及总次数强制执行业务逻辑约束 (Business Logic Constraints)。如果未能实现这些限制，攻击者可能会滥用高价值功能——例如发送短信一次性密码 (SMS OTP)、触发密码重置邮件、使用优惠码或执行海量数据导出——从而导致经济损失、资源耗尽 (Resource Exhaustion) 或声誉损害。这些漏洞通常存在于交易端点或通信功能中，攻击者通过自动化脚本重复执行操作，远远超出预期的业务阈值。从攻击者的角度来看，这是短信轰炸 (SMS Pumping)、邮件炸弹 (Mail Bombing) 或针对缺乏明确速率限制 (Rate Limiting) 或计数节流的工作流进行暴力破解 (Brute-forcing) 的首选目标。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **测试状态** | 未执行 (Not Performed) |
| **严重程度** | 中 / 高* |

> *如果功能涉及金融交易、短信成本或影响全系统的可用性，严重程度将变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### 应用程序是否定义并强制执行敏感业务功能的限制？
- [ ] 是 — 限制被严格执行且**无法**绕过  
- [ ] 是 — 强制执行了限制，但阈值过高，无法防止滥用  
- [ ] 否 — 未对功能执行应用任何限制  

### 速率限制和使用计数器是否在服务端强制执行？
- [ ] 是 — 强制执行严格位于服务端 (Server-side) 且**不可**被操纵  
- [ ] 否 — 强制执行依赖于客户端逻辑 (Client-side Logic)（例如 JavaScript 或隐藏字段）  
- [ ] 否 — 任何层级均未实施强制执行  

### 是否可以通过请求头或会话篡改绕过使用限制？
- [ ] 否 — **无法**通过 `X-Forwarded-For`、`Origin` 或会话轮换进行绕过  
- [ ] 是 — **可以**通过伪造 IP 请求头或清除 Cookie 来绕过限制  
- [ ] 是 — **可以**通过创建多个账号或会话来绕过限制  

### 超出限制时是否触发了适当的防御性响应？
- [ ] 是 — 应用程序返回 `429 Too Many Requests` 并记录该事件 *(最安全)*  
- [ ] 是 — 应用程序返回错误，但**未**记录潜在的滥用行为  
- [ ] 否 — 应用程序继续处理请求但忽略输出结果  
- [ ] 否 — 应用程序不提供任何响应或在负载下崩溃  

### 滥用该功能对业务的影响是否重大？
- [ ] 否 — 功能滥用产生的成本或资源影响微不足道  
- [ ] 是 — 滥用**可能**导致轻微的资源消耗或用户困扰  
- [ ] 是 — 滥用**可能**导致重大的经济损失（例如短信费用）或拒绝服务 (Service Denial) *(关键)*

---

## WSTG-BUSL-06 — 业务工作流绕过测试 (Testing for the Circumvention of Work Flows)

工作流绕过 (Workflow circumvention) 涉及篡改应用程序逻辑，以绕过多步流程中预定的序列或前提条件。攻击者会识别敏感的端点 (Endpoints)——例如支付确认、管理审批或多因素身份验证 (MFA) 完成页面——并尝试直接访问它们，从而跳过强制性的中间步骤。当服务端逻辑 (Server-side logic) 未能维持稳健的状态机 (State machine)，而是依赖于用户会遵循 UI 驱动路径的假设时，就会出现此漏洞。成功利用该漏洞可能导致严重的业务逻辑缺陷 (Business logic failures)，包括未经授权的交易、权限提升 (Privilege Escalation) 以及身份验证机制的绕过。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **测试状态** | 未执行 |
| **严重性** | 中 / 高* |

> *如果绕过行为允许未经授权的金融交易、权限提升或管理访问，则严重性变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**工具：** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### 应用程序是否包含多阶段业务流程？
- [ ] 否 — 应用程序仅由单次请求操作组成  
- [ ] 是 — 存在多阶段流程（例如：购物车、向导表单、注册）  

### 服务器是否严格强制执行步骤顺序？
- [ ] 是 — 服务端状态追踪确保步骤**无法**被跳过  
- [ ] 是 — 客户端控制暗示了某种顺序，但服务器**未**验证进度  
- [ ] 否 — 应用程序**未**强制执行任何特定的操作顺序  

### 攻击者能否通过直接请求终端 URL 或端点来到达流程的最终阶段？
- [ ] 否 — 在未完成先前步骤的情况下，**无法**直接访问终端步骤  
- [ ] 是 — 终端步骤（例如：`/api/v1/checkout/complete`）**可以**通过直接请求到达  

### 应用程序是否验证先前步骤的前提数据是否存在且有效？
- [ ] 是 — 服务器在继续操作前验证所有先前步骤的数据  
- [ ] 否 — 服务器在预期数据但未进行验证的情况下，**易受**“跳过”步骤攻击的影响  

### 是否可以通过操纵工作流来绕过多因素身份验证 (MFA) 或身份验证等安全控制？
- [ ] 否 — 关键控制**无法**通过流程操纵绕过  
- [ ] 是 — 通过跳转到验证后的步骤，**可能**绕过 MFA 或身份检查

---

## WSTG-BUSL-07 — 测试针对应用程序误用的防御机制

针对应用程序误用的防御评估旨在衡量应用程序识别、记录并响应偏离预期业务逻辑 (Business Logic) 的异常用户行为的能力。与传统的基于特征 (Signature-based) 的安全性不同，这些防御机制侧重于检测表明自动化滥用的模式，例如快速连续请求、凭据填充 (Credential Stuffing) 或顺序资源枚举。从攻击者的角度来看，此测试确定了应用程序对自动化以及旨在泄露数据或耗尽资源而不触发标准安全警报的“低速且缓慢”攻击的韧性。未能实施这些防御措施通常会导致大规模数据抓取、账户接管 (Account Takeover) 或通过合法但资源密集型的应用程序功能导致拒绝服务。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### 是否在敏感端点上强制执行了速率限制 (Rate Limiting) 或流量削减 (Throttling) 机制？
- [ ] 是 — 在所有敏感端点上都**应用**并强制执行了严格的速率限制 *(最安全)*  
- [ ] 是 — **应用**了速率限制，但可以通过 IP 轮换 (IP rotation) 或标头篡改 (header manipulation) 绕过  
- [ ] 是 — 仅对身份验证端点**应用**了速率限制，导致其他端点暴露  
- [ ] 否 — 未对任何应用程序功能**应用**速率限制或流量削减  

### 应用程序是否检测并阻止自动化交互模式？
- [ ] 是 — **启用**了行为分析或验证码 (CAPTCHAs)，并有效阻止了机器人 (Bots)  
- [ ] 是 — **启用**了反自动化机制，但通过使用无头浏览器 (Headless Browsers) 或会话循环 (Session Cycling) **可能实现**绕过  
- [ ] 否 — 应用程序**无法**区分人类用户和自动化脚本  

### 异常行为是否被记录并触发防御性响应？
- [ ] 是 — 误用行为会触发自动拦截并向安全团队发出告警  
- [ ] 是 — 误用行为会被记录用于审计，但**未**采取自动防御行动  
- [ ] 否 — 应用程序**不记录**也不响应异常活动模式  

### 是否可以通过操纵客户端标识符或标头 (Headers) 来绕过防御？
- [ ] 否 — 防御依赖于服务器端状态和经过验证的遥测数据 (Telemetry)  
- [ ] 是 — **可以**通过清除 Cookies 或轮换 `User-Agent` 字符串来绕过控制  
- [ ] 是 — **可以**通过注入诸如 `X-Forwarded-For` 或 `X-Real-IP` 之类的标头来绕过控制

---

## WSTG-BUSL-08 — 测试上传非预期文件类型

上传非预期文件类型允许攻击者通过提交应用程序不打算处理的文件来绕过业务逻辑 (Business Logic) 限制，这可能导致远程代码执行 (Remote Code Execution, RCE)、跨站脚本攻击 (Cross-Site Scripting, XSS) 或拒绝服务攻击 (Denial of Service, DoS)。攻击者通过修改文件扩展名、MIME 类型 (MIME types) 和魔术字节 (Magic Bytes) 来探测这些漏洞，以诱使服务端验证逻辑接受恶意内容。这种情况通常发生在个人资料图片上传、文档管理系统或工单附件中，因为这些场景在后端 (Back-end) 缺乏足够的验证。如果上传的文件被执行，成功的利用可能会危及宿主服务器；或者如果文件以错误的 Content-Type 返回给其他用户，则会导致客户端攻击。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**工具：** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### 应用程序是否将文件上传限制在特定的扩展名集合中？
- [ ] 是 — 强制执行严格的扩展名白名单 (Whitelist)，且**无法绕过**  
- [ ] 是 — 存在扩展名白名单，但**可以通过**双扩展名 (Double Extensions) 或空字节 (Null Bytes) 绕过  
- [ ] 否 — 任何文件扩展名**都可以**被上传  

### 除了文件扩展名之外，是否还对文件内容进行了验证？
- [ ] 是 — 服务端逻辑会校验魔术字节并执行深度内容检测 (Deep Content Inspection)  
- [ ] 是 — 服务端逻辑仅通过 `Content-Type` 标头验证 MIME 类型  
- [ ] 否 — 在扩展名检查之后**未应用**任何内容验证  

### 攻击者是否可以通过上传 Web Shell 或脚本来执行代码？
- [ ] 否 — 上传的文件存储在不可执行目录或存储桶 (Storage Bucket)（如 S3/Azure Blob）中  
- [ ] 是 — 文件存储在 Web 可访问目录中，但通过服务器配置**禁用了**执行功能  
- [ ] 是 — 上传的恶意脚本**可以**通过可预测的 URL 路径直接执行 *(严重)*  

### 是否可以通过上传的文件类型进行如 XSS 的客户端攻击？
- [ ] 否 — 文件在提供下载时带有 `Content-Disposition: attachment` 和 `X-Content-Type-Options: nosniff`  
- [ ] 是 — SVG 或 HTML 文件**可以**被上传并在浏览器中渲染，导致 XSS  
- [ ] 是 — 图像元数据 (EXIF) **被处理**并反映在 UI 中，且未经过无害化处理 (Sanitization)

---

## WSTG-BUSL-09 — 测试恶意文件上传

文件上传功能允许用户向服务器提交数据，这为将恶意内容引入应用程序环境提供了直接向量。攻击者利用弱验证逻辑上传 Web 脚本 (Web Shell)、恶意软件或经过特殊设计的文件——例如 SVG 或 HTML——以实现远程代码执行 (Remote Code Execution, RCE) 或跨站脚本攻击 (Cross-Site Scripting, XSS)。此漏洞通常存在于个人资料设置、文档管理系统和附件处理程序等功能中，服务器在这些功能中未能正确验证文件类型、内容和执行权限。成功利用该漏洞通常会导致系统完全受损、数据外泄或将服务器用作内部网络攻击的跳板 (Pivot Point)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**工具：** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### 应用程序是否提供上传文件的功能？
- [ ] 否 — 不存在文件上传功能  
- [ ] 是 — 存在面向已认证用户的功能  
- [ ] 是 — **未经身份验证**的用户也可使用该功能 *(严重)*  

### 服务器端是否强制执行文件扩展名验证？
- [ ] 是 — 应用了严格的扩展名白名单 (Allowlist)，且**无法**绕过  
- [ ] 是 — 使用了白名单，但**可以**通过大小写敏感性或双重扩展名绕过  
- [ ] 是 — 使用了黑名单 (Blocklist)，其**可以**通过替代扩展名绕过（例如 `.phtml`, `.asa`）  
- [ ] 否 — 服务器端未应用扩展名验证  

### 除扩展名或 MIME 类型外，是否对文件内容进行了验证？
- [ ] 是 — 应用程序验证文件幻数 (Magic Bytes) 并执行深度内容检测  
- [ ] 是 — 应用程序仅检查 `Content-Type` 标头，这**很容易**被伪造  
- [ ] 否 — 应用程序完全依赖文件扩展名进行验证  

### 上传的文件是否存储在具有执行权限的目录中？
- [ ] 否 — 文件存储在专用存储服务（例如 S3）或不可执行目录中  
- [ ] 是 — 文件存储在 Web 服务器上，但已通过配置**禁用**了执行权限  
- [ ] 是 — 文件存储在 Web 可访问目录中，且**启用了**执行权限 *(严重)*  

### 是否可以通过文件名操纵绕过安全过滤器？
- [ ] 否 — 文件名已由服务器清理 (Sanitized) 或随机重新生成  
- [ ] 是 — 可以使用空字节 (Null Bytes)（例如 `shell.php%00.jpg`）绕过过滤器  
- [ ] 是 — 路径遍历 (Path Traversal) 字符（例如 `../../`）可被用于将文件上传到任意目录

---

## WSTG-BUSL-10 — 测试支付功能 (Test Payment Functionality)

测试支付功能涉及识别业务逻辑漏洞 (Business Logic Flaws)，这些漏洞可能允许攻击者绕过支付网关 (Payment Gateways)、操纵订单总额或伪造成功的交易信号。这些漏洞通常存在于应用程序、客户端浏览器以及第三方支付处理器（如 Stripe、PayPal）或内部账本系统之间的通信中。攻击者可能会尝试修改传输中的价格参数、提交负数数量以降低总费用，或重放 (Replay) 成功的交易回调以在未实际付款的情况下完成订单。成功利用此类漏洞会导致直接经济损失、库存差异，并可能损害应用程序的电子商务完整性。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 严重 (High / Critical) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### 在处理之前，交易金额是否在服务端进行了验证？
- [ ] 是 — 金额根据服务端产品数据库进行了严格验证 *(最安全)*  
- [ ] 是 — 金额经过验证，但通过参数污染 (Parameter Pollution) 或货币切换**可能绕过**逻辑  
- [ ] 否 — 交易金额**可以**在客户端请求中被修改，并被后端 (Backend) 接受  

### 商品数量是否可以被操纵，从而导致总额为零或负数？
- [ ] 否 — 负数或零值数量会被应用程序验证逻辑拒绝  
- [ ] 是 — 购物车接受负数数量，但在结账时总额会被修正  
- [ ] 是 — **可以**使用负数数量来减少订单总额或获得抵扣额度 *(严重)*  

### 应用程序是否安全地处理支付网关回调 (Callback) 或 Webhook？
- [ ] 是 — 回调需要有效的加密签名，且**无法**被伪造  
- [ ] 是 — 检查了签名，但密钥较弱或验证逻辑**可以**被绕过  
- [ ] 否 — 应用程序依赖未经身份验证或未经确认的回调来确认支付状态  

### 应用程序在结账或支付阶段是否存在竞态条件 (Race Condition) 漏洞？
- [ ] 否 — 事务完整性和数据库锁定机制已**启用**  
- [ ] 是 — **可能存在**竞态条件，但不会影响最终价格或订单履行  
- [ ] 是 — 并发请求**可能**导致单次付款触发多次订单履行，或礼品卡的“双重支出 (Double-spending)”  

### 折扣码或忠诚度积分是否存在被操纵或重复使用的风险？
- [ ] 否 — 折扣码经过一次性使用验证，且**应用了**逻辑约束  
- [ ] 是 — 折扣码可以重复使用或应用于不符合条件的商品，但影响有限  
- [ ] 是 — 攻击者**可以**应用多个冲突的代码，或绕过过期和使用次数限制

---

## WSTG-CLNT-01 — DOM 型跨站脚本攻击测试

DOM 型跨站脚本攻击 (DOM-based Cross-Site Scripting, DOM XSS) 发生在应用程序包含以不安全方式处理来自不可信源数据的客户端 JavaScript 时，通常是通过危险的接收点 (Sink) 将数据写回文档对象模型 (Document Object Model, DOM)。与反射型 (Reflected) 或存储型 (Stored) XSS 不同，该漏洞完全存在于客户端代码中；由于数据由 `innerHTML`、`eval()` 或 `document.write()` 等接收点处理，有效负载 (Payload) 通常根本不会发送到服务器。攻击者通过构造包含恶意片段或参数的 URL 来利用此漏洞，当受害者的浏览器加载这些 URL 时，会在用户会话上下文中执行任意 JavaScript。这可能导致会话令牌 (Session Tokens) 外泄、敏感数据被窃以及代表已认证用户执行未经授权的操作。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**工具：** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### 在 JavaScript 中是否使用了不可信源来捕获用户控制的数据？
- [ ] 否 — JavaScript **没有**使用 `location.hash`、`location.search` 或 `document.referrer`  
- [ ] 是 — 使用了不可信源，但数据**没有**被传递到任何执行接收点 (Sink)  
- [ ] 是 — 使用了不可信源，且数据直接流向客户端逻辑  

### 用户控制的数据是否被传递到危险的 DOM 接收点 (Sink)？
- [ ] 否 — **没有**使用诸如 `innerHTML`、`outerHTML`、`document.write()` 或 `eval()` 之类的接收点  
- [ ] 是 — 存在危险接收点，但数据已使用诸如 `DOMPurify` 之类的安全库进行了严格的**净化 (Sanitized)**  
- [ ] 是 — 存在危险接收点，但数据处理仅使用了**不足**的或自定义的基于正则表达式的过滤  
- [ ] 是 — 存在危险接收点，且数据在**没有**任何净化或编码的情况下被传递 *(关键)*  

### 执行接收点是否可以通过基于 URL 的 Payload 触发？
- [ ] 否 — **无法**通过外部输入触发从源到接收点的流  
- [ ] 是 — 流是**可能**的，但需要复杂的用户交互  
- [ ] 是 — 流是**可能**的，且可以通过简单的 URL 片段或查询参数触发  

### 现代安全响应头或基于浏览器的缓解措施是否中和了风险？
- [ ] 是 — 已**启用**带有 `script-src` 和 `trusted-types` 的严格内容安全策略 (Content Security Policy, CSP)，可防止执行  
- [ ] 是 — 存在 CSP，但 `unsafe-inline` 或弱哈希使得绕过 (Bypass) 成为**可能**  
- [ ] 否 — 不存在用于缓解 DOM 型执行的 CSP  

### 应用程序是否使用了具有内置保护功能的客户端框架？
- [ ] 是 — 框架（如 Angular, React）会自动对数据进行编码，且**无法**绕过  
- [ ] 是 — 使用了框架，但**启用**了诸如 `dangerouslySetInnerHTML` 之类的“逃生舱” (Escape Hatches)  
- [ ] 否 — 应用程序使用原生 JavaScript (Vanilla JavaScript) 或不带自动转义功能的旧版库（例如旧版本的 jQuery）

---

## WSTG-CLNT-02 — JavaScript 执行测试

JavaScript 执行测试侧重于识别应用程序不当处理用户提供的数据，从而导致在受害者浏览器上下文中执行未经授权脚本的情况。这种漏洞通常会导致跨站脚本攻击 (XSS)，攻击者借此可以窃取会话 Cookie、操作文档对象模型 (DOM) 或代表用户执行操作。渗透测试人员会检查接收器 (Sinks)，例如 `innerHTML`、`document.write()` 和 `eval()`，在这些位置，来自源 (Sources)（如 URL 片段、`localStorage` 或 `window.name`）的未净化 (Sanitized) 数据可能会被处理。成功的漏洞利用 (Exploit) 会破坏用户会话的完整性和机密性，并可作为更复杂的客户端攻击的跳板。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**工具：** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### 应用程序是否在危险的 JavaScript 接收器 (Sinks) 中处理用户提供的数据？
- [ ] 否 — 所有数据均经过净化或在 `textContent` 等安全接收器中使用  
- [ ] 是 — 数据在危险接收器中处理，但**已应用**净化/编码  
- [ ] 是 — 数据在危险接收器中处理，且**未应用**净化  

### 是否存在内容安全策略 (CSP) 以缓解脚本执行？
- [ ] 是 — **已启用**严格的 CSP 且无法绕过 (Bypass)  
- [ ] 是 — **已启用** CSP，但可以通过 `unsafe-inline` 或不安全的 CDN 进行绕过  
- [ ] 否 — **未启用** CSP  

### 是否可以通过 URL 参数或片段执行 JavaScript（基于 DOM 的 XSS）？
- [ ] 否 — 输入已正确编码，**无法**执行  
- [ ] 是 — 输入已在 DOM 中反射，但现代框架的保护机制**阻止了**执行  
- [ ] 是 — 输入在浏览器中直接执行，且漏洞利用**是可能的**  

### 事件处理程序 (Event Handlers) 或 URI 方案是否容易受到脚本注入攻击？
- [ ] 否 — 输入已净化，删除了事件属性和 `javascript:` 方案  
- [ ] 是 — **可以**注入事件处理程序，但过滤器限制了负载 (Payload) 的复杂性  
- [ ] 是 — **可以**通过 `onerror`、`onload` 或 `href` 属性执行任意 JavaScript

---

## WSTG-CLNT-03 — HTML 注入 (HTML Injection)

HTML 注入 (HTML Injection) 发生在应用程序在浏览器渲染用户提供的输入之前未能对其进行清理 (Sanitize) 时，允许攻击者将任意 HTML 标签注入到网页的文档对象模型 (DOM) 中。虽然与跨站脚本攻击 (XSS) 相似，但 HTML 注入的主要目标是操纵页面的视觉呈现，或通过注入恶意表单和欺骗性内容来促成网络钓鱼 (Phishing) 攻击。攻击者针对反映在用户界面 (UI) 中的参数（如搜索词、用户配置文件字段或错误消息），以诱骗受害者泄露敏感凭据 (Credentials) 或与未经授权的第三方链接进行交互。此漏洞对应用程序用户界面的完整性构成重大风险，并可能是更复杂的社会工程学 (Social Engineering) 或会话 (Session) 相关攻击的前奏。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### 用户控制的输入是否在没有进行适当 HTML 编码 (HTML encoding) 的情况下反映在 DOM 中？
- [ ] 否 — 所有反映的输入在渲染前都经过严格的 HTML 编码  
- [ ] 是 — 某些字符已编码，但可能绕过特定标签  
- [ ] 是 — 输入以原始形式反映，HTML 注入是可能的  

### 是否可以注入结构性 HTML 元素来改变页面布局？
- [ ] 否 — 应用程序过滤或剥离了如 `<div>`、`<table>` 或 `<iframe>` 等结构性标签  
- [ ] 是 — 可以注入结构性元素，但不会对 UI 产生重大影响  
- [ ] 是 — 通过注入的标签可能导致严重的 UI 篡改 (Defacement)  

### 是否可以注入功能性网络钓鱼元素，如表单或恶意链接？
- [ ] 否 — `<form>`、`<input>` 和 `<a>` 标签被有效阻止或清理  
- [ ] 是 — 可以注入恶意链接以将用户重定向到外部站点  
- [ ] 是 — 可以注入功能性 `<form>` 元素以获取凭据 (中)  

### 安全标头或内容安全策略 (CSP) 是否减轻了注入的影响？
- [ ] 是 — 已实施限制性 CSP，且应用了 `form-action` 或 `frame-src`  
- [ ] 否 — 存在安全标头，但 CSP 已禁用或包含 unsafe-inline/通配符  
- [ ] 否 — 未启用 CSP 或相关的安全标头

---

## WSTG-CLNT-04 — 客户端 URL 重定向测试 (Testing for Client-Side URL Redirect)

客户端 URL 重定向 (Client-side URL Redirect) 发生在 Web 应用程序接受用户控制的输入（通常通过 URL 参数或 Hash 片段 (hash fragments)）并将其用于客户端重定向接收器 (Sink)，如 `window.location` 或 `document.location.href` 时。攻击者主要利用此漏洞进行复杂的网络钓鱼 (Phishing) 活动，因为受害者最初信任合法域名，随后被静默重定向到恶意网站。除了钓鱼攻击外，如果重定向发生时 URL 或 `Referer` 请求头中存在敏感信息，客户端重定向通常可以串联起来以窃取 OAuth 访问令牌 (OAuth access tokens) 或会话标识符 (Session identifiers) 等敏感数据。在现代单页面应用 (Single Page Applications - SPAs) 中，此漏洞经常出现在路由逻辑、身份验证后的 “return to” 参数或语言切换功能中。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **测试状态** | 未执行 |
| **严重程度** | 中危 (Medium)* |

> *如果重定向可被用于窃取 OAuth 令牌、会话 ID 或绕过身份验证流程中的安全控制，严重程度将变为高危 (High)。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**工具：** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### 应用程序是否使用客户端脚本根据用户输入处理重定向？
- [ ] 否 — 重定向完全由服务端通过 `3xx` HTTP 状态码处理  
- [ ] 是 — 应用程序使用 `window.location` 等 JavaScript 接收器 (Sinks) 根据 URL 参数进行跳转  
- [ ] 是 — 应用程序使用客户端框架路由（例如 React Router, Vue Router）处理用户提供的路径  

### 是否针对目标 URL 实施了输入验证或白名单 (Whitelist) 控制？
- [ ] 是 — 强制执行严格的允许域名/路径白名单 (Whitelist)，且无法绕过 (Bypass)  
- [ ] 是 — 存在验证但依赖于弱正则表达式 (Regex) 或黑名单 (Blacklists)，且可以绕过  
- [ ] 否 — 应用程序接受任何任意字符串作为重定向目标  

### 是否可以使用常见的混淆 (Obfuscation) 技术绕过重定向逻辑？
- [ ] 否 — 控制措施能正确处理编码字符、协议相对 URL (Protocol-relative URLs) (`//`) 和反斜杠  
- [ ] 是 — 可以通过 URL 编码 (URL encoding) 或双重编码绕过  
- [ ] 是 — 可以通过协议相对 URL 或 `@` 字符（例如 `https://expected.com@attacker.com`）绕过  
- [ ] 是 — 可以通过特定的浏览器特性 (Quirks) 或空字节注入 (Null byte injection) 绕过  

### 重定向过程是否会向目标站点暴露敏感数据？
- [ ] 否 — URL 中不存在敏感数据，重定向过程中也不通过请求头传输敏感数据  
- [ ] 是 — 敏感数据（例如会话令牌、API 密钥）通过 `Referer` 请求头泄露到外部站点  
- [ ] 是 — URL 片段 (`#`) 中的敏感数据可被目标站点的脚本访问  

### 是否可以通过重定向接收器执行 JavaScript (DOM-based XSS)？
- [ ] 否 — 接收器仅允许跳转到 `http` 或 `https` 协议  
- [ ] 是 — 重定向接收器允许 `javascript:` 或 `data:` URI，导致 DOM 型跨站脚本攻击 (DOM-based XSS) 成为可能

---

## WSTG-CLNT-05 — 测试 CSS 注入

当应用程序允许用户提供的输入在未经过适当清理或转义的情况下影响网页的层叠样式表 (CSS) 时，就会发生 CSS 注入 (CSS Injection)。虽然通常被认为只是外观问题，但攻击者可以利用 CSS 注入，通过使用属性选择器 (attribute selectors) 和背景图片 (background-image) 属性触发外部请求，从而窃取诸如 CSRF 令牌或会话 ID (session IDs) 之类的敏感数据。这种漏洞通常出现在用户偏好（例如：自定义主题、字体颜色）被反映在 `<style>` 块或内联 `style` 属性中的端点。从攻击者的角度来看，成功利用该漏洞可能导致界面劫持 (UI redressing)、通过未经授权的布局修改进行网络钓鱼 (phishing)，或者在严格的内容安全策略 (CSP) 可能会阻止基于 JavaScript 攻击的环境中进行隐蔽的数据外泄 (data exfiltration)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果注入点允许泄露 CSRF 令牌等敏感属性，或者可用于敏感工作流中的界面劫持，则严重程度变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### 应用程序是否在 CSS 上下文中反映用户控制的输入？
- [ ] 否 — 输入未在 style 标签、属性或 CSS 文件中反映  
- [ ] 是 — 输入已反映，但严格限制为安全的字母数字值  
- [ ] 是 — 输入在 `style` 属性或 `<style>` 块内反映  

### CSS 特定的元字符和函数是否经过了适当的清理？
- [ ] 是 — 诸如 `{`、`}`、`:` 的字符以及 `url()` 等函数已得到妥善转义  
- [ ] 是 — 已进行清理，但可能通过编码或 CSS 注释进行绕过 (bypass)  
- [ ] 否 — 未对 CSS 元字符应用任何清理  

### 是否可以使用 CSS 选择器进行数据外泄？
- [ ] 否 — 选择器无法触发外部请求或泄露数据  
- [ ] 是 — 可以通过属性选择器和 `background-image` 进行部分数据外泄  
- [ ] 是 — 可以使用自动化暴力破解 (brute-force) 技术实现敏感令牌的完全外泄  

### 内容安全策略 (CSP) 是否减轻了 CSS 注入的风险？
- [ ] 是 — `style-src` 仅限 'self'，且 `img-src` 或 `connect-src` 受到限制  
- [ ] 是 — 存在 CSP，但使用了 'unsafe-inline' 或允许通配符外部域  
- [ ] 否 — 未实施 CSP 来防止通过 CSS 加载外部资源  

### 该漏洞是否可用于执行界面劫持或网络钓鱼？
- [ ] 否 — 注入范围过于受限，无法显著修改布局  
- [ ] 是 — 可以修改 UI，允许覆盖恶意元素  
- [ ] 是 — 可以完全控制页面布局，从而实现极具欺骗性的网络钓鱼攻击

---

## WSTG-CLNT-06 — 客户端资源操纵测试

客户端资源操纵 (Client-side Resource Manipulation) 发生在应用程序允许用户控制的输入影响浏览器加载资源（如脚本、样式表或 iframe）的来源或路径时。通过操纵 URI 分段 (URI fragments)、查询参数 (query parameters) 或其他 DOM 源 (DOM sources)，攻击者可以强制应用程序加载恶意的外部内容或执行未授权代码。此类漏洞通常存在于 JavaScript 逻辑中，这些逻辑在没有针对可信白名单 (allow-list) 进行充分验证的情况下，动态填充 HTML 元素的 `src` 或 `href` 属性。从攻击者的角度来看，通过构建一个将资源加载重定向到攻击者控制的服务器的 URL，即可利用该漏洞，这可能导致基于 DOM 的跨站脚本攻击 (DOM-based Cross-Site Scripting, XSS)、敏感数据外泄或 UI 界面重绘 (UI redressing)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**工具：** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### 应用程序是否使用客户端接收器动态加载或嵌入资源？
- [ ] 否 — 资源仅通过静态 HTML 或服务端逻辑加载  
- [ ] 是 — 客户端 JavaScript 动态填充资源属性，如 `src`、`href` 或 `data`  

### 是否针对资源 URI 来源实施了验证控制或白名单？
- [ ] 是 — 所有动态资源**均应用了**严格的白名单和源验证 (origin validation)  
- [ ] 是 — 存在验证但依赖于弱黑名单或易被绕过的正则表达式 (regex)  
- [ ] 否 — 未对用户控制的资源路径应用任何验证控制  

### 是否可以使用替代协议或编码绕过 URI 验证？
- [ ] 否 — **无法**绕过资源过滤器  
- [ ] 是 — **可以**使用不同的协议绕过，如 `data:`、`vbscript:` 或 `javascript:`  
- [ ] 是 — **可以**使用编码字符或空字节 (null bytes) 绕过简单的字符串匹配  

### 攻击者能否将资源加载重定向到外部、不可信的域名？
- [ ] 否 — 资源路径被限制在同源 (same origin) 或固定的子目录内  
- [ ] 是 — 应用程序**可以**被强制从任意外部域名加载脚本或样式  

### 资源操纵的最大影响是什么？
- [ ] 低 — 操纵仅影响非可执行元素，如图像或非敏感文本  
- [ ] 中 — 操纵允许 CSS 注入 (CSS Injection) 或显著的 UI 界面重绘  
- [ ] 高 — 操纵导致 DOM-based XSS 或执行任意 JavaScript *(紧急)*

---

## WSTG-CLNT-07 — 跨源资源共享 (Cross-Origin Resource Sharing, CORS)

跨源资源共享 (Cross-Origin Resource Sharing, CORS) 是一种基于浏览器的机制，允许服务器显式准许来自不同源的资源访问，从而有效地放宽了同源策略 (Same-Origin Policy, SOP)。当应用程序过度信任任意起源时，就会发生错误配置，这通常表现为在 `Access-Control-Allow-Origin` 响应标头中反射 `Origin` 标头，或使用实现不当的正则表达式 (Regex) 白名单。从攻击者的角度来看，可以通过在受控域名上托管恶意脚本来利用宽松的 CORS 策略，该脚本向受攻击的应用程序发起身份验证请求，从而允许攻击者直接从响应正文中提取敏感数据，如 CSRF 令牌 (Tokens) 或个人身份信息 (Personally Identifiable Information, PII)。此漏洞在处理已认证会话并返回非公开信息的 API 端点上最为关键。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**工具：** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### 应用程序是否在已认证端点上实现了 CORS 标头？
- [ ] 否 — **未启用** CORS；浏览器默认为严格的同源策略 (Same-Origin Policy)  
- [ ] 是 — 存在 CORS 标头，且仅限于严格的**白名单**  
- [ ] 是 — 存在 CORS 标头，但配置过于**宽松**  

### 服务器如何处理 `Access-Control-Allow-Origin` (ACAO) 标头？
- [ ] ACAO 设置为特定的、**受信任的**静态域名  
- [ ] ACAO 设置为 `*` (通配符) 且**不能**发送凭据 (Credentials)  
- [ ] ACAO 设置为 `*` (通配符) 且**可以**发送凭据 *(注：大多数浏览器会拦截此配置)*  
- [ ] ACAO **动态反射**来自请求的 `Origin` 标头值  

### `Access-Control-Allow-Credentials` (ACAC) 标头是否设置为 `true`？
- [ ] 否 — ACAC **不存在**或设置为 `false`  
- [ ] 是 — ACAC 设置为 `true`，但 ACAO **仅限于**受信任的源  
- [ ] 是 — ACAC 设置为 `true` 且 ACAO **反射**任意源 *(高危)*  

### 是否可以通过子域名或 null 源操纵绕过起源白名单？
- [ ] 否 — 白名单被严格执行且**无法**绕过  
- [ ] 是 — 白名单接受目标的**任何**子域名 (例如：`attacker.target.com`)  
- [ ] 是 — 白名单接受通过沙箱化 iframe 产生的 `null` 源  
- [ ] 是 — 白名单易受正则表达式绕过的影响 (例如：`target.com.attacker.com` 或 `target.com-safe.com`)  

### CORS 配置是否允许敏感数据泄露？
- [ ] 否 — 暴露的端点**不包含**敏感数据或特定于会话的数据  
- [ ] 是 — 已认证端点返回 PII、凭据或 CSRF 令牌，这些内容**可以**被未经授权的源读取

---

## WSTG-CLNT-08 — 跨站 Flash 攻击测试 (Testing for Cross Site Flashing)

跨站 Flash 攻击 (Cross-Site Flashing, XSF) 是一种客户端漏洞，当 Flash (SWF) 文件未能正确处理用户控制的输入时就会发生，允许攻击者执行恶意 ActionScript 代码或桥接到浏览器的 JavaScript 环境中。通过操纵传递给 `ExternalInterface.call`、`getURL` 或 `loadMovie` 等接收器 (Sinks) 的参数（如 `FlashVars` 或 URL 查询字符串），攻击者可以在受漏洞影响的域名上下文中执行操作，包括会话劫持 (Session Hijacking) 和数据泄露 (Data Exfiltration)。这种漏洞主要存在于仍托管 SWF 文件的遗留企业级应用程序中，由于输入验证不足，Flash 影片被重新利用作为跨站脚本攻击 (Cross-Site Scripting, XSS) 的矢量，或通过宽松的跨域配置绕过同源策略 (Same-Origin Policy, SOP)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### 应用程序中是否托管了遗留的 Adobe Flash (.swf) 文件？
- [ ] 否 — 应用程序范围内没有托管或引用 Flash 文件  
- [ ] 是 — 存在 SWF 文件，但仅提供静态内容  
- [ ] 是 — 存在 SWF 文件，并通过 `FlashVars` 或 URL 字符串接受动态参数  

### 传递给 ActionScript 接收器的输入是否经过了清洗？
- [ ] 是 — 所有输入都经过严格验证，ActionScript 接收器 (Sinks) **无法**被操纵  
- [ ] 是 — 已应用验证，但**有可能**通过特定的编码技术绕过  
- [ ] 否 — 输入直接传递到敏感接收器，如 `ExternalInterface.call` 或 `getURL` *(高危)*  

### Flash 文件是否可被用于执行任意 JavaScript (XSS)？
- [ ] 否 — `ExternalInterface` 已**禁用**，或 `allowScriptAccess` 参数被设置为 `never`  
- [ ] 是 — `allowScriptAccess` 设置为 `sameDomain`，但 SWF 托管在目标域名上  
- [ ] 是 — `allowScriptAccess` 设置为 `always`，允许来自任何域名的 XSS  

### `crossdomain.xml` 策略是否防止了未经授权的跨源请求？
- [ ] 是 — `crossdomain.xml` 具有严格限制，仅允许受信任的特定源 (Origins)  
- [ ] 否 — `crossdomain.xml` 文件**缺失**  
- [ ] 是 — 策略过于宽松（例如：`<allow-access-from domain="*" />`）  

### 是否可以强制 SWF 文件加载外部受攻击者控制的影片？
- [ ] 否 — **无法**强制应用程序加载外部 SWF 文件  
- [ ] 是 — `loadMovie` 或 `loadMovieNum` 函数接受未经验证的外部 URL

---

## WSTG-CLNT-09 — 点击劫持 (Clickjacking) 测试

点击劫持 (Clickjacking)，也称为用户界面重绘 (UI redressing)，发生在攻击者使用透明或不透明的层，诱导用户在打算点击顶层页面时，点击另一个页面上的按钮或链接。这种漏洞破坏了用户操作的完整性，可能导致代表受害者执行未经授权的数据修改、账户更改或资金转账。攻击者通常通过将目标网站嵌入到受控恶意站点的 `<iframe>` 中，并覆盖欺骗性内容或诱导性元素来利用此漏洞。它最常出现在执行敏感状态更改操作的页面上，例如“删除账户”按钮、密码重置表单或管理配置面板。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果可以通过点击劫持触发敏感操作（例如资金转账、账户删除或权限提升 (Privilege Escalation)），则严重程度变为“高”。

**参考文献：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**工具：** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### 应用程序是否实现了服务端响应头以防止未经授权的框架嵌套 (Framing)？
- [ ] 是 — `X-Frame-Options` 或 `Content-Security-Policy` **已应用**并能防止框架嵌套 *(最安全)*  
- [ ] 是 — 响应头存在但**配置错误**（例如 `X-Frame-Options: ALLOW-FROM` 缺乏广泛的浏览器支持）  
- [ ] 否 — 未应用任何框架保护响应头  

### 是否实现了 `Content-Security-Policy` (CSP) 的 `frame-ancestors` 指令？
- [ ] 是 — `frame-ancestors 'none'` 或 `'self'` **已应用**  
- [ ] 是 — `frame-ancestors` 存在但**允许**不受信任或过于宽泛的来源  
- [ ] 否 — 该指令**缺失**或未实现 CSP  

### `X-Frame-Options` (XFO) 响应头是否针对旧版浏览器支持进行了正确配置？
- [ ] 是 — `DENY` 或 `SAMEORIGIN` **已应用**  
- [ ] 是 — XFO 存在，但由于存在 CSP `frame-ancestors` 指令，现代浏览器会将其**忽略**  
- [ ] 否 — XFO 响应头**缺失**或设置为不安全的值  

### 框架保护是否可以通过已知技术绕过 (Bypass)？
- [ ] 否 — 无法通过标准技术（双重框架嵌套等）**绕过**  
- [ ] 是 — 由于 `frame-ancestors` 白名单中的正则表达式或逻辑薄弱，保护**可以**被绕过  
- [ ] 是 — 由于应用程序仅依赖基于 JavaScript 的框架破解 (Frame-busting)，保护**可以**被绕过  

### 敏感操作是否可以通过点击劫持概念验证 (Proof-of-Concept / PoC) 被利用？
- [ ] 否 — 框架嵌套被阻断或无法触及敏感操作  
- [ ] 是 — 用户界面重绘**是可能的**，但需要复杂的用户交互  
- [ ] 是 — 用户界面重绘**是可能的**，并且允许立即执行状态更改操作 *(紧急/Critical)*

---

## WSTG-CLNT-10 — 测试 WebSocket (Testing WebSockets)

WebSocket 测试侧重于客户端与服务器之间建立的全双工通信通道 (Full-duplex communication channel)，该通道绕过了传统的 HTTP 请求-响应模式。渗透测试人员 (Pentesters) 评估握手 (Handshake) 过程的安全性、跨站 WebSocket 劫持 (Cross-Site WebSocket Hijacking, CSWSH) 防护措施的存在，以及对消息有效载荷 (Payload) 实施的输入验证的严格程度。漏洞利用通常涉及劫持活动会话以实时窃取数据，或通过持久套接字注入恶意有效载荷，从而触发命令注入 (Command Injection) 或 SQL 注入 (SQLi) 等服务器端漏洞。由于许多传统的 Web 应用防火墙 (Web Application Firewalls, WAF) 无法有效检测 WebSocket 流量，该协议通常为绕过边界防御提供了一个隐蔽的矢量 (Vector)。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 中 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**工具：** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### WebSocket 连接是否通过加密通道建立？
- [ ] 是 — 对所有连接强制执行 `wss://` (WebSocket Secure)  
- [ ] 否 — 使用了 `ws://`，允许中间人 (Person-in-the-middle, PITM) 窃听  

### 应用程序是否针对跨站 WebSocket 劫持 (CSWSH) 进行了保护？
- [ ] 否 — 服务器在握手期间严格验证 `Origin` 标头  
- [ ] 是 — `Origin` 标头验证薄弱，或者**可以**通过 null 或伪造的源 (Origin) 进行绕过  
- [ ] 是 — 未执行 `Origin` 标头验证，允许跨站攻击者发起连接 *（严重）*  

### 是否对 WebSocket 消息有效载荷 (Payload) 应用了输入验证和清理 (Sanitization)？
- [ ] 是 — 所有传入消息均经过清理，并根据模式 (Schema) 进行了验证  
- [ ] 是 — 存在部分验证，但**可能**使用编码或非标准有效载荷进行绕过  
- [ ] 否 — 消息被直接处理，导致注入（XSS, SQLi, RCE）或逻辑篡改  

### 是否对每个 WebSocket 消息都执行了身份验证 (Authentication) 和授权 (Authorization) 检查？
- [ ] 是 — 对每条消息都验证了会话，或协议使用令牌 (Token) 实现无状态化  
- [ ] 是 — 在握手时进行了身份验证，但**未**针对每条消息强制执行特定操作的授权  
- [ ] 否 — 仅在握手时检查身份验证，导致会话劫持后可获得完整访问权限  

### 服务器是否对 WebSocket 实施了速率限制 (Rate Limiting) 或资源约束？
- [ ] 是 — **已启用**并强制执行速率限制和最大消息大小  
- [ ] 否 — 不存在任何限制，允许通过消息泛洪或超大有效载荷导致拒绝服务 (Denial of Service, DoS) 攻击

---

## WSTG-CLNT-11 — 测试网页消息传递 (Test Web Messaging)

网页消息传递 (Web Messaging) 或 `postMessage` 能够实现窗口对象之间的跨源通信 (cross-origin communication)，例如父页面与生成的弹出窗口或嵌入的内嵌框架 (iframe) 之间的通信。该机制对于现代 Web 功能至关重要，但如果接收端应用程序未能验证发送者的身份或未正确处理消息有效负载 (Payload)，则会引入重大风险。攻击者通过托管一个嵌入目标应用程序的恶意网站，并发送精心构造的消息来触发非预期的操作、窃取敏感数据或执行基于 DOM 的跨站脚本攻击 (DOM-based Cross-Site Scripting, XSS)。当消息监听器没有严格验证 `origin` 属性，或者将 `message.data` 直接传递到危险的执行接收器 (execution sinks) 时，就会发生成功的攻击。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **测试状态** | 未执行 |
| **风险等级** | 中 / 高 (Medium / High) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**工具：** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### 应用程序是否利用 `postMessage` API 进行跨文档通信？
- [ ] 否 — 客户端代码中**不**存在 `postMessage` 监听器  
- [ ] 是 — `postMessage` 用于内部组件通信  
- [ ] 是 — `postMessage` 用于与第三方域名或挂件 (widgets) 进行通信  

### 传入消息的源 (origin) 是否经过严格验证？
- [ ] 是 — 使用 `===` 或恒等比较，根据严格的白名单检查源 *(最安全)*  
- [ ] 是 — 使用正则表达式验证源，但逻辑**容易**被绕过 (例如：`^https://trusted.com`)  
- [ ] 否 — **未**检查 `origin` 属性，允许任何域名发送消息  
- [ ] 否 — 发送者的 `targetOrigin` 中使用了通配符 `*`，可能导致数据泄露给恶意监听器  

### 消息有效负载 (Payload) 在被应用程序处理前是否经过清理？
- [ ] 是 — 数据被视为纯文本处理，且未使用危险的接收器 (sinks)  
- [ ] 是 — 数据在使用前经过解析 (例如 `JSON.parse`) 和验证，且**无法**绕过  
- [ ] 否 — 消息数据被直接传递到危险的接收器，如 `innerHTML`、`eval()` 或 `setTimeout()`  
- [ ] 否 — 消息数据在未经验证的情况下被用于窗口导航或更改 `location.href`  

### 攻击者是否可以通过恶意消息触发未授权的操作或窃取数据？
- [ ] 否 — 即使是伪造的消息，也无法访问敏感操作或数据  
- [ ] 是 — 精心构造的消息**可以**触发敏感的状态更改 (例如：修改密码、更新个人资料)  
- [ ] 是 — 精心构造的消息**可以**导致基于 DOM 的 XSS 或窃取 CSRF 令牌/会话数据 (session data)

---

## WSTG-CLNT-12 — 测试浏览器存储 (Test Browser Storage)

测试浏览器存储涉及分析应用程序如何利用客户端机制，如本地存储 (LocalStorage)、会话存储 (SessionStorage)、IndexedDB 和 WebSQL 来持久化数据。如果不安全地存储敏感信息——包括个人可识别信息 (Personally Identifiable Information, PII)、身份验证令牌 (Authentication Tokens) 或会话标识符——在攻击者实现跨站脚本 (Cross-Site Scripting, XSS) 或获得对用户设备的物理访问时，这些信息可能会被泄露。渗透测试人员必须确定数据是否以明文形式存储，注销后是否仍可访问，以及存储生命周期是否得到妥善管理以防止数据泄漏。从攻击者的角度来看，这些存储机制是窃取状态维护信息的理想目标，从而实现会话劫持 (Session Hijacking) 或长期账户持久化。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高* |

> *如果会话令牌或敏感 PII 存储在 LocalStorage 中且可通过 XSS 访问，则严重程度变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### 应用程序是否在浏览器存储中存储敏感信息？
- [ ] 否 — 应用程序不使用浏览器存储，或仅存储非敏感的 UI 状态  
- [ ] 是 — 存储了敏感数据，但使用强大的客户端加密技术进行了加密  
- [ ] 是 — 敏感数据（令牌、PII、机密）以明文 (Cleartext) 形式存储 *(中)*  

### 存储的数据是否可被未经授权的脚本访问（XSS 风险）？
- [ ] 否 — 数据存储在 HttpOnly Cookie 中（而非浏览器存储），或未利用存储  
- [ ] 是 — 使用了 LocalStorage/SessionStorage，使得数据可通过任何 XSS 有效负载 (Payload) 完全访问 *(高)*  

### 用户注销时，浏览器存储是否已被妥善清除？
- [ ] 是 — 在注销过程中，所有与应用程序相关的存储均被明确清除  
- [ ] 否 — 注销后存储依然存在，但不包含敏感会话数据  
- [ ] 否 — 会话终止后，身份验证令牌或 PII 仍保留在存储中 *(中)*  

### 应用程序是否对大型/敏感数据集使用 IndexedDB 或 WebSQL？
- [ ] 否 — 未使用这些存储机制  
- [ ] 是 — 数据已存储，且在 DOM 中渲染之前经过了妥善的清理 (Sanitized)  
- [ ] 是 — 敏感数据集以明文形式存储在 IndexedDB/WebSQL 结构中  

### 存储数据的完整性是否可以被操纵以影响应用程序逻辑？
- [ ] 否 — 应用程序在处理存储中的数据之前会对其进行验证或签名  
- [ ] 是 — 修改存储值（例如：用户角色、标志）是可能的，并会导致应用程序行为发生改变

---

## WSTG-CLNT-13 — 跨站脚本包含 (Cross-Site Script Inclusion, XSSI) 测试

跨站脚本包含 (Cross-Site Script Inclusion, XSSI) 发生在 Web 应用程序在动态生成的 JavaScript 文件或 JSONP (JSON with Padding) 响应中导出敏感数据时，攻击者控制的站点随后可以通过 `<script>` 标签包含这些文件。由于脚本包含绕过了同源策略 (Same-Origin Policy, SOP)，攻击者可以在其自己的上下文中执行该脚本，并覆盖全局对象或变量以窃取 (Exfiltrate) 敏感信息。这种漏洞通常出现在返回用户特定数据（如电子邮件地址、API 密钥 (API Keys) 或会话元数据）的经过身份验证的端点中。从攻击者的角度来看，利用 (Exploit) 过程涉及构建一个引用易受攻击脚本的恶意页面，并捕获由此产生的全局变量更改或函数调用。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重程度** | 中 / 高 |

> *如果泄露的数据包含 CSRF 令牌、会话标识符或高度敏感的个人可识别信息 (PII)，严重程度将变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### 是否有任何身份验证端点返回包含敏感信息的动态 JavaScript 或 JSONP？
- [ ] 否 — 所有脚本都是静态的，**不能**包含用户特定数据  
- [ ] 是 — 存在动态脚本，但**不包含**敏感信息  
- [ ] 是 — 动态脚本包含 PII 或令牌，并且**可以**跨源访问  

### 敏感脚本响应是否实现了 `X-Content-Type-Options: nosniff` 标头？
- [ ] 是 — 标头已**应用**，可防止 MIME 类型嗅探 (MIME-type sniffing)  
- [ ] 否 — 标头**缺失**，可能允许将非脚本内容作为脚本执行  

### 敏感脚本是否受到抗 XSSI 机制（如不可执行前缀或自定义标头）的保护？
- [ ] 是 — 脚本需要无法通过标准 `<script>` 标签发送的自定义标头或令牌  
- [ ] 是 — 脚本使用无法轻易绕过的不可执行前缀（例如 `)]}'`）  
- [ ] 否 — 脚本可以通过使用环境凭据 (Ambient Credentials) 的 GET 请求直接访问 *(严重)*  

### 是否可以通过覆盖全局变量或原型 (Prototypes) 来窃取敏感数据？
- [ ] 否 — 敏感数据的作用域限制在局部，或通过现代浏览器功能进行保护  
- [ ] 是 — 敏感数据被分配给全局变量，并且**可以**被捕获  
- [ ] 是 — 数据通过**可以**被拦截的 JSONP 回调泄露

---

## WSTG-CLNT-14 — 测试反向标签页钓鱼 (Testing for Reverse Tabnabbing)

反向标签页钓鱼 (Reverse Tabnabbing) 是一种客户端漏洞 (Client-side vulnerability)。当通过 `target="_blank"` 打开一个页面时，该页面通过 `window.opener` 对象保留对父页面的功能性引用。这允许攻击者控制的目标站点通过编程方式将原始的、受信任的标签页重定向到恶意 URL，通常是旨在窃取凭证的钓鱼页面 (Phishing page)。该攻击利用了用户对原始标签页的固有信任，因为用户不太可能注意到他们刚刚离开的页面已经导航到了不同的域名 (Domain)。现代安全实践要求在锚点标签 (Anchor tags) 上使用 `rel="noopener"` 或 `rel="noreferrer"` 属性，或者在 JavaScript 中将 `opener` 属性设置为 null，以防止这种跨窗口通信。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中* |

> *严重程度在大多数现代浏览器中为“低”，因为它们现在默认将 `target="_blank"` 处理为 `rel="noopener"`。如果应用程序针对旧版浏览器，或者在不将 `opener` 属性设置为 null 的情况下使用 `window.open()`，则严重程度变为“中”。

**参考资料:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### 是否存在在新窗口或标签页中打开的链接？
- [ ] 否 — 没有链接使用 `target="_blank"` 或等效的基于 JavaScript 的重定向  
- [ ] 是 — `target="_blank"` 仅用于内部、受信任的链接  
- [ ] 是 — `target="_blank"` 用于外部链接或用户提供的 URL  

### 是否对锚点标签限制了 `window.opener` 关系？
- [ ] 是 — `rel="noopener"` 或 `rel="noreferrer"` **始终应用于**所有带有 `target="_blank"` 的链接 *(最安全)*  
- [ ] 是 — 已应用属性，但在高风险或用户生成的链接中**缺失**  
- [ ] 否 — **未应用**属性，允许子页面访问 `window.opener`  

### 应用程序是否容易受到通过 JavaScript `window.open()` 触发的反向标签页钓鱼攻击？
- [ ] 否 — 未使用 `window.open()`，或者已显式将 `opener` 属性设置为 null  
- [ ] 是 — 使用了 `window.open()` 且子窗口的 `opener` 引用**仍可访问**  

### 攻击者能否控制在新标签页中打开的链接目标？
- [ ] 否 — 所有链接目标都是硬编码的，并指向受信任的域名  
- [ ] 是 — 链接目标由用户提供（例如：社交媒体个人资料、简介链接），且**未**针对标签页钓鱼防护进行清理 (Sanitize)

---

## WSTG-APIT-01 — API 侦察 (API Reconnaissance)

API 侦察 (API Reconnaissance) 是识别和映射 API 攻击面 (Attack Surface) 的系统化过程，旨在发现端点 (Endpoints)、支持的方法以及底层数据结构。攻击者执行此阶段是为了发现未公开的（影子）API (Shadow API)、带有遗留漏洞的弃用版本，以及暴露的 Swagger 或 OpenAPI 定义等文档文件，这些文件可能会泄露内部业务逻辑。通过分析可预测的 URL 模式、客户端 JavaScript 文件和目录暴力破解 (Brute Force) 结果，攻击者可以构建 API 的全面映射，从而识别失效的对象级授权 (BOLA) 或批量赋值 (Mass Assignment) 攻击的目标。此类侦察通常针对 `/api/v1/`、`/swagger.json` 或 `/graphql` 等常见路径，以获取应用程序后端功能的路线图。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **风险等级** | 信息级 / 低危 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**工具：** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### API 文档文件（Swagger, OpenAPI, WSDL）是否可以公开访问？
- [ ] 否 — API 文档不可访问或受到身份验证限制  
- [ ] 是 — 文档可以访问，但需要有效的凭据  
- [ ] 是 — API 文档（例如 `/swagger-ui.html`）在未经身份验证的情况下可以**公开访问**  

### 是否可以通过模糊测试 (Fuzzing) 发现未公开或“影子”API 端点？
- [ ] 否 — 目录和端点模糊测试未发现任何未公开路径  
- [ ] 是 — 发现了未公开端点，但未经授权**无法**访问  
- [ ] 是 — 发现了未公开端点，且在没有有效授权的情况下**可以**访问  

### 应用程序是否泄露了多个 API 版本（例如 /v1/, /v2/, /beta/）？
- [ ] 否 — 仅当前加固后的 API 版本可访问  
- [ ] 是 — 存在旧版本，但实施了与当前版本**相同**的安全控制  
- [ ] 是 — 旧版本（例如 `/v1/`）可以访问，且**缺乏**新版本的安全控制  

### 客户端资源（JavaScript/移动应用）是否泄露了 API 端点结构或密钥？
- [ ] 否 — 客户端代码**不**包含硬编码的 API 路径或敏感密钥  
- [ ] 是 — 客户端代码包含 API 端点映射，但**不**包含敏感密钥  
- [ ] 是 — API 密钥、密钥 (Secrets) 或仅限内部使用的端点 URL **被硬编码**在客户端资源中  

### 在已知端点上是否可以发现隐藏的参数或请求头 (Headers)？
- [ ] 否 — 参数模糊测试（例如通过 `Arjun`）**未**发现隐藏输入  
- [ ] 是 — 发现了隐藏参数（例如 `debug=true`, `admin=1`），但它们**无效**  
- [ ] 是 — 发现了隐藏参数，且**可以**用于改变应用程序行为

---

## WSTG-APIT-02 — API 失效的对象级授权

失效的对象级授权 (Broken Object Level Authorization, BOLA)，也被称为不安全的直接对象引用 (Insecure Direct Object Reference, IDOR)，当 API 未能验证用户是否具有访问或操作由 ID 标识的特定资源的适当权限时，就会发生此漏洞。攻击者通过系统地枚举或猜测请求路径、查询参数或 JSON 正文中的标识符（例如数字 ID 或 UUID）来利用此漏洞，从而访问属于其他用户的数据。这种漏洞是现代 API 安全中最常见且影响最大的问题，可能导致大规模数据外泄、未经授权的修改或完全接管账户。从攻击者的角度来看，目标是识别接受对象标识符的端点 (Endpoint)，并测试服务器端授权逻辑是否缺失，或者是否可以通过操纵这些 ID 来绕过该逻辑。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **测试状态** | 未执行 |
| **严重程度** | 高 / 紧急 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**工具：** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### 在 API 请求中，对象标识符是否可预测或可枚举？
- [ ] 否 — 标识符较长、随机且符合密码学安全（例如 UUIDv4）  
- [ ] 是 — 标识符是连续的整数（例如 `101`, `102`）  
- [ ] 是 — 标识符遵循可预测的模式，或源自公开信息  

### API 是否对每个请求执行对象所有权的服务器端验证？
- [ ] 是 — 每个请求都应用了授权检查，且**无法绕过**  
- [ ] 是 — 应用了检查，但可以通过参数污染 (Parameter Pollution) 或方法隧道 (Method Tunneling) **绕过**  
- [ ] 否 — 应用程序仅依赖用户已通过身份验证，而不检查特定资源的所有权  

### 是否可以通过更改标识符来访问或修改另一个用户的资源？
- [ ] 否 — **无法**未经授权访问其他用户的资源  
- [ ] 是 — **可能**存在未经授权的读取访问（水平 BOLA / Horizontal BOLA）  
- [ ] 是 — **可能**存在未经授权的修改或删除（水平 BOLA）  

### API 是否允许通过替换 ID 来访问管理级或系统级对象？
- [ ] 否 — 管理资源受二级授权层保护  
- [ ] 是 — **可能**访问或修改系统级资源（垂直 BOLA / Vertical BOLA）  

### 是否可以通过将标识符移动到请求的不同部分来绕过授权检查？
- [ ] 否 — 无论参数位置如何，授权逻辑都是一致的  
- [ ] 是 — 当 ID 从 URL 路径移动到 JSON 正文或请求头 (Headers) 时，**可以绕过**  
- [ ] 是 — 当提供多个 ID 且服务器处理了未经授权的 ID 时，**可以绕过**

---

## WSTG-APIT-99 — GraphQL 测试

GraphQL 是一种用于 API 的查询语言 (Query Language)，它允许客户端请求特定的数据结构，但配置错误通常会导致严重的安全性风险，包括信息泄露 (Information Disclosure) 和资源耗尽 (Resource Exhaustion)。漏洞通常源于启用了内省 (Introspection)、缺乏查询深度/复杂度限制，以及解析器函数 (Resolver functions) 中的失效的对象级授权 (Broken Object-Level Authorization, BOLA)。攻击者利用这些端点 (Endpoints)，通过内省来映射整个模式 (Schema)，利用循环片段 (Circular fragments) 触发拒绝服务 (Denial of Service, DoS)，或者通过嵌套查询访问未经授权的字段来绕过授权。测试重点关注 `/graphql`、`/v1/graphql` 或 `/graphiql` 端点，以确保实现过程中强制执行了严格的访问控制和资源管理。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **测试状态** | 未执行 |
| **严重性** | 高 / 严重* |

> *如果失效的对象级授权 (BOLA) 允许未经授权访问个人身份信息 (PII) 或管理变异 (Mutations)，严重性将变为“严重”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**工具：** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### GraphQL 内省系统是否已启用？
- [ ] 否 — 内省已**禁用**，无法映射模式  
- [ ] 是 — 内省已**启用**，但需要管理身份验证  
- [ ] 是 — 内省已**启用**且可公开访问 *(高)*  

### 查询是否强制执行资源限制（深度和复杂度）？
- [ ] 是 — **应用**并强制执行了严格的查询深度和复杂度限制  
- [ ] 是 — **应用**了限制，但可以使用别名 (Aliases) 或片段 (Fragments) 绕过  
- [ ] 否 — 未**应用**任何限制，允许递归查询和 DoS  

### 解析器中是否存在失效的对象级授权 (BOLA)？
- [ ] 否 — 针对每个查询都在字段和对象级别**应用**了授权  
- [ ] 是 — 针对部分字段**应用**了授权，但泄露了敏感对象  
- [ ] 是 — 未**应用**授权，允许通过 ID 篡改访问任何对象  

### GraphQL 变异 (Mutations) 是否得到了妥善保护，防止未经授权的使用？
- [ ] 否 — 模式中**不**存在变异  
- [ ] 是 — 变异要求有效的身份验证和严格的授权检查  
- [ ] 是 — 未身份验证的用户**可能**执行变异或缺乏授权  

### 错误信息是否泄露敏感的实现或模式细节？
- [ ] 否 — 错误信息是通用的，且**不**泄露内部逻辑  
- [ ] 是 — 错误信息通过“您是指...？”(Did you mean...?) 泄露了底层数据库类型或建议  
- [ ] 是 — 响应中**泄露**了完整的堆栈跟踪 (Stack traces) 和敏感配置数据