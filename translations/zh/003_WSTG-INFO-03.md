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