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