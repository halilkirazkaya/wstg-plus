## WSTG-INFO-01 — Conduct Search Engine Discovery and Reconnaissance for Information Leakage

Search engine discovery involves leveraging public search engines, cached pages, and indexing services to identify information that the target organization has unintentionally exposed. Attackers use advanced search operators (Google Dorks) and third-party indexing services to discover sensitive files, directories, login portals, error messages, and metadata that should not be publicly accessible. This reconnaissance phase is critical because it requires zero direct interaction with the target, making it virtually undetectable while potentially revealing high-value entry points such as exposed admin panels, configuration files, database dumps, and internal documentation.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Are sensitive files or directories indexed by search engines?
- [ ] No — no sensitive content found in search engine results  
- [ ] Yes — configuration files, backups, or internal documents **are** indexed *(Critical)*  

### Does Google Dorking reveal exposed administrative or login interfaces?
- [ ] No — admin panels and login pages are **not** indexed  
- [ ] Yes — admin panels **are** indexed but require strong multi-factor authentication  
- [ ] Yes — admin panels **are** indexed and publicly accessible **without** sufficient controls  

### Are cached versions of pages exposing sensitive data that has since been removed?
- [ ] No — no sensitive data found in cached snapshots  
- [ ] Yes — cached pages contain credentials, internal IP addresses, or sensitive information  

### Is the `robots.txt` file unintentionally disclosing sensitive paths?
- [ ] No — `robots.txt` does **not** reveal sensitive directory structures  
- [ ] Yes — `robots.txt` lists sensitive paths that aid attacker reconnaissance  
- [ ] No `robots.txt` file exists  

### Are third-party services (Shodan, Censys, Wayback Machine) revealing historical or current exposure?
- [ ] No — no significant findings from third-party indexing services  
- [ ] Yes — historical snapshots or service scans reveal sensitive metadata or service banners

---

## WSTG-INFO-02 — Fingerprint Web Server

Web server fingerprinting is the process of identifying the specific software type, version, and underlying operating system of a target web server. Attackers perform this reconnaissance to identify known vulnerabilities, such as unpatched CVEs or configuration weaknesses, that are specific to certain versions of Apache, Nginx, IIS, or other server technologies. This is typically achieved by analyzing HTTP response headers (e.g., `Server`, `X-Powered-By`), examining unique protocol behaviors, or observing information leaked through default error pages and files. Successful fingerprinting allows an attacker to tailor their exploitation strategy, moving from generic scanning to high-probability attacks against known architectural flaws.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Informational / Low* |

> *Severity becomes High if the fingerprinting reveals an outdated or end-of-life (EOL) server version with known critical vulnerabilities.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### Are identifying strings present in the HTTP response headers?
- [ ] No — `Server` and `X-Powered-By` headers are **not present** or contain generic values  
- [ ] Yes — headers reveal server software but **not** specific version numbers  
- [ ] Yes — headers reveal specific server software and **exact** version numbers  

### Do default error pages or system-generated responses leak server details?
- [ ] No — custom error pages are used and **do not** disclose server information  
- [ ] Yes — default error pages leak server software name and/or version  
- [ ] Yes — error pages leak server details, version, and **underlying operating system**  

### Is the server version disclosed via default files or directory structures?
- [ ] No — default installation files, manuals, and test scripts have been **removed**  
- [ ] Yes — default files (e.g., `info.php`, `manual/`, `test.html`) are **present** and disclose version data  

### Can the server type be accurately inferred through unique behavior analysis?
- [ ] No — server responses are normalized and behavior-based fingerprinting is **not possible**  
- [ ] Yes — unique behaviors (e.g., header ordering, specific error codes, or TCP/IP stack quirks) **allow** for server identification  

### Is the identified server version currently supported and patched?
- [ ] Yes — the identified version is current and **supported** by the vendor  
- [ ] No — the identified version is **outdated** or **end-of-life (EOL)** with known vulnerabilities

---

## WSTG-INFO-03 — Review Webserver Metafiles for Information Leakage

Webserver metafiles such as `robots.txt`, `sitemap.xml`, and `security.txt` are intended to guide crawlers and provide administrative metadata, but they often inadvertently reveal sensitive directory structures or hidden endpoints. Attackers analyze these files to enumerate the application's attack surface, identifying "Disallow" directives that frequently point to unlinked administrative panels, backup directories, or development environments. Beyond search engine instructions, files in the `.well-known/` directory or files like `humans.txt` can disclose technology stacks, developer information, or organizational security contacts. Systematic review of these files allows a tester to gain structural and technical insights without performing aggressive brute-force discovery.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Low / Informational* |

> *Severity becomes Medium if metafiles reveal unauthenticated administrative interfaces or sensitive configuration backups.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Does the `robots.txt` file exist and expose sensitive directories?
- [ ] No — `robots.txt` does **not** exist  
- [ ] Yes — `robots.txt` exists but only contains standard public paths  
- [ ] Yes — `robots.txt` reveals sensitive directory paths or administrative interfaces  
- [ ] Yes — `robots.txt` reveals credentials or specific software versions in comments  

### Is a `sitemap.xml` file present and revealing hidden application structure?
- [ ] No — `sitemap.xml` does **not** exist  
- [ ] Yes — `sitemap.xml` is present but only lists public-facing URLs  
- [ ] Yes — `sitemap.xml` lists non-linked endpoints or internal-only resources that **can** be accessed  

### Are security and organizational metafiles exposing technical details?
- [ ] No — no metadata files found in root or `.well-known/`  
- [ ] Yes — `security.txt` or `humans.txt` are present but contain standard information  
- [ ] Yes — metafiles disclose internal hostnames, developer identities, or technology stack details that **can** aid in social engineering or further attacks  

### Are there legacy or framework-specific metafiles present?
- [ ] No — no framework-specific files (e.g., `info.php`, `composer.json`, `package.json`) found  
- [ ] Yes — files like `README.md`, `CHANGELOG.txt`, or `.DS_Store` are present but sanitized  
- [ ] Yes — framework or version-specific files are present and **allow** for precise technology fingerprinting

---

## WSTG-INFO-04 — Enumerate Applications on Webserver

Application enumeration is the process of identifying all web applications and services hosted on a single web server or IP address. Because modern web servers often host multiple virtual hosts, legacy staging environments, or administrative interfaces on non-standard ports and paths, failing to identify these hidden assets leaves a significant portion of the attack surface untested. Attackers utilize techniques such as DNS zone transfers, virtual host brute-forcing, and reverse IP lookups to discover these forgotten or obscured entry points, which are frequently less secure than the primary production application.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Informational / Low |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Tools:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Are multiple IP addresses or DNS aliases associated with the target infrastructure?
- [ ] No — only a single IP and A-record exist  
- [ ] Yes — multiple IP addresses or aliases **are** identified, expanding the scope  

### Does the web server host multiple Virtual Hosts (vhosts) on the same IP?
- [ ] No — only the primary domain is served by the server  
- [ ] Yes — additional virtual hosts identified but access **is not possible** (e.g., 403 Forbidden)  
- [ ] Yes — hidden, development, or staging virtual hosts **are** identified and accessible  

### Are applications hosted on non-standard ports?
- [ ] No — only standard ports (80/443) are open  
- [ ] Yes — non-standard ports are open but do **not** host web applications  
- [ ] Yes — additional web applications **are** identified on non-standard ports (e.g., 8080, 8443, 9000)  

### Are distinct applications mapped to specific URI paths?
- [ ] No — a single application serves the entire root directory  
- [ ] Yes — multiple applications **are** discovered through directory enumeration (e.g., `/api`, `/portal`, `/backup`)  

### Do third-party reconnaissance services reveal historical or secondary applications?
- [ ] No — no significant findings from Shodan, Censys, or Wayback Machine  
- [ ] Yes — historical snapshots or service scans reveal applications that **are** currently active but unlinked

---

## WSTG-INFO-05 — Review Webpage Content for Information Leakage

Reviewing webpage content involves the manual and automated inspection of application responses, including HTML, JavaScript, and CSS files, to identify sensitive information unintentionally exposed to users. Attackers scrutinize client-side source code for developer comments, hidden form fields, internal server paths, hardcoded API keys, and debugging information that can aid in further exploitation phases. This leakage often occurs when developers forget to strip metadata or debugging code before moving to production, potentially revealing logic flaws or backend infrastructure details. From an attacker's perspective, this provides a low-effort method to map the application's internal structure and discover overlooked entry points without triggering security alerts or interacting with the server's backend logic.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium* |

> *Severity becomes High if sensitive hardcoded credentials, private keys, or internal environment variables are discovered.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Tools:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Are developer comments containing sensitive information present in the source code?
- [ ] No — no comments or only generic, non-sensitive comments found  
- [ ] Yes — comments exist but contain **no** sensitive logic or data  
- [ ] Yes — comments **reveal** internal paths, SQL queries, or administrative instructions  
- [ ] Yes — comments **reveal** credentials or sensitive developer notes *(High)*  

### Do hidden HTML input fields contain sensitive data or state information?
- [ ] No — no hidden fields found or they contain only non-sensitive tokens  
- [ ] Yes — hidden fields are used for state but are **encrypted** or **signed**  
- [ ] Yes — hidden fields contain **plaintext** passwords, user roles, or internal IDs  

### Are hardcoded secrets, API keys, or internal IP addresses present in JavaScript files?
- [ ] No — no sensitive strings or secrets found in JS files  
- [ ] Yes — JS files contain public API keys with **proper** restrictions  
- [ ] Yes — JS files contain **unprotected** API keys, internal IPs, or private endpoints  
- [ ] Yes — JS files contain **hardcoded** administrative credentials or private keys *(Critical)*  

### Does the application reveal framework versions or internal file paths in the source?
- [ ] No — framework information and file paths are **minified** or **obfuscated**  
- [ ] Yes — framework names and versions are **visible** but patched  
- [ ] Yes — internal file paths or absolute server paths are **exposed** in source code  

### Is the source code leak-prone due to the lack of minification or obfuscation?
- [ ] No — production code is **fully minified** and stripped of comments  
- [ ] Yes — code is minified but comments **remain** in the source  
- [ ] Yes — source maps (`.map` files) are **publicly accessible**, allowing full reconstruction of the source code

---

## WSTG-INFO-06 — Identify Application Entry Points

Identifying application entry points involves mapping the entire attack surface by enumerating all possible ways a user or external system can interact with the application. This process includes discovering all URLs, API endpoints, parameters (GET, POST, cookie-based), HTTP headers, and specialized protocols like WebSockets or gRPC. For a penetration tester, this stage is vital because vulnerabilities are often found in undocumented "shadow" APIs, legacy endpoints, or complex parameter structures that were overlooked during development. Thorough enumeration ensures that no component of the application remains an untested black box, thereby preventing attackers from finding unmonitored routes into the system.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Test Status** | Not Performed |
| **Severity** | Informational |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Have all GET and POST parameters been enumerated across the application?
- [ ] Yes — comprehensive enumeration via automated and manual crawling **is complete**  
- [ ] Yes — only common parameters have been identified via standard crawling  
- [ ] No — parameters are largely **unidentified** or untested  

### Are undocumented or hidden parameters (e.g., debug, admin) searched for using fuzzing?
- [ ] No — hidden parameter discovery is **not necessary** for this specific scope  
- [ ] Yes — parameter fuzzing (e.g., using Arjun) **has been performed** with no findings  
- [ ] Yes — hidden parameters **were discovered** and mapped for further testing  
- [ ] No — hidden parameter discovery **was not** performed  

### Have all supported HTTP methods (PUT, DELETE, PATCH, etc.) been identified for each endpoint?
- [ ] Yes — method discovery **is applied** across all discovered endpoints  
- [ ] Yes — method discovery **is applied** only to high-interest or sensitive endpoints  
- [ ] No — only standard GET and POST methods **are identified**  

### Are non-standard entry points like WebSockets, gRPC, or custom headers mapped?
- [ ] No — application **does not** utilize non-standard protocols or custom headers  
- [ ] Yes — all protocol-specific entry points and custom headers **are identified**  
- [ ] No — non-standard entry points **exist** but have not been mapped  

### Has the API surface (including versioned endpoints like /v1/ or /v2/) been fully mapped?
- [ ] No — application **does not** expose an API  
- [ ] Yes — all API versions and endpoints **are identified** and documented  
- [ ] Yes — only the current production API version **is identified**  
- [ ] No — API endpoints **remain unmapped** or only partially discovered

---

## WSTG-INFO-07 — Map Execution Paths Through Application

Mapping execution paths involves the systematic identification of all reachable endpoints, functional flows, and decision-making branches within a web application. This process is vital for ensuring that no "dark" functionality—such as legacy code, debug interfaces, or undocumented API routes—remains hidden from security scrutiny, as these often lack modern security controls. Attackers utilize a combination of automated spidering and manual exploration to visualize the application’s structure, often discovering vulnerabilities like unauthorized administrative access or business logic flaws during the process. By correlating inputs to specific server-side responses, testers can pinpoint sensitive processing logic that requires targeted deep-dive testing to ensure robust coverage.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Informational |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### Has the application been fully indexed using automated and manual crawling?
- [ ] Yes — comprehensive map of all visible and linked resources **is completed**  
- [ ] Yes — automated crawling **is completed** but manual exploration is pending  
- [ ] No — application has **not** been indexed  

### Are hidden or undocumented endpoints identified through JS analysis or directory brute-forcing?
- [ ] No — no undocumented paths discovered via side-channel analysis  
- [ ] Yes — hidden paths discovered but they **cannot** be accessed without authorization  
- [ ] Yes — undocumented endpoints **are** accessible and provide sensitive functionality *(Critical / High)*  

### Can execution paths be altered via non-standard parameters or headers?
- [ ] No — parameters like `debug`, `test`, or `admin` do **not** affect execution flow  
- [ ] Yes — parameters change output but **do not** bypass security controls  
- [ ] Yes — path-altering parameters **are applied** and allow bypassing intended logic  

### Is the flow of multi-step business logic (e.g., multi-factor auth, checkout) fully mapped?
- [ ] Yes — all states and transitions in multi-step processes **are identified**  
- [ ] No — complex state machine transitions **cannot** be fully mapped  

### Are API documentation files (Swagger/OpenAPI) or client-side maps (Source Maps) exposed?
- [ ] No — no architectural maps or documentation files are publicly exposed  
- [ ] Yes — documentation is present but **is protected** by authentication  
- [ ] Yes — Swagger UI, OpenAPI JSON, or JS Source Maps **are enabled** and publicly accessible

---

## WSTG-INFO-08 — Fingerprint Web Application Framework

Fingerprinting a web application framework involves identifying the specific technology stack and software versions used to build and serve the application. Attackers analyze HTTP response headers, framework-specific cookies, predictable file structures, and unique JavaScript artifacts to determine if the target is running platforms like React, Angular, Django, or Spring. This reconnaissance phase is vital because it allows a tester to narrow down the attack surface to known vulnerabilities (CVEs) and common configuration weaknesses specific to that framework. By understanding the underlying architecture, an attacker can move from generic testing to highly targeted exploitation strategies.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Informational |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Are HTTP response headers exposing the framework name or version?
- [ ] No — headers such as `X-Powered-By` or `Server` are **not** present or are sanitized  
- [ ] Yes — framework name is present but version **is hidden**  
- [ ] Yes — framework name and version **are both exposed**  

### Are framework-specific cookies or directory structures identifiable?
- [ ] No — common framework artifacts (e.g., `.js` paths, cookie names) are obfuscated or renamed  
- [ ] Yes — framework-specific cookies (e.g., `JSESSIONID`, `PHPSESSID`, `csrftoken`) **are** identifiable  
- [ ] Yes — directory structures (e.g., `/wp-content/`, `_next/static/`) **are** visible and confirm the framework  

### Do error messages or source code comments disclose framework details?
- [ ] No — error handling is generic and comments are stripped  
- [ ] Yes — framework-specific stack traces **are enabled** and visible in responses  
- [ ] Yes — HTML source code contains comments or metadata tags identifying the framework  

### Are known vulnerabilities associated with the identified framework version?
- [ ] No — the identified framework is up-to-date with **no known** public vulnerabilities  
- [ ] Yes — the framework version is outdated and specific CVEs **can be** identified

---

## WSTG-INFO-09 — Fingerprint Web Application

Fingerprinting a web application is the process of identifying the specific framework, Content Management System (CMS), or underlying technology stack and its associated versions. This reconnaissance phase is vital because it allows an attacker to map identified components against known vulnerabilities (CVEs) and public exploits, significantly narrowing the attack surface. Information is typically harvested from HTTP response headers, unique file paths, cookie names, HTML source code metadata, and cryptographic hashes of static assets. By accurately determining the technology stack, an attacker can move from generic automated scanning to highly targeted exploitation of version-specific weaknesses.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Low / Informational |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### Are HTTP response headers disclosing the technology stack or version numbers?
- [ ] No — sensitive headers are stripped or contain generic values  
- [ ] Yes — default headers like `X-Powered-By`, `Server`, or `X-AspNet-Version` **are** present  
- [ ] Yes — headers disclose specific, outdated versions of the web server or application framework  

### Is the application framework or CMS identifiable through unique file paths or naming conventions?
- [ ] No — file paths are obfuscated and do **not** reveal the underlying technology  
- [ ] Yes — common directory structures (e.g., `/wp-content/`, `/_next/`, `/node_modules/`) **are** visible  
- [ ] Yes — unique files like `robots.txt`, `humans.txt`, or `sitemap.xml` contain technology-specific metadata  

### Are specific version numbers exposed within the HTML source code or static assets?
- [ ] No — no version information is found in comments or asset parameters  
- [ ] Yes — HTML comments or meta tags (e.g., `<meta name="generator" content="...">`) **are** present  
- [ ] Yes — version strings are appended to CSS/JS filenames (e.g., `jquery.js?v=1.12.4`) and **cannot** be disabled  

### Do default error pages or management interfaces reveal the software environment?
- [ ] No — application uses custom, generic error pages for all status codes  
- [ ] Yes — default error pages for the web server or framework **are** enabled and leak version data  
- [ ] Yes — administrative login portals (e.g., `/wp-admin`, `/phpmyadmin`) **are** accessible and identifiable  

### Are cookies revealing the session management framework?
- [ ] No — session cookies use generic or custom names  
- [ ] Yes — technology-specific cookie names (e.g., `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`) **are** used

---

## WSTG-INFO-10 — Map Application Architecture

Mapping the application architecture involves identifying the underlying technologies, infrastructure components, and data flow paths that support the web application. By analyzing HTTP response headers, error messages, cookie formats, and file extensions, testers can reconstruct a schematic of the web servers, application servers, databases, and third-party integrations in use. This knowledge is fundamental for tailoring subsequent attacks, as it allows for the selection of platform-specific exploits and the identification of potential bottlenecks or misconfigured intermediary devices like load balancers and WAFs. An attacker leverages this structural map to move from generic scanning to targeted exploitation of the specific software stack and its interconnected components.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Informational / Low* |

> *Severity becomes Medium if internal network topology or private IP addresses are disclosed.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Can the web server and application server technologies be accurately identified?
- [ ] No — no identification **possible** due to effective hardening or obfuscation  
- [ ] Yes — technology type is known but specific versions **cannot** be determined  
- [ ] Yes — technology and specific versions **are** fully disclosed via headers or file signatures *(Informational)*  

### Are intermediary devices (WAFs, Load Balancers, Proxies) detectable in the communication path?
- [ ] No — no intermediaries are detected or they are fully transparent  
- [ ] Yes — existence is suspected via timing or behavior, but identity **is not** confirmed  
- [ ] Yes — specific products (e.g., Cloudflare, Nginx, F5) **are** identified via unique headers or behavior  

### Does the application leak information about its internal directory structure or network topology?
- [ ] No — internal paths and IPs are **not** disclosed  
- [ ] Yes — internal paths are leaked in error messages or stack traces but internal IPs **are not**  
- [ ] Yes — both internal directory structures and private IP addresses **are** disclosed  

### Can the backend database type and version be inferred from application behavior?
- [ ] No — database details **cannot** be determined  
- [ ] Yes — database type (e.g., PostgreSQL, MSSQL) is inferred via error messages or behavior  
- [ ] Yes — database type and version **are** explicitly disclosed in response bodies or headers  

### Is the application hosted on identifiable cloud service providers or specific third-party CDNs?
- [ ] No — hosting infrastructure is **not** identifiable  
- [ ] Yes — cloud provider (AWS, Azure, GCP) is identified but specific services **are not**  
- [ ] Yes — specific cloud services (e.g., S3 buckets, Lambda, Azure Functions) **are** mapped and reachable

---

## WSTG-CONF-01 — Test Network Infrastructure Configuration

Network infrastructure configuration testing involves identifying weaknesses in the underlying server and network components that support the web application. Attackers target misconfigured services, outdated protocols, and unnecessary open ports to gain a foothold, move laterally, or perform reconnaissance. Common findings include insecure TLS versions, verbose service banners, and exposed administrative interfaces that should be restricted to internal networks. By exploiting these infrastructure-level flaws, an adversary can compromise the confidentiality of data in transit or gain unauthorized access to the host operating system.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Tools:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Are there any unnecessary open ports or services exposed on the application host?
- [ ] No — only required ports (e.g., 443) are **accessible**  
- [ ] Yes — additional services are exposed but follow a **secure** configuration  
- [ ] Yes — non-essential services (e.g., FTP, Telnet, SMB) are **exposed** publicly  

### Do service banners or headers leak sensitive version information?
- [ ] No — banners are stripped or generic  
- [ ] Yes — banners reveal server software (e.g., Apache/nginx) but **not** version numbers  
- [ ] Yes — verbose banners reveal specific software versions, aiding **exploitation**  

### Is the TLS configuration following modern security best practices?
- [ ] Yes — only TLS 1.2/1.3 **enabled** with strong cipher suites  
- [ ] Yes — legacy protocols (TLS 1.0/1.1) are **enabled**  
- [ ] No — weak ciphers or insecure protocols (SSLv2/v3) are **enabled**  

### Are administrative or management interfaces restricted to authorized networks?
- [ ] Yes — interfaces (e.g., SSH, RDP, control panels) are **not accessible** from the public internet  
- [ ] No — interfaces are publicly **accessible** but require multi-factor authentication  
- [ ] No — interfaces are publicly **accessible** with single-factor authentication or default credentials

---

## WSTG-CONF-02 — Test Application Platform Configuration

Application platform configuration testing involves auditing the underlying web server, application server, and framework settings to ensure they are hardened against common exploits. Attackers actively seek out default credentials, unpatched platform vulnerabilities, and exposed administrative interfaces to gain unauthorized access or execute code. Misconfigurations often result in the disclosure of sensitive environment variables, internal system paths, or the presence of unnecessary sample applications that expand the attack surface. From a professional perspective, a poorly configured platform serves as a high-probability entry point for achieving initial access to the target environment.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if administrative interfaces are accessible with default credentials or if sample scripts allow for Remote Code Execution.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Are default credentials or accounts active on the platform?
- [ ] No — all default accounts are **disabled** or passwords changed  
- [ ] Yes — default accounts exist but are **not accessible** from the external network  
- [ ] Yes — default accounts are **active** and accessible via the internet *(Critical)*  

### Is the platform disclosing sensitive version information via headers or error pages?
- [ ] No — version strings are **hidden** or generic  
- [ ] Yes — detailed version information **is disclosed** in HTTP headers (e.g., `Server`, `X-Powered-By`)  
- [ ] Yes — full stack traces and internal system paths **are exposed** in verbose error messages  

### Are administrative or management interfaces properly restricted?
- [ ] No — administrative interfaces are **not exposed**  
- [ ] Yes — interfaces exist but are **restricted** by IP allowlisting or Multi-Factor Authentication  
- [ ] Yes — interfaces (e.g., `/admin`, `/manager`, `/console`) are **publicly accessible** with single-factor auth  

### Are sample files, scripts, or documentation present on the production server?
- [ ] No — all non-essential files have been **removed**  
- [ ] Yes — sample files or documentation **are present** but do not leak sensitive info  
- [ ] Yes — sample scripts (e.g., `info.php`, `examples/`) **are present** and provide a direct attack vector  

### Is the server configured to support insecure HTTP methods?
- [ ] No — only `GET` and `POST` are **enabled**  
- [ ] Yes — potentially risky methods like `PUT`, `DELETE`, or `TRACE` are **enabled** but restricted  
- [ ] Yes — risky methods are **enabled** and allow for unauthorized file modification or credential theft

---

## WSTG-CONF-03 — Test File Extensions Handling for Sensitive Information

Testing for file extension handling involves identifying whether the web server or application server reveals sensitive information by serving files that should be restricted or executed. Attackers frequently probe for backup files, configuration files, and source code fragments (e.g., `.bak`, `.old`, `.env`, `.inc`) that may be left in the web root and served as plain text due to a lack of specific handler mappings. This vulnerability matters because it can lead to the exposure of database credentials, API keys, and internal business logic, facilitating deeper exploitation of the infrastructure. These exposures typically occur in public directories, upload folders, or via misconfigured MIME-type settings on the server.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if configuration files containing credentials or full source code are accessible.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Are sensitive backup or temporary file extensions (e.g., `.bak`, `.old`, `.swp`, `~`) accessible?
- [ ] No — server returns 403 Forbidden or 404 Not Found for common backup extensions  
- [ ] Yes — server allows downloading of backup files but they **do not** contain sensitive data  
- [ ] Yes — server allows downloading of backup files containing sensitive data or source code *(High)*  

### Does the server expose environment or system configuration files (e.g., `.env`, `.config`, `.yml`, `.ini`)?
- [ ] No — restricted configuration files **cannot** be accessed and return 403/404  
- [ ] Yes — sensitive configuration files **are** accessible and disclose internal secrets or credentials  

### Can source code be retrieved by appending alternate extensions (e.g., `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] No — application code is **not** rendered as plain text regardless of extension manipulation  
- [ ] Yes — source code is revealed because the server **does not** have a handler for the appended extension  

### Are administrative or metadata directories (e.g., `.git/`, `.svn/`, `.DS_Store`) blocked from public access?
- [ ] No — these directories/files do **not** exist in the web root  
- [ ] Yes — access is **not possible** due to server-side rewrite rules or permissions  
- [ ] Yes — metadata directories **are** accessible and allow for full source code reconstruction  

### How does the server handle requests for files with multiple extensions (e.g., `file.php.jpg`)?
- [ ] No — the server correctly prioritizes the security of the internal extension or blocks the request  
- [ ] Yes — the server processes the file as the first extension, but bypass **is not possible** for execution  
- [ ] Yes — the server ignores the final extension, allowing code execution or source disclosure **is possible**

---

## WSTG-CONF-04 — Review Old Backup and Unreferenced Files for Sensitive Information

Reviewing old backup and unreferenced files involves identifying forgotten, temporary, or hidden files on a web server that are not intended for public access. These files often include source code backups (`.zip`, `.bak`), text editor swap files (`.swp`, `~`), or source control metadata (`.git`, `.svn`) that can leak sensitive information. Attackers use automated wordlists and discovery tools to brute-force common naming conventions, aiming to exfiltrate database credentials, hardcoded API keys, or logic that facilitates further exploitation. This vulnerability typically stems from manual server maintenance or flawed CI/CD pipelines that fail to sanitize the production web root.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if configuration files, source code archives, or credentials are discovered.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Are directory listings enabled on the web server?
- [ ] No — directory listing is **disabled** and returns a 403 Forbidden or custom error  
- [ ] Yes — directory listing is **enabled** on non-sensitive directories  
- [ ] Yes — directory listing is **enabled** on directories containing source code or sensitive files *(Critical)*  

### Are backup files with common extensions (e.g., .bak, .old, .save) discoverable?
- [ ] No — common backup extensions are **not found** or blocked by server configuration  
- [ ] Yes — backup files exist but do **not** contain sensitive information  
- [ ] Yes — backup files for configuration or source code **are** accessible *(High)*  

### Are source control directories (e.g., .git, .svn) or metadata exposed?
- [ ] No — source control metadata is **not present** or correctly blocked  
- [ ] Yes — metadata exists but the full repository **cannot** be reconstructed  
- [ ] Yes — the entire source code repository **can** be exfiltrated via exposed metadata  

### Are compressed archives (e.g., .zip, .tar.gz, .rar) of the application present in the web root?
- [ ] No — no sensitive archive files discovered via brute-force or enumeration  
- [ ] Yes — archives found but they are password protected or contain public assets  
- [ ] Yes — unencrypted archives containing application source or data **are** publicly accessible  

### Does the application leak temporary files created by text editors or IDEs?
- [ ] No — temporary files like `.swp`, `~`, or `.DS_Store` are **not present**  
- [ ] Yes — non-sensitive temporary files are **present**  
- [ ] Yes — temporary files reveal snippets of source code or internal paths

---

## WSTG-CONF-05 — Enumerate Infrastructure and Application Admin Interfaces

Administrative interfaces represent the management control plane of an application and its underlying infrastructure, often granting privileged access to system configurations, user data, and server-side operations. Attackers prioritize enumerating these interfaces through directory brute-forcing, analysis of `robots.txt`, or observing predictable URL patterns to find entry points that may lack robust authentication or rely on default credentials. Discovery of an exposed admin panel significantly increases the risk of complete system compromise, unauthorized data modification, or service disruption. These interfaces are frequently found on non-standard ports or subdomains, making them prime targets for automated scanners and manual reconnaissance.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the interface allows system-wide configuration changes, user management, or data exfiltration without multi-factor authentication.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Are administrative interfaces discoverable via directory fuzzing or reconnaissance?
- [ ] No — no administrative interfaces are exposed or discoverable  
- [ ] Yes — interfaces are discoverable but access **is restricted** to internal IP ranges  
- [ ] Yes — interfaces **are** discoverable and publicly accessible  

### Is authentication enforced on discovered administrative portals?
- [ ] Yes — multi-factor authentication (MFA) **is enforced**  
- [ ] Yes — single-factor authentication **is enforced**  
- [ ] No — no authentication is required to access the interface *(Critical)*  

### Are administrative paths hidden or non-standard to prevent discovery?
- [ ] Yes — paths use non-standard, randomized, or non-predictable naming  
- [ ] No — paths use common naming conventions (e.g., `/admin`, `/manager`, `/console`) and **can** be easily guessed  

### Does the interface provide access to sensitive system or application functionality?
- [ ] No — interface is a "read-only" status dashboard with minimal impact  
- [ ] Yes — interface **can** modify application configuration or user data  
- [ ] Yes — interface **can** execute system-level commands or perform database management

---

## WSTG-CONF-06 — Test HTTP Methods

Testing HTTP methods involves identifying which verbs are supported by the web server and application to ensure only necessary functionality is exposed. Beyond standard `GET` and `POST`, servers may inadvertently enable dangerous methods like `PUT`, `DELETE`, or `TRACE`, which can allow attackers to upload malicious files, delete existing content, or exfiltrate session cookies via Cross-Site Tracing (XST). Pentesters examine response headers such as `Allow` or `Public` and attempt to bypass restrictions using method overriding techniques or verb tampering to access unauthorized functionality. This test is crucial for hardening the attack surface and preventing misconfigurations that could lead to server compromise or unauthorized data manipulation.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium* |

> *Severity becomes High if `PUT` or `DELETE` allow unauthorized file manipulation or if `TRACE` facilitates session token exfiltration.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Are dangerous HTTP methods like `PUT` or `DELETE` disabled across the application?
- [ ] Yes — only safe methods (GET, POST, HEAD) **are enabled**  
- [ ] No — `PUT` or `DELETE` **are enabled** but require valid authentication  
- [ ] No — `PUT` or `DELETE` **are enabled** and accessible without authentication *(Critical)*  

### Is the `TRACE` method disabled to prevent Cross-Site Tracing (XST)?
- [ ] Yes — `TRACE` method **is disabled** or returns a 405 Method Not Allowed  
- [ ] No — `TRACE` method **is enabled** but does not reflect sensitive headers  
- [ ] No — `TRACE` method **is enabled** and reflects `Cookie` or `Authorization` headers *(Medium)*  

### Is HTTP Method Overriding (e.g., `X-HTTP-Method-Override`) supported by the server?
- [ ] No — server does **not** process method override headers  
- [ ] Yes — server processes override headers but access controls **are enforced**  
- [ ] Yes — server processes override headers and access control bypass **is possible**  

### Does the server respond securely to arbitrary or malformed HTTP methods?
- [ ] Yes — server returns a 405 Method Not Allowed or 501 Not Implemented  
- [ ] No — server returns a 200 OK or 500 Error for arbitrary methods like `JEFF` or `TEST`  
- [ ] No — using arbitrary methods allows bypassing authentication or authorization filters  

### Is the `OPTIONS` method restricted or configured to minimize information disclosure?
- [ ] Yes — `OPTIONS` is disabled or returns a minimal `Allow` header  
- [ ] No — `OPTIONS` reveals a wide range of enabled methods, including DAV or debugging verbs

---

## WSTG-CONF-07 — Test HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) is a security mechanism that allows a web server to inform browsers that it should only be accessed via HTTPS, preventing protocol downgrade attacks and cookie hijacking. By enforcing an encrypted connection, HSTS mitigates Man-in-the-Middle (MitM) attacks such as SSLStrip, where an attacker intercepts an initial insecure HTTP request to prevent the transition to TLS. From an attacker's perspective, the absence of this header or a short `max-age` duration provides a window of opportunity to intercept sensitive session tokens or credentials during the initial cleartext handshake. This test focuses on verifying the presence, duration, and scope of the `Strict-Transport-Security` header across all sensitive application endpoints.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium* |

> *Severity becomes Medium if the application handles PII, session tokens, or credentials, as the lack of HSTS increases the success rate of MitM attacks.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### Is the `Strict-Transport-Security` header present in HTTPS responses?
- [ ] Yes — header **is present** on all sensitive endpoints  
- [ ] Yes — header **is present** but only on the landing page  
- [ ] No — header **is missing** from the application entirely  

### Is the `max-age` directive set to a sufficient duration?
- [ ] Yes — `max-age` is set to one year or more (e.g., `31536000`)  
- [ ] Yes — `max-age` is set but is **too short** (e.g., less than 6 months)  
- [ ] No — `max-age` directive **is missing** or set to `0` *(Critical)*  

### Does the HSTS policy cover all subdomains?
- [ ] Yes — `includeSubDomains` directive **is applied**  
- [ ] No — `includeSubDomains` directive **is missing**, leaving subdomains vulnerable to downgrade  
- [ ] No — feature does not apply as no subdomains exist  

### Is the application registered for HSTS Preloading?
- [ ] Yes — `preload` directive **is present** and domain is in the HSTS preload list  
- [ ] Yes — `preload` directive **is present** but domain is **not** yet in the preload list  
- [ ] No — `preload` directive **is missing**, allowing a window for MitM on the first visit  

### Is the HSTS header incorrectly sent over plaintext HTTP?
- [ ] No — header is **only** sent over secure HTTPS connections  
- [ ] Yes — header **is sent** over HTTP, which is ignored by browsers and may leak configuration details

---

## WSTG-CONF-08 — Test RIA Cross Domain Policy

Rich Internet Application (RIA) cross-domain policies involve files like `crossdomain.xml` and `clientaccesspolicy.xml` which define how web clients—specifically Adobe Flash and Microsoft Silverlight—handle cross-origin requests. These files are critical for security because they explicitly override the Same-Origin Policy (SOP), determining which external domains are permitted to interact with the application and read its responses. Overly permissive configurations, such as using wildcards in the `domain` attribute, allow attackers to host malicious RIA objects on a third-party site that can exfiltrate sensitive data or perform actions on behalf of authenticated users. Pentesters typically locate these files at the web root to evaluate the level of trust granted to third-party origins and identify potential cross-origin data leaks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the application handles sensitive user data or session identifiers that can be exfiltrated via a permissive policy.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Are cross-domain policy files (`crossdomain.xml` or `clientaccesspolicy.xml`) present in the web root?
- [ ] No — files **cannot** be found in the root directory  
- [ ] Yes — `crossdomain.xml` (Flash) **is** present  
- [ ] Yes — `clientaccesspolicy.xml` (Silverlight) **is** present  
- [ ] Yes — both RIA policy files **are** present  

### Is the `allow-access-from` domain attribute restricted to trusted origins?
- [ ] No — files exist but contain no `allow-access-from` entries  
- [ ] Yes — specific, trusted domains are explicitly whitelisted and bypass **not possible**  
- [ ] Yes — domains are whitelisted but include untrusted third-party or sub-domains  
- [ ] Yes — a wildcard `*` **is used**, allowing access from **any** origin *(Critical)*  

### Does the policy permit insecure communication over HTTP for HTTPS resources?
- [ ] No — `secure="true"` is set (or defaulted), preventing HTTP origins from accessing HTTPS data  
- [ ] Yes — `secure="false"` is explicitly set, allowing MITM attackers to exfiltrate secure data  

### Are HTTP headers overly permissive in the cross-domain configuration?
- [ ] No — `allow-http-request-headers-from` is restricted or **not enabled**  
- [ ] Yes — specific headers are allowed for trusted domains  
- [ ] Yes — wildcard `*` is used for headers, allowing attackers to send custom headers from any domain  

### Is the `site-control` (master-policy) configuration restrictive?
- [ ] No — `permitted-cross-domain-policies` is set to `none` or `master-only`  
- [ ] Yes — the policy allows other files on the server to define their own rules (`all`)

---

## WSTG-CONF-09 — Test File Permission

Testing file permissions involves auditing the access control levels assigned to files and directories on the web server to ensure that sensitive resources are not over-exposed to unauthorized users or processes. Improperly configured permissions can allow attackers to read sensitive configuration files containing database credentials, view proprietary source code, or modify server-side scripts to achieve remote code execution. This vulnerability typically occurs during the deployment phase when default "world-readable" or "world-writable" flags are left on critical application directories, such as configuration paths, log folders, or upload repositories. From an attacker's perspective, discovering an improperly secured file often serves as the primary catalyst for privilege escalation or full system compromise.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if sensitive configuration files (e.g., `.env`, `web.config`) are readable or if write access to the web root is possible.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Tools:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Are sensitive application configuration files protected from unauthorized access?
- [ ] Yes — configuration files (e.g., `.env`, `config.php`) are **not accessible** via web requests  
- [ ] Yes — files are present but access is **restricted** to authorized local users only  
- [ ] No — sensitive configuration files **are accessible** and disclose credentials or secrets  

### Can an attacker modify files or directories within the web root?
- [ ] No — all web root directories are **read-only** for the web server user  
- [ ] Yes — writable directories exist but script execution is **disabled**  
- [ ] Yes — world-writable directories exist and **allow** for the upload and execution of scripts *(Critical)*  

### Are version control metadata or backup files exposed due to lax permissions?
- [ ] No — `.git`, `.svn`, and backup files (e.g., `.bak`, `~`) are **not present** or protected  
- [ ] Yes — metadata directories exist but directory listing is **disabled**  
- [ ] Yes — sensitive metadata or backups are **fully accessible**, allowing for source code recovery  

### Does the web server user operate with the principle of least privilege?
- [ ] Yes — the web server process runs as a **dedicated low-privilege** user  
- [ ] No — the web server process runs with **unnecessary privileges** (e.g., `root` or `Administrator`)  

### Are log files secured against unauthorized reading or modification?
- [ ] Yes — log files are **restricted** to administrative users  
- [ ] Yes — log files are readable but do **not** contain session tokens or PII  
- [ ] No — log files are **publicly readable** and disclose sensitive transaction data or user info

---

## WSTG-CONF-10 — Subdomain Takeover

Subdomain takeover occurs when a DNS entry (typically a CNAME, but occasionally A or MX records) points to a decommissioned or non-existent resource on a third-party cloud provider or hosting service. Attackers identify these "dangling" DNS records by looking for specific error messages from providers like AWS, GitHub, or Azure that indicate a resource is no longer claimed. By registering the abandoned resource name on the provider's platform, the attacker gains full control over the subdomain, allowing them to host malicious content, exfiltrate session cookies scoped to the base domain, and bypass Content Security Policy (CSP) or CORS protections. This vulnerability is particularly dangerous because it leverages the trust associated with the organization's legitimate domain structure to facilitate high-impact phishing and data theft.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical* |

> *Severity becomes Critical if the subdomain can be used to hijack session cookies from the base domain or bypass SSO/OAuth redirects.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Tools:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### Does the application utilize third-party hosting or cloud services via subdomains?
- [ ] No — all subdomains point to organization-controlled IP space  
- [ ] Yes — subdomains point to third-party providers (S3, GitHub Pages, Heroku, etc.)  

### Are there any "dangling" DNS records pointing to non-existent resources?
- [ ] No — all identified DNS records resolve to active, controlled resources  
- [ ] Yes — CNAME or A records exist for resources that **no longer exist** at the provider  
- [ ] Yes — DNS records resolve to NXDOMAIN or provider-specific error pages *(e.g., "NoSuchBucket")*  

### Can the identified dangling resource be successfully claimed?
- [ ] No — provider has protections against claiming abandoned names or account verification is required  
- [ ] Yes — the resource name **can** be registered on the third-party platform by any user  

### Does the base domain use "Domain" scoped cookies that could be compromised?
- [ ] No — cookies are strictly scoped to specific subdomains or use `Host-Only` flags  
- [ ] Yes — sensitive cookies (session IDs, JWTs) are scoped to the parent domain and **can** be exfiltrated via a hijacked subdomain  

### Can the subdomain be used to bypass security headers or authentication flows?
- [ ] No — no security dependencies on the identified subdomains  
- [ ] Yes — the subdomain is whitelisted in CSP `script-src` or `connect-src` directives  
- [ ] Yes — the subdomain is a valid redirect URI for OAuth or SSO configurations

---

## WSTG-CONF-11 — Test Cloud Storage

Testing for cloud storage misconfigurations involves identifying and auditing publicly accessible object storage services such as Amazon S3, Azure Blobs, or Google Cloud Storage. These resources often contain sensitive data including backups, source code, PII, or configuration files that are exposed due to overly permissive Access Control Lists (ACLs) or Identity and Access Management (IAM) policies. Attackers enumerate bucket names through reconnaissance of JavaScript files, HTML source, and brute-force techniques to find entry points for data exfiltration or unauthorized file modification. Exploitation can lead to significant impact, including full data breaches or the ability to serve malicious files from a trusted domain.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical* |

> *Severity becomes Critical if PII, credentials, or production backups are accessible, or if unauthenticated write access is enabled.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Are cloud storage endpoints (buckets/containers) identified and enumerated?
- [ ] No — no cloud storage endpoints are used or discoverable  
- [ ] Yes — storage endpoints are identified via code analysis or traffic monitoring  
- [ ] Yes — storage endpoints are discovered via naming convention brute-force  

### Can unauthenticated users list the contents of the cloud storage?
- [ ] No — bucket listing is **disabled** and returns 403 Forbidden or 404 Not Found  
- [ ] Yes — listing is **enabled** but no sensitive files are present  
- [ ] Yes — listing is **enabled** and sensitive file names/metadata are visible  

### Is the downloading of sensitive files from identified buckets possible?
- [ ] No — files are protected by IAM policies or valid authentication is required  
- [ ] Yes — files are accessible but require a signed URL that **cannot** be easily guessed  
- [ ] Yes — sensitive files (backups, `.env`, PII) are **publicly accessible** for download *(Critical)*  

### Is unauthenticated file upload or modification permitted?
- [ ] No — unauthenticated uploads or modifications are **not possible**  
- [ ] Yes — authenticated upload is required but bypass **is possible** via weak policy conditions  
- [ ] Yes — unauthenticated writing, overwriting, or deleting of files is **possible** *(Critical)*  

### Are Cross-Origin Resource Sharing (CORS) policies on the storage endpoint restrictive?
- [ ] Yes — CORS is **disabled** or restricted to specific trusted origins  
- [ ] No — CORS is **enabled** with a wildcard `*` origin, allowing unauthorized web-based access

---

## WSTG-CONF-12 — Content Security Policy (CSP)

Content Security Policy (CSP) is a critical defense-in-depth mechanism implemented via HTTP response headers to mitigate risks such as Cross-Site Scripting (XSS), clickjacking, and data injection attacks. By defining which dynamic resources are allowed to load, a well-configured CSP restricts an attacker's ability to execute unauthorized scripts or exfiltrate sensitive data to external domains. Pentesters evaluate the policy for overly permissive directives, the use of unsafe keywords like `'unsafe-inline'`, and reliance on insecure CDNs that might facilitate bypasses. A weak or missing CSP does not create a vulnerability directly but significantly increases the impact of successful injection attacks by failing to provide a secondary layer of protection.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Tools:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Is a Content Security Policy (CSP) header present in the application responses?
- [ ] Yes — `Content-Security-Policy` header is **present** and **enforced**  
- [ ] Yes — `Content-Security-Policy-Report-Only` is **present** for testing  
- [ ] No — no CSP header is **present** in the responses  

### Are the `script-src` or `default-src` directives properly restricted?
- [ ] Yes — directives use strict whitelisting, nonces, or hashes and bypass **not possible**  
- [ ] Yes — directives are **properly configured** but whitelist includes known bypassable CDNs  
- [ ] No — directives use wildcards (`*`) or `data:` schemes, making bypass **possible**  

### Does the policy allow the execution of unsafe inline scripts or eval?
- [ ] No — `'unsafe-inline'` and `'unsafe-eval'` are **not present**  
- [ ] Yes — `'unsafe-inline'` is **present** but protected by a `nonce` or `hash`  
- [ ] Yes — `'unsafe-inline'` or `'unsafe-eval'` are **enabled** without additional protections *(Critical)*  

### Is the application protected against clickjacking via the `frame-ancestors` directive?
- [ ] Yes — `frame-ancestors` is set to `'none'` or `'self'`  
- [ ] Yes — `frame-ancestors` is **enabled** but allows specific trusted third-party domains  
- [ ] No — `frame-ancestors` is **missing**, relying on legacy `X-Frame-Options` or no protection  

### Is there a mechanism to report CSP violations?
- [ ] Yes — `report-uri` or `report-to` is **configured** and active  
- [ ] No — violation reporting is **disabled** or **not configured**

---

## WSTG-CONF-13 — Path Confusion

Path Confusion arises from discrepancies in how different web components, such as reverse proxies, load balancers, and backend application servers, parse and interpret URL paths. Attackers exploit these inconsistencies by injecting specific characters like semicolons, encoded slashes, or dot-segments to deceive security filters and access restricted endpoints or static resources. This vulnerability can lead to critical security failures, including authentication bypass, unauthorized data access, and web cache poisoning, as the front-end may apply security rules to a path that the back-end interprets differently. Successful exploitation typically occurs at the architectural boundary where request routing and normalization logic diverge across the tech stack.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if path confusion results in a bypass of authentication or access control for sensitive administrative endpoints.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Do different architectural layers interpret path delimiters (e.g., `;`, `#`, `?`) consistently?
- [ ] Yes — all layers normalize and interpret path delimiters identically  
- [ ] No — inconsistencies exist but **cannot** be used to access restricted resources  
- [ ] No — discrepancies allow an attacker to bypass front-end filters and reach back-end logic **is possible**  

### Can access controls be bypassed using path traversal sequences or encoded characters?
- [ ] No — normalization is applied consistently before security checks  
- [ ] Yes — bypass is **possible** via dot-segment sequences (e.g., `/admin/..;/`)  
- [ ] Yes — bypass is **possible** via URL-encoded characters (e.g., `%2f`, `%2e%2e%2f`)  

### Is the application susceptible to Web Cache Poisoning through path confusion?
- [ ] No — caching logic is not affected by path ambiguity or extra path info  
- [ ] Yes — malicious content **can** be cached for legitimate paths due to mapping discrepancies  
- [ ] Yes — sensitive information **can** be cached in public directories via `RCD` (Relative Path Overwrite) techniques  

### Does the server handle "extra path information" (PathInfo) securely?
- [ ] No — feature is **disabled** or does not exist  
- [ ] Yes — `PathInfo` is handled and does **not** interfere with security filters  
- [ ] No — `PathInfo` allows an attacker to append data that confuses the application's routing logic

---

## WSTG-CONF-14 — Test Other HTTP Security Header Misconfigurations

Testing for HTTP security header misconfigurations involves verifying the presence and correct implementation of defensive headers that provide essential protection against common web-based attack vectors. While these headers do not fix underlying code vulnerabilities, their absence significantly increases the risk of man-in-the-middle (MITM) attacks, MIME-sniffing exploitation, and unintended cross-origin data leakage via referrers. From an attacker's perspective, missing security headers are high-visibility indicators of a poorly hardened environment, often facilitating more complex exploitation chains like session hijacking or information exfiltration. These configurations are typically evaluated at the server level and should be consistently applied across all response types, including API endpoints and static resources.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Security Header Presence Audit
| Header | Present | Missing | Recommended Configuration |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` or `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Feature-specific, e.g., `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### How are security headers enforced across the application scope?
- [ ] Yes — headers are globally applied via a central load balancer or reverse proxy and **cannot** be overridden  
- [ ] Yes — headers are applied to main document responses but are **missing** on API responses or static assets  
- [ ] No — headers are applied inconsistently or are **missing** across the entire application domain  

### Is the Strict-Transport-Security (HSTS) header correctly configured?
- [ ] Yes — `max-age` is set to a long duration (e.g., 2 years) and `includeSubDomains` is **enabled**  
- [ ] Yes — `max-age` is set but `includeSubDomains` is **disabled**, leaving subdomains vulnerable  
- [ ] No — HSTS header is **not present** or `max-age` is set to 0, allowing protocol downgrade attacks  

### Does the Referrer-Policy prevent the leakage of sensitive URL parameters?
- [ ] Yes — policy is set to `no-referrer` or `same-origin` *(Most secure)*  
- [ ] Yes — policy is set to `strict-origin-when-cross-origin`  
- [ ] No — policy is **not set** or is set to `unsafe-url`, potentially leaking tokens or sensitive PII in referer headers  

### Is the X-Content-Type-Options header preventing MIME-sniffing?
- [ ] Yes — `nosniff` directive is **present** and correctly implemented  
- [ ] No — header is **missing**, potentially allowing a browser to execute non-executable files (e.g., images) as scripts

---

## WSTG-IDNT-01 — Test Role Definitions

Testing for role definitions involves identifying and documenting the various user roles and permission levels defined within the application to ensure they are logically separated and adhere to the principle of least privilege. An attacker analyzes the application to map out high-privilege roles versus standard user roles, looking for any ambiguity in permission boundaries or undocumented administrative functions. Identifying these roles is the first step in discovering privilege escalation paths, as inconsistencies in role definitions often lead to unauthorized access to sensitive data or administrative features. This phase typically occurs during initial reconnaissance and business logic mapping, focusing on user profiles, administrative dashboards, and API permission structures.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Are roles clearly defined and documented in the application or its documentation?
- [ ] Yes — roles are fully documented and follow the principle of **least privilege**  
- [ ] Yes — roles are documented but contain **overlapping permissions**  
- [ ] No — no formal role documentation or definitions exist  

### Is there a clear separation between administrative and standard user functions?
- [ ] Yes — administrative functions are **completely isolated** from standard user views  
- [ ] Yes — separation is present but administrative endpoints are **predictable or guessable**  
- [ ] No — administrative and standard functions are **mixed** within the same interfaces  

### Does the application use a centralized mechanism for Role-Based Access Control (RBAC)?
- [ ] Yes — centralized RBAC or ABAC is **implemented** and consistently enforced  
- [ ] Yes — decentralized controls exist but are **inconsistently applied** across modules  
- [ ] No — access control logic is **hardcoded** into individual pages or components  

### Are there undocumented or hidden roles present in the system?
- [ ] No — all discovered roles match documented functionality  
- [ ] Yes — hidden or "shadow" roles (e.g., `super_admin`, `support`, `test`) **are present**  

### Can a low-privileged user enumerate the permissions or roles of other users?
- [ ] No — users **cannot** view or enumerate the roles/permissions of other entities  
- [ ] Yes — users **can** enumerate roles through profile pages, metadata, or API responses  *(Medium)*

---

## WSTG-IDNT-02 — Test User Registration Process

Testing the user registration process involves evaluating the workflow through which new identities are created to ensure it cannot be abused for unauthorized access or automated exploitation. Vulnerabilities in this process often manifest as lack of identity verification (email/SMS), susceptibility to mass account creation via bots, or information disclosure through verbose error messages that reveal existing usernames. Attackers exploit these flaws to perform user enumeration, conduct large-scale spam campaigns, or manipulate registration parameters to gain elevated privileges during the account creation phase. This test is vital for protecting the application's integrity and preventing attackers from populating the system with fraudulent or malicious accounts.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Is identity verification required before a new account becomes active?
- [ ] Yes — registration requires a unique, time-bound token sent via email or SMS  
- [ ] Yes — verification is required but tokens are **weak** or **predictable**  
- [ ] No — accounts are activated **immediately** without any verification step  

### Does the registration process prevent user enumeration?
- [ ] Yes — the application returns **identical** responses for both new and existing emails/usernames  
- [ ] No — error messages (e.g., "Email already in use") **allow** an attacker to map registered users  
- [ ] No — timing differences in server responses **reveal** whether a user exists  

### Are anti-automation controls implemented to prevent mass registration?
- [ ] Yes — CAPTCHA or proof-of-work mechanisms are **present** and **effective**  
- [ ] Yes — controls exist but bypass **is possible** via API endpoints or header manipulation  
- [ ] No — no rate limiting or CAPTCHA is **applied** to the registration endpoint  

### Can registration parameters be manipulated to escalate privileges?
- [ ] No — user roles are assigned **strictly** by the server-side logic  
- [ ] Yes — parameters such as `role`, `admin`, or `group_id` **can** be modified in the request to gain higher access  

### Is the password policy enforced during the registration phase?
- [ ] Yes — strong password requirements are **enforced** server-side  
- [ ] No — weak passwords (e.g., "password123") are **accepted** by the system

---

## WSTG-IDNT-03 — Test Account Provisioning Process

The account provisioning process governs how new identities are created, verified, and assigned privileges within an application environment. Attackers target this workflow to perform automated mass-registrations for spam or denial-of-service, bypass identity verification steps to create fraudulent accounts, or manipulate parameters to grant themselves elevated permissions during the signup phase. Vulnerabilities often manifest in self-service registration forms, invitation-only systems, or administrative provisioning consoles where business logic flaws allow for unauthorized account creation or privilege escalation. From an attacker's perspective, compromising the provisioning flow provides a legitimate foothold that can be used as a staging ground for deeper exploitation, data exfiltration, or social engineering attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes Critical if an attacker can provision administrative accounts or bypass global registration restrictions.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### Is self-registration strictly controlled or limited to authorized users?
- [ ] Yes — registration is **disabled** or requires manual administrative approval  
- [ ] Yes — registration is open but restricted to **authorized** email domains or invitation tokens  
- [ ] No — registration is **open** to any user with no domain or token restrictions  

### Is a robust identity verification mechanism (e.g., email/SMS) required before account activation?
- [ ] Yes — verification is **required** and **cannot** be bypassed  
- [ ] Yes — verification is **required** but **can** be bypassed via parameter tampering or predictable tokens  
- [ ] No — accounts are **active immediately** upon submission without any verification  

### Can the user manipulate role or permission parameters during the registration request?
- [ ] No — roles are **assigned server-side** and no client-side parameters exist  
- [ ] Yes — role parameters exist but manipulation is **not possible** due to server-side validation  
- [ ] Yes — role/permission parameters (e.g., `role=admin`, `is_staff=true`) **can** be modified to gain elevated access *(Critical)*  

### Are rate limits or anti-automation controls implemented to prevent mass account creation?
- [ ] Yes — rate limits and CAPTCHAs are **active** and effective  
- [ ] Yes — controls exist but bypass **is possible** via header spoofing or CAPTCHA solving services  
- [ ] No — no rate limits or anti-automation controls are **applied**  

### Does the provisioning process leak information about existing accounts (User Enumeration)?
- [ ] No — response messages are **identical** for existing and non-existing users  
- [ ] Yes — different responses or timing variations **allow** for the enumeration of existing users

---

## WSTG-IDNT-04 — Testing for Account Enumeration and Guessable User Account

Account enumeration occurs when an application reveals the existence or non-existence of a user account through variations in HTTP responses, error messages, or measurable timing differences. Attackers exploit this behavior by systematically submitting lists of usernames to identify valid accounts, which are subsequently targeted for credential stuffing, brute-force attacks, or social engineering. This vulnerability commonly manifests in login portals, password reset features, registration forms, and API endpoints that handle user-specific identifiers. From an attacker's perspective, successful enumeration significantly narrows the attack surface by transforming a blind brute-force attempt into a targeted exploitation of known, valid identities.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Are authentication error messages consistent across all scenarios?
- [ ] Yes — generic error messages are used for both invalid usernames and invalid passwords  
- [ ] Yes — messages are similar, but slight variations in punctuation or casing **are present**  
- [ ] No — error messages explicitly state "User not found" or "Incorrect password" *(Vulnerable)*  

### Does the password reset or "forgot password" flow leak account existence?
- [ ] No — the application returns a generic message regardless of whether the email/username exists  
- [ ] Yes — controls are in place, but bypass **is possible** via response length or status codes  
- [ ] Yes — the application explicitly confirms if a reset email was sent or if the account **does not exist**  

### Is the registration process protected against user enumeration?
- [ ] No — registration does not leak information or uses CAPTCHA/rate-limiting to prevent bulk checks  
- [ ] Yes — controls are in place, but bypass **is possible** via timing or API response differences  
- [ ] Yes — the application immediately notifies the user if the username or email **is already taken**  

### Are timing differences measurable when comparing valid vs. invalid usernames?
- [ ] No — response times are consistent regardless of account validity due to constant-time comparisons  
- [ ] Yes — timing differences exist but are too small to be reliably measured over the network  
- [ ] Yes — measurable timing discrepancies **are present** (e.g., due to password hashing execution only for valid users)  

### Does the application use predictable or guessable naming conventions for accounts?
- [ ] No — account identifiers are random (UUIDs) or user-defined and complex  
- [ ] Yes — account identifiers follow a pattern (e.g., `emp001`, `emp002`) and enumeration **is possible**  
- [ ] Yes — identifiers are sequential integers, making full database enumeration **possible**

---

## WSTG-IDNT-05 — Testing for Weak or Unenforced Username Policy

Testing for weak or unenforced username policies focuses on the rules governing account identifier creation and their susceptibility to enumeration or spoofing. Weak policies often permit predictable sequences, common dictionary words, or identifiers that mirror internal employee IDs, which significantly reduces the search space for brute-force and credential stuffing attacks. From an attacker's perspective, identifying these patterns is the first step in large-scale account discovery, often achieved by analyzing registration error messages, password reset behavior, or metadata in public-facing profiles. Ensuring a robust policy prevents attackers from systematically mapping the user base and facilitates defense against targeted phishing and automated authentication bypass attempts.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Are usernames based on highly predictable or sequential patterns?
- [ ] No — usernames are random, user-defined, or high-entropy strings  
- [ ] Yes — usernames follow a predictable format (e.g., `user1001`, `user1002`) but authentication is robust  
- [ ] Yes — usernames are **strictly sequential** or follow a known corporate format, facilitating easy enumeration  

### Does the application enforce minimum length and complexity requirements for usernames?
- [ ] Yes — strict length and character set policies **are enforced** during registration  
- [ ] Yes — policies exist but allow extremely short (e.g., 1-2 characters) or overly simple usernames  
- [ ] No — **no policy** is enforced regarding username length or character types  

### Can valid usernames be enumerated through application responses?
- [ ] No — application returns generic error messages and exhibits consistent timing for all attempts  
- [ ] Yes — application returns distinct error messages (e.g., "Username already taken") but rate limiting **is applied**  
- [ ] Yes — application **leaks** valid usernames through registration errors, password resets, or timing differences  

### Does the policy prevent the registration of common or reserved administrative usernames?
- [ ] Yes — application blocks common names (e.g., `admin`, `root`, `support`) and system-reserved terms  
- [ ] No — users **can** register accounts with sensitive or administrative names  

### Is the username policy consistent across all entry points (API, Mobile, Web)?
- [ ] Yes — policies **are applied** consistently across all interfaces and versions  
- [ ] No — legacy endpoints or specific API versions **do not** enforce the same username constraints

---

## WSTG-ATHN-01 — Testing for Credentials Transported over an Encrypted Channel

Testing for credentials transported over an encrypted channel ensures that sensitive authentication data, such as usernames, passwords, and session tokens, are protected from interception during transit. Attackers positioned on the network—such as through Man-in-the-Middle (MitM) attacks on public Wi-Fi or compromised internal networks—can use packet sniffing tools to capture cleartext credentials if TLS/SSL is not properly enforced. This vulnerability typically occurs at login endpoints, password reset forms, and API authentication headers where the application fails to mandate HTTPS or improperly redirects from HTTP. Successful exploitation grants the attacker full access to user accounts, leading to total compromise of user data and potential lateral movement within the application.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Is HTTPS enforced for the entire authentication process?
- [ ] Yes — HSTS is **enabled** and all traffic is strictly forced over HTTPS  
- [ ] Yes — redirection to HTTPS occurs, but HSTS is **not enabled**  
- [ ] No — credentials **can** be submitted over unencrypted HTTP  

### Are login pages and forms served over an encrypted channel?
- [ ] Yes — the page containing the login form is delivered via HTTPS  
- [ ] No — the login page is served over HTTP, allowing for form-action hijacking or credential sniffing  

### Is the `Secure` flag applied to all sensitive session cookies?
- [ ] Yes — `Secure` flag is **applied** to all authentication and session cookies  
- [ ] Yes — `Secure` flag is **applied** only to some cookies  
- [ ] No — `Secure` flag is **not applied**, allowing cookies to be exfiltrated via unencrypted requests  

### Does the server support weak TLS versions or insecure cipher suites?
- [ ] No — only modern TLS (1.2+) and strong ciphers are **enabled**  
- [ ] Yes — legacy versions (TLS 1.0/1.1) or weak ciphers (RC4, DES, CBC) are **supported**  

### Does the application prevent credential transmission to third-party domains over unencrypted channels?
- [ ] Yes — no sensitive data is sent to external domains or all external calls are forced over HTTPS  
- [ ] No — authentication headers or credentials are sent to third-party endpoints (e.g., analytics or SSO) via HTTP

---

## WSTG-ATHN-02 — Testing for Default Credentials

Testing for default credentials involves identifying administrative interfaces, services, or application components that still use factory-set or vendor-provided usernames and passwords. This vulnerability is a high-priority target for attackers because it often provides an immediate path to full system compromise, administrative access, or remote code execution with minimal effort. It typically occurs in off-the-shelf software, CMS platforms, database management tools, and integrated hardware like IoT devices or network appliances. Exploitation is usually performed by cross-referencing identified software versions against public default password databases or using automated credential stuffing tools during the initial reconnaissance and exploitation phases.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Test Status** | Not Performed |
| **Severity** | Critical / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Are administrative or management interfaces exposed to the internet or untrusted network segments?
- [ ] No — no management interfaces are accessible  
- [ ] Yes — interfaces are present but restricted by network-level controls (e.g., VPN, IP Allowlist)  
- [ ] Yes — interfaces **are** publicly accessible and require authentication  

### Have vendor-supplied default credentials been changed for all identified components?
- [ ] Yes — all default credentials **have been changed** across all components *(Most secure)*  
- [ ] Yes — credentials **have been changed** for the main application, but secondary components (e.g., CMS, DB) remain default  
- [ ] No — default credentials **are active** on one or more identifiable interfaces *(Critical)*  

### Does the application enforce a password change upon first login for new or administrative accounts?
- [ ] Yes — a password change **is mandatory** before any administrative action can be taken  
- [ ] No — the application **allows** the use of default credentials indefinitely  

### Are lockout mechanisms or rate-limiting controls active to prevent automated default credential testing?
- [ ] Yes — aggressive rate limiting or account lockout **is applied**  
- [ ] Yes — controls are present but bypass **is possible** via header manipulation or IP rotation  
- [ ] No — no rate limiting or lockout mechanisms **are enabled**  

### Do third-party plugins, middleware, or administrative tools (e.g., phpMyAdmin, Tomcat Manager) use unique credentials?
- [ ] No — third-party components do not exist in the environment  
- [ ] Yes — all third-party components use unique, non-default credentials  
- [ ] No — at least one third-party component **is accessible** via known default credentials

---

## WSTG-ATHN-03 — Testing for Weak Lock Out Mechanism

An account lockout mechanism is a critical defense-in-depth control designed to mitigate brute-force and credential stuffing attacks by disabling an account after a specified number of failed authentication attempts. Without a robust lockout policy, attackers can use automated tools to systematically guess passwords across multiple accounts (password spraying) or target a single high-value account until successful. This vulnerability typically manifests on login portals, password reset forms, and API endpoints that lack rate limiting or account-level protection. From an attacker's perspective, a weak or absent lockout mechanism allows for infinite password attempts, while an overly aggressive one can be exploited to cause a distributed Denial of Service (DoS) against legitimate users by intentionally locking their accounts.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the application handles sensitive PII, financial data, or if administrative accounts are susceptible to brute-force without any secondary controls.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Is an account lockout mechanism enforced after a set number of failed attempts?
- [ ] Yes — account is locked after a defined, reasonable threshold (e.g., 5-10 attempts)  
- [ ] Yes — account is locked but only after an excessively high number of attempts  
- [ ] No — account **cannot** be locked and allows for indefinite brute-force  

### Can the lockout mechanism be bypassed using common evasion techniques?
- [ ] No — lockout is enforced server-side and is **not** affected by client-side changes  
- [ ] Yes — lockout **is possible** to bypass by rotating IP addresses or using a proxy pool  
- [ ] Yes — lockout **is possible** to bypass by manipulating HTTP headers like `X-Forwarded-For` or `User-Agent`  
- [ ] Yes — lockout **is possible** to bypass by clearing cookies or starting new sessions  

### Does the lockout mechanism provide a path for account recovery or expiration?
- [ ] Yes — account is automatically unlocked after a set duration or via email verification  
- [ ] Yes — account requires administrative intervention to unlock  
- [ ] No — lockout is permanent with no clear recovery path, increasing DoS risk  

### Does the application differentiate between a valid and invalid username during lockout?
- [ ] No — the application response is identical for both existing and non-existing users  
- [ ] Yes — the application reveals if an account is locked, allowing for **username enumeration**  

### Is the lockout mechanism susceptible to a Denial of Service (DoS) attack?
- [ ] No — controls like CAPTCHA or IP-based rate limiting prevent mass account locking  
- [ ] Yes — an attacker **can** systematically lock out all known users by providing incorrect passwords

---

## WSTG-ATHN-04 — Testing for Bypassing Authentication Schema

Bypassing the authentication schema involves identifying and exploiting flaws in the application logic that allow an attacker to gain access to protected resources without providing valid credentials. This vulnerability typically occurs when the application relies on client-side controls, fails to enforce server-side authorization on every request, or leaves administrative "backdoors" and debug endpoints exposed in production. Attackers exploit these weaknesses by performing forced browsing to sensitive URLs, manipulating HTTP parameters like `authenticated=true`, or spoofing headers to trick the application into assuming a session is already established. Successful exploitation results in a complete breakdown of the authentication perimeter, potentially leading to unauthorized data exfiltration or full system compromise.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Test Status** | Not Performed |
| **Severity** | Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Can sensitive internal pages be accessed directly without an active session?
- [ ] No — direct access results in redirection to login or a 403/404 response  
- [ ] Yes — some sensitive pages are accessible but provide limited or non-sensitive data  
- [ ] Yes — sensitive administrative or user-specific pages **are** fully accessible without authentication *(Critical)*  

### Does the application rely on client-side flags or parameters to determine authentication status?
- [ ] No — authentication status is managed strictly via server-side session state  
- [ ] Yes — client-side flags exist but modification **does not** grant access to protected areas  
- [ ] Yes — modifying parameters (e.g., `is_authenticated=true`, `user_role=admin`) **allows** authentication bypass  

### Can authentication be bypassed by spoofing or manipulating specific HTTP headers?
- [ ] No — headers such as `X-Forwarded-For`, `Referer`, or custom headers have no impact on authentication  
- [ ] Yes — manipulating headers (e.g., `X-Custom-IP-Authorization`, `X-Remote-User`) **is possible** and grants unauthorized access  

### Are there alternative paths or development artifacts that circumvent the standard login flow?
- [ ] No — all protected resources are gated by a centralized, production-ready authentication check  
- [ ] Yes — legacy endpoints, debug routes (e.g., `/debug/login`), or forgotten API versions **are** accessible without credentials  

### Does the application fail to re-authenticate or verify sessions for high-privilege actions?
- [ ] No — high-privilege or sensitive actions require a fresh or valid session token  
- [ ] Yes — once a bypass is found on one endpoint, it **can** be used to perform administrative actions across the application

---

## WSTG-ATHN-05 — Testing for Vulnerable Remember Password

The "Remember Me" or persistent login functionality is designed to maintain a user's authenticated state across browser restarts by storing a token or credential on the client side. If this feature is poorly implemented—such as by storing plaintext passwords, weak hashes, or low-entropy tokens in cookies—it exposes the user to account takeover through Cross-Site Scripting (XSS), physical access, or network interception. Pentesters evaluate the entropy of these persistence tokens, the security flags applied to the storage mechanism, and the lifecycle of the session to ensure that the convenience of persistent access does not bypass critical security controls like multi-factor authentication or session invalidation. Exploitation often involves harvesting these long-lived tokens to gain unauthorized access to accounts without requiring the original credentials.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if plaintext credentials or unsalted hashes are stored in the persistent cookie, or if the token bypasses Multi-Factor Authentication (MFA).

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### Does the application implement a "Remember Me" or "Stay Logged In" feature?
- [ ] No — feature does **not** exist in the application  
- [ ] Yes — feature exists and uses cryptographically strong, non-predictable tokens  
- [ ] Yes — feature exists but uses predictable or low-entropy tokens *(Medium)*  
- [ ] Yes — feature exists and stores plaintext credentials or base64-encoded passwords *(High)*  

### Are the security flags correctly applied to the persistent cookie?
- [ ] Yes — `HttpOnly`, `Secure`, and `SameSite` flags are **applied**  
- [ ] Yes — some flags are applied but `HttpOnly` is **missing**  
- [ ] Yes — some flags are applied but `Secure` is **missing** over HTTPS  
- [ ] No — no security flags are **applied** to the persistent cookie  

### Does the "Remember Me" token remain valid after a manual logout?
- [ ] No — token is invalidated on the server-side immediately upon logout  
- [ ] Yes — token remains valid but requires a password for sensitive actions  
- [ ] Yes — token remains valid and allows full account access **without** re-authentication *(Medium)*  

### Is the persistent session terminated upon a password change?
- [ ] Yes — all persistent tokens are revoked when the user changes their password  
- [ ] No — the "Remember Me" token remains valid after a password reset/change **is performed** *(High)*  

### Does the persistent token bypass Multi-Factor Authentication (MFA) on subsequent visits?
- [ ] No — MFA is required even when a "Remember Me" token is present  
- [ ] Yes — MFA is bypassed, but the token is tied to a specific device fingerprint  
- [ ] Yes — MFA is **fully bypassed** using only the persistent cookie from any device *(High)*

---

## WSTG-ATHN-06 — Testing for Browser Cache Weakness

Browser cache weakness occurs when sensitive information is stored in the local browser cache and can be retrieved by an unauthorized user with access to the same physical machine. This lack of proper cache control headers allows potentially sensitive data such as personal information, account details, or session identifiers to persist after the user has logged out or closed the session. Attackers or subsequent users on shared or public terminals can exploit this by navigating through browser history, using the "Back" button, or inspecting local cache files to exfiltrate cached responses. Ensuring the implementation of strict directives like `Cache-Control: no-store` and `Pragma: no-cache` is critical for any application handling authenticated content to ensure sensitive data is never written to the disk.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium* |

> *Severity becomes Medium if the application is frequently accessed from shared/public kiosks or contains highly sensitive PII/PHI data.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Tools:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Are appropriate cache-control headers present on authenticated or sensitive pages?
- [ ] Yes — `Cache-Control: no-store, no-cache` and `Pragma: no-cache` are **present** and **correctly configured**  
- [ ] Yes — some headers are present but `no-store` is **missing**, allowing disk caching  
- [ ] No — no cache-related headers are **applied**, relying on default browser behavior  

### Does the application use the `private` directive for user-specific data?
- [ ] Yes — `Cache-Control: private` is **enabled** for all personalized content to prevent proxy caching  
- [ ] No — content is marked as `public` or lacks the `private` directive, making it **cachable** by intermediate proxies  

### Is sensitive information accessible via the browser "Back" button after a successful logout?
- [ ] No — the browser prompts for re-authentication or the page is **not loaded** from the local cache  
- [ ] Yes — sensitive information is **still visible** via the back button after the session has been terminated  

### Are sensitive non-HTML files (e.g., PDFs, CSV exports, JSON API responses) protected from caching?
- [ ] No — no sensitive non-HTML files are generated or handled by the application  
- [ ] Yes — strict cache-control headers are **applied** to all sensitive file downloads and API endpoints  
- [ ] No — sensitive downloads or API responses are **stored** in the local browser cache or temporary directories  

### Does the application use `Expires: 0` or a date in the past to prevent stale data usage?
- [ ] Yes — `Expires` header is set to `0` or a historical date, **ensuring** the browser treats content as immediately stale  
- [ ] No — `Expires` header is **missing** or set to a future date for sensitive pages

---

## WSTG-ATHN-07 — Testing for Weak Password Policy

Testing for weak password policies involves evaluating the complexity, length, and entropy requirements enforced by an application during account creation and credential updates. Insufficient policies directly compromise the confidentiality of user accounts by making them susceptible to automated dictionary attacks, brute-force attempts, and credential stuffing. These vulnerabilities typically reside in registration endpoints, password reset workflows, and administrative user management panels. From an attacker's perspective, a permissive policy significantly reduces the computational cost and time required to successfully compromise credentials using common wordlists or pattern-based cracking.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Test Status** | Not Performed |
| **Severity** | Medium / Low* |

> *Severity becomes High if the application lacks account lockout or rate-limiting mechanisms to prevent automated guessing.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Tools:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Is a minimum password length enforced by the application?
- [ ] Yes — minimum length is 12+ characters *(Most secure)*  
- [ ] Yes — minimum length is between 8 and 11 characters  
- [ ] Yes — minimum length is **less than** 8 characters  
- [ ] No — no minimum length is enforced  

### Are complexity requirements (uppercase, lowercase, digits, symbols) enforced?
- [ ] Yes — multiple character classes are mandatory and bypass **not possible**  
- [ ] Yes — character classes are suggested but **not** enforced  
- [ ] No — no complexity requirements are applied  

### Does the application block common/weak passwords or passwords containing user-specific data?
- [ ] Yes — known weak passwords (e.g., "password123") and username-based passwords **cannot** be used  
- [ ] Yes — common passwords are blocked, but user-specific data (e.g., username, email) **can** be used  
- [ ] No — any string meeting length/complexity requirements is accepted  

### Can complexity or length requirements be bypassed via client-side modification?
- [ ] No — policies are strictly enforced on the **server-side**  
- [ ] Yes — policies are enforced via JavaScript and **can** be bypassed by intercepting the request  

### Is the password policy clearly communicated to the user without leaking implementation details?
- [ ] Yes — policy is visible during input and provides generic failure messages  
- [ ] Yes — policy is visible but failure messages reveal specific regex or logic gaps  
- [ ] No — policy is **not** visible, forcing trial-and-error discovery

---

## WSTG-ATHN-08 — Testing for Weak Security Question Answer

Testing for weak security question answers involves evaluating the knowledge-based authentication (KBA) mechanisms used to verify a user's identity, typically during password recovery or multi-factor authentication flows. This matters because security questions often rely on static, non-secret information that can be easily discovered through open-source intelligence (OSINT) or social engineering, leading to unauthorized account takeover. These vulnerabilities typically occur in the "Forgot Password" or "Account Unlock" modules where users provide answers to pre-set questions. From an attacker's perspective, this is a prime target for automated brute-forcing or targeted research, as many users provide predictable or publicly verifiable answers to common questions like "What is your mother's maiden name?" or "What high school did you attend?".


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the security question is the sole requirement for a full password reset and account takeover without further verification.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Tools:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### Does the application implement security questions for identity verification or password recovery?
- [ ] No — security questions are **not used** for authentication or recovery  
- [ ] Yes — security questions are **used** as an optional secondary factor  
- [ ] Yes — security questions are **used** as the primary/sole recovery mechanism *(High Risk)*  

### Are security questions selected from a fixed list or are they user-defined?
- [ ] No — questions are entirely user-defined and require high entropy  
- [ ] Yes — questions are fixed but include non-OSINT-discoverable options  
- [ ] Yes — questions are from a fixed list of common, OSINT-discoverable topics *(Vulnerable)*  

### Is rate limiting or account lockout applied to security question answer attempts?
- [ ] Yes — strict rate limiting and account lockout **are applied** to prevent brute-forcing  
- [ ] Yes — rate limiting **is applied** but bypass **is possible** via IP rotation or header manipulation  
- [ ] No — **no rate limiting** is enforced on answer attempts  

### Does the application enforce complexity or validation on the answer content?
- [ ] Yes — validation prevents common words and ensures answer complexity/uniqueness  
- [ ] Yes — basic validation is present but **can** be bypassed with common wordlists or dictionary attacks  
- [ ] No — **no validation** is performed; simple or single-character answers **are permitted**  

### Can the security question challenge be bypassed through logical flaws?
- [ ] No — the recovery flow strictly requires a valid answer to proceed  
- [ ] Yes — the recovery flow **can** be bypassed by manipulating HTTP response codes or session parameters  
- [ ] Yes — the answer is reflected in the source code or hidden fields of the page

---

## WSTG-ATHN-09 — Testing for Weak Password Change or Reset Functionalities

Password change and reset mechanisms are critical components of authentication management that, if improperly implemented, allow attackers to take over user accounts. These vulnerabilities typically involve predictable reset tokens, lack of account lockouts during brute-force attempts, insecure delivery of tokens, or the ability to change passwords without verifying the current password. Attackers exploit these flaws by intercepting or guessing recovery links, performing host header injection to redirect reset emails, or utilizing session fixation to maintain access after a password change. Ensuring these processes require strong verification and utilize cryptographic randomness is essential for maintaining account integrity and preventing unauthorized access.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Is the current password required to set a new password during a standard change?
- [ ] Yes — current password **is required** and verified  
- [ ] Yes — current password **is required** but can be bypassed via parameter pollution or manipulation  
- [ ] No — current password **is not** requested, allowing account takeover via session hijacking *(High)*  

### Are password reset tokens cryptographically secure and unpredictable?
- [ ] Yes — tokens are high-entropy, random, and **not predictable**  
- [ ] Yes — tokens are generated but show low entropy or predictable patterns (e.g., timestamp-based)  
- [ ] No — tokens are **predictable** or based on static user data like Base64 encoded email/ID *(Critical)*  

### Can the password reset link be redirected via Host Header Injection?
- [ ] No — application ignores or validates the `Host` header for link generation  
- [ ] Yes — application uses the `Host` or `X-Forwarded-Host` header to construct links, but validation **is present**  
- [ ] Yes — links **can** be redirected to an attacker-controlled domain via header manipulation *(High)*  

### Does the reset token have a limited lifecycle and immediate invalidation?
- [ ] Yes — tokens expire quickly and are invalidated **immediately** after use or password change  
- [ ] Yes — tokens expire after a long duration (e.g., 24+ hours) or allow **multiple uses**  
- [ ] No — tokens **never expire** or remain valid after the password has been successfully changed  

### Are there rate limits or lockouts on the password reset and change endpoints?
- [ ] Yes — strict rate limiting and CAPTCHAs **are applied** to prevent enumeration and brute-force  
- [ ] Yes — limited controls exist but bypass **is possible** via IP rotation or header spoofing  
- [ ] No — no rate limiting **is applied**, allowing for bulk account enumeration or token brute-forcing

---

## WSTG-ATHN-10 — Testing for Weaker Authentication in Alternative Channel

Testing for weaker authentication in alternative channels involves identifying and analyzing secondary paths to account access—such as mobile APIs, password recovery workflows, help desk interfaces, or legacy subdomains—that may not enforce the same security rigor as the primary web portal. These alternative channels often lack robust multi-factor authentication (MFA), strict rate limiting, or complex password requirements, creating a "weakest link" that undermines the entire authentication system. Attackers specifically target these overlooked entry points to perform credential stuffing or bypass MFA by exploiting discrepancies in how different interfaces handle identity verification. Ensuring parity across all authentication surfaces is essential to prevent unauthorized access and maintain the integrity of the user's session and data.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Are alternative authentication channels present and identified?
- [ ] No — only a single authentication channel exists  
- [ ] Yes — multiple channels identified (e.g., mobile app API, desktop client, legacy portal, SOAP services)  

### Do alternative channels enforce security parity with the primary channel?
- [ ] Yes — all channels enforce **identical** password complexity and MFA requirements  
- [ ] Yes — channels enforce password complexity but MFA is **not required** or **can** be bypassed  
- [ ] No — alternative channels have **significantly weaker** authentication requirements  

### Is rate limiting and account lockout consistent across all channels?
- [ ] Yes — rate limiting and lockout are **consistently enforced** across all endpoints  
- [ ] Yes — controls are present but bypass **is possible** on specific channels (e.g., mobile API)  
- [ ] No — rate limiting or lockout is **not applied** to secondary authentication paths  

### Can MFA be bypassed by switching to an alternative channel?
- [ ] No — MFA is mandatory and **cannot** be bypassed regardless of the entry point  
- [ ] Yes — MFA is required on the web portal but **not enforced** on the mobile or legacy API  

### Are password recovery or "forgot password" flows weaker than the standard login?
- [ ] No — recovery flows use strong, out-of-band verification and do not leak information  
- [ ] Yes — recovery flows use weak security questions that **can** be easily guessed or researched  
- [ ] Yes — recovery flows are vulnerable to enumeration or **do not** require the same level of proof as a standard login

---

## WSTG-ATHN-11 — Testing Multi-Factor Authentication (MFA)

Multi-factor authentication (MFA) testing evaluates the robustness of the secondary security layer designed to prevent unauthorized access even when primary credentials are compromised. Attackers frequently attempt to bypass MFA by identifying endpoints where it is inconsistently enforced, such as legacy API versions, mobile backends, or password reset workflows. Exploitation often involves manipulating server responses, brute-forcing short-lived codes (OTP), or exploiting race conditions and session management flaws that allow a user to transition to an authenticated state without providing the second factor.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### Is MFA enforced consistently across all authentication portals?
- [ ] Yes — MFA is required for all web, mobile, and API-based login attempts  
- [ ] Yes — but MFA is **not enforced** on specific endpoints (e.g., legacy `/api/v1/login` or mobile-specific portals)  
- [ ] No — MFA is **not implemented** in the application *(Critical)*  

### Can the MFA verification step be bypassed via direct URL navigation or response manipulation?
- [ ] No — direct navigation or parameter tampering **cannot** bypass the challenge  
- [ ] Yes — navigating directly to internal dashboard URLs **is possible** without completing the MFA challenge  
- [ ] Yes — manipulating the server response (e.g., changing a `401 Unauthorized` to `200 OK`) **is possible** to gain access  

### Is the MFA code verification process protected against brute-force attacks?
- [ ] Yes — strict rate limiting or account lockout **is applied** after multiple failed OTP attempts  
- [ ] Yes — rate limiting exists but **is possible** to bypass via IP rotation or header manipulation  
- [ ] No — no rate limiting is enforced, and automated brute-forcing of codes **is possible**  

### Does the application maintain a secure session state during the MFA transition?
- [ ] Yes — high-privileged session tokens are issued **only after** successful MFA completion  
- [ ] No — a fully functional session cookie is issued **before** MFA is completed, allowing access to some features  
- [ ] No — the application uses a static identifier or predictable session transition that **can** be hijacked  

### Are alternative MFA factors (SMS, Email, Backup Codes) vulnerable to exploitation?
- [ ] No — all factors use secure, non-predictable, and time-limited values  
- [ ] Yes — backup codes are predictable or **can** be enumerated  
- [ ] Yes — the "Resend Code" functionality can be abused to perform SMS/Email flooding or reveal partial contact details

---

## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Directory traversal and file inclusion vulnerabilities arise when an application uses user-controllable input to construct paths to files or directories without sufficient validation or sanitization. Attackers exploit these flaws by injecting sequences such as `../` to navigate outside the intended directory, potentially accessing sensitive system files, configuration data, or application source code. In more severe cases involving Local File Inclusion (LFI) or Remote File Inclusion (RFI), an attacker may achieve remote code execution by including malicious scripts or leveraging log poisoning and PHP wrappers. These vulnerabilities typically reside in parameters used for dynamic content loading, template engines, or image retrieval endpoints where the server-side logic handles file system paths.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Tools:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Do parameters accept file names or paths for server-side processing?
- [ ] No — no parameters appear to interact with the file system  
- [ ] Yes — parameters exist but use a strict allowlist of file identifiers  
- [ ] Yes — parameters accept direct file names or paths  

### Is the input sanitized to prevent directory traversal sequences?
- [ ] Yes — input is validated against a strict allowlist and traversal sequences are **not possible**  
- [ ] Yes — input is sanitized by removing `../` sequences, but recursive bypass **is possible**  
- [ ] No — no sanitization or validation **is applied** to path-related input  

### Is it possible to access files outside the restricted directory via traversal sequences?
- [ ] No — the application or OS prevents access to files outside the defined scope  
- [ ] Yes — access to files within the web root **is possible**  
- [ ] Yes — access to sensitive system files (e.g., `/etc/passwd`, `C:\Windows\win.ini`) **is possible** *(Critical)*  

### Does the server allow the inclusion of remote URLs (RFI)?
- [ ] No — remote file inclusion is **disabled** at the server/application level  
- [ ] Yes — remote files **can** be included but execution **is not possible**  
- [ ] Yes — remote files **can** be included and executed on the server *(Critical)*  

### Can filters be bypassed using encoding or special characters?
- [ ] No — filters effectively handle various encodings and null bytes  
- [ ] Yes — bypass **is possible** using URL encoding, double URL encoding, or 16-bit Unicode  
- [ ] Yes — bypass **is possible** using null byte injection (`%00`) or filesystem wrappers (e.g., `php://filter`)

---

## WSTG-ATHZ-02 — Testing for Bypassing Authorization Schema

Bypassing authorization schemas occurs when an application fails to enforce access controls, allowing an attacker to access resources or functions outside their intended permissions. This vulnerability typically manifests as Horizontal Privilege Escalation, where an attacker accesses data belonging to another user of the same rank, or Vertical Privilege Escalation, where a low-privileged user gains administrative capabilities. Attackers exploit these flaws by manipulating request parameters such as resource IDs, modifying session tokens, or leveraging HTTP headers like `X-Original-URL` to circumvent path-based restrictions. Ensuring robust authorization is critical for maintaining data confidentiality and preventing unauthorized administrative actions that could compromise the entire system.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### Is horizontal privilege escalation prevented between users of the same role?
- [ ] Yes — authorization checks **are applied** to every resource request and bypass **not possible**  
- [ ] Yes — authorization checks **are applied** but can be bypassed via IDOR or parameter tampering  
- [ ] No — users **can** access data belonging to other users by simply changing a resource ID *(High)*  

### Is vertical privilege escalation prevented for low-privileged users?
- [ ] No — application has only one privilege level  
- [ ] Yes — administrative functions **cannot** be accessed by low-privileged users  
- [ ] Yes — administrative functions are hidden in the UI but **can** be accessed via direct URL  
- [ ] No — low-privileged users **can** perform administrative actions by manipulating roles or permissions *(Critical)*  

### Can authorization be bypassed using HTTP verb tampering?
- [ ] No — access controls are enforced regardless of the HTTP method used  
- [ ] Yes — changing the method (e.g., from `GET` to `POST` or `PUT`) **bypasses** authorization checks  

### Are administrative or restricted endpoints accessible without any authentication?
- [ ] No — all sensitive endpoints require a valid session and proper permissions  
- [ ] Yes — some administrative endpoints are accessible if the attacker knows the direct URL  
- [ ] Yes — unauthenticated access to sensitive functions **is possible** *(Critical)*  

### Do HTTP headers allow for the circumvention of path-based authorization?
- [ ] No — the application is **not** susceptible to header-based bypasses  
- [ ] Yes — headers such as `X-Original-URL`, `X-Rewrite-URL`, or `X-Forwarded-For` **can** be used to bypass access controls

---

## WSTG-ATHZ-03 — Testing for Privilege Escalation

Privilege escalation occurs when an attacker exploits vulnerabilities to gain access to resources or functionality reserved for users with higher authorization levels or different identities. In vertical escalation, a standard user attempts to access administrative functions, while horizontal escalation involves accessing data belonging to another user with the same privilege level. These flaws typically manifest in poorly implemented access control lists (ACLs), insecure direct object references (IDOR), or parameter tampering within session or identity tokens. From an attacker's perspective, this is often achieved by manipulating numeric IDs, modifying role-based parameters in cookies or JWTs, or directly calling hidden API endpoints that lack server-side authorization checks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### Does the application implement multiple privilege levels or multi-tenancy?
- [ ] No — application is a single-user or single-role system  
- [ ] Yes — multiple roles or tenants **are** defined and require authorization testing  

### Is horizontal privilege escalation (same-level access) possible?
- [ ] No — authorization checks **are applied** to all resource identifiers and session owners  
- [ ] Yes — access to other users' data **is possible** through IDOR or parameter tampering  
- [ ] Yes — data leakage **is possible** but modification of other users' data **is not possible**  

### Is vertical privilege escalation (low-to-high) possible?
- [ ] No — administrative endpoints strictly enforce role-based access controls (RBAC)  
- [ ] Yes — administrative functions **can** be accessed by low-privileged users via direct URL access  
- [ ] Yes — administrative functions **can** be accessed by manipulating role-related headers, cookies, or JWT claims  

### Can user roles or permissions be modified via Mass Assignment or Parameter Pollution?
- [ ] No — role-based fields are strictly read-only and **cannot** be modified by users  
- [ ] Yes — user permissions **can** be elevated by including hidden parameters (e.g., `role=admin`) in profile updates or registration  

### Are authorization checks consistently enforced across all API versions and HTTP methods?
- [ ] Yes — controls **are applied** consistently across all versions and methods  
- [ ] No — legacy API versions (e.g., `/v1/`) or specific HTTP methods (e.g., `PUT`, `DELETE`, `PATCH`) **bypass** authorization  
- [ ] No — administrative functions are hidden in the UI but **enabled** at the API level for all users

---

## WSTG-ATHZ-04 — Testing for Insecure Direct Object References

Insecure Direct Object References (IDOR) occur when an application provides direct access to objects based on user-supplied input without performing an authorization check to verify the requester's permissions. This vulnerability significantly impacts confidentiality and integrity, as it allows attackers to view, modify, or delete data belonging to other users or the system. It typically manifests in URL parameters, POST body data, or JSON keys that refer to internal database keys, filenames, or account identifiers. From an attacker's perspective, exploitation involves manipulating these identifiers—often through incrementing integers or fuzzing UUIDs—to access resources outside their authorized scope.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### Does the application use direct object identifiers in request parameters?
- [ ] No — application uses indirect references or encrypted maps *(Most secure)*  
- [ ] Yes — application uses non-predictable identifiers (e.g., long UUIDs/hashes)  
- [ ] Yes — application uses predictable identifiers (e.g., incremental integers or usernames)  

### Is server-side authorization performed for every request involving an object identifier?
- [ ] Yes — server-side checks **cannot** be bypassed for any identifier  
- [ ] Yes — server-side checks are present but bypass **is possible** via parameter pollution or header manipulation  
- [ ] No — no authorization check **is applied** to verify ownership of the requested object  

### Can an attacker access or modify objects belonging to other users (Horizontal IDOR)?
- [ ] No — access to other users' data **is not possible**  
- [ ] Yes — viewing of other users' data **is possible** but modification **is not possible**  
- [ ] Yes — both viewing and modification of other users' data **is possible** *(Critical)*  

### Is it possible to access administrative or system-level objects (Vertical IDOR)?
- [ ] No — access to higher-privileged objects **is not possible**  
- [ ] Yes — access to administrative configurations or system files **is possible**  

### Does the application leak object identifiers via search, metadata, or other endpoints?
- [ ] No — identifiers are **not exposed** unintentionally  
- [ ] Yes — identifiers for other users/objects **can** be enumerated through secondary endpoints or public profiles

---

## WSTG-ATHZ-05 — Testing for OAuth Weaknesses

OAuth weaknesses encompass a variety of flaws in the implementation of the OAuth 2.0 or OpenID Connect (OIDC) protocols, often resulting in complete account takeover or unauthorized data access. These vulnerabilities typically manifest during the authorization flow, specifically concerning the validation of the `redirect_uri`, the entropy and verification of the `state` parameter, and the secure handling of access or ID tokens. Attackers exploit these by intercepting authorization codes, redirecting tokens to attacker-controlled domains via open redirects, or performing Cross-Site Request Forgery (CSRF) to link a victim's session to an attacker's account. Because OAuth often serves as a primary authentication mechanism, a single misconfiguration can compromise the integrity of the entire user identity system.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Tools:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Is the `state` parameter implemented and validated to prevent CSRF?
- [ ] Yes — `state` is mandatory, unique per session, and strictly validated on the server  
- [ ] Yes — `state` is present but is static or predictable across different sessions  
- [ ] Yes — `state` is present but the application **does not** validate it upon callback  
- [ ] No — `state` parameter is **not** used in the authorization request *(Critical)*  

### Is the `redirect_uri` strictly validated against a whitelist?
- [ ] Yes — only exact matches against a pre-registered whitelist are **accepted**  
- [ ] Yes — validation is applied but bypass **is possible** via path traversal or subdomain manipulation  
- [ ] Yes — validation is applied but bypass **is possible** via parameter pollution or fragment injection  
- [ ] No — application **allows** arbitrary URLs in the `redirect_uri` parameter *(Critical)*  

### Can the `response_type` be manipulated to alter the flow?
- [ ] No — application only accepts the intended flow (e.g., `code`) and rejects others  
- [ ] Yes — switching from `code` to `token` (Implicit flow) **is possible** and leaks tokens in the URL  
- [ ] Yes — hybrid flows or unauthorized `response_type` values are **enabled** and processed  

### Are access tokens or authorization codes leaked via Referer headers or browser history?
- [ ] No — tokens/codes are handled securely and sensitive pages use appropriate `Referrer-Policy`  
- [ ] Yes — tokens/codes **are** leaked to third-party domains via the `Referer` header  
- [ ] Yes — tokens/codes **are** visible in the browser history due to insecure caching or GET requests  

### Does the application enforce the "Least Privilege" principle for OAuth scopes?
- [ ] Yes — application requests only the minimum scopes necessary for the functionality  
- [ ] No — application requests excessive or administrative scopes by default  
- [ ] No — user **cannot** review or opt-out of specific scopes during the consent phase

---

## WSTG-SESS-01 — Testing for Session Management Schema

Session management schema testing focuses on analyzing how the application handles the lifecycle and structure of session identifiers to ensure they are robust against prediction and hijacking. Attackers capture multiple session tokens to perform statistical analysis, looking for patterns, low entropy, or predictable increments that allow them to forge valid sessions for other users. Weaknesses in the schema often manifest as session fixation vulnerabilities or the use of insufficiently random values, typically found in HTTP cookies, URL parameters, or custom header implementations. Compromising the session schema is a high-impact attack that grants an adversary complete access to a victim's authenticated state without needing their primary credentials.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### Is the session identifier sufficiently complex and random?
- [ ] Yes — token passes statistical randomness tests (high entropy) and bypass **is not possible**  
- [ ] Yes — token is long but contains predictable patterns or static segments  
- [ ] No — token is sequential, based on predictable timestamps, or has **critically low** entropy *(Critical)*  

### Where is the session identifier transmitted and stored?
- [ ] Identifier is stored in a `Secure` and `HttpOnly` cookie *(Most secure)*  
- [ ] Identifier is stored in a cookie but lacks `Secure` or `HttpOnly` flags  
- [ ] Identifier is transmitted via URL parameters or `GET` requests **only** *(High Risk)*  

### Does the application issue a new session identifier upon successful authentication?
- [ ] Yes — a new, unique session ID **is generated** immediately after login  
- [ ] No — the session ID remains the same before and after login *(Session Fixation possible)*  

### Is the session identifier tied to a specific IP address or User-Agent to prevent reuse?
- [ ] Yes — session is invalidated if the client's fingerprint changes significantly  
- [ ] No — session **can** be reused across different IP addresses or devices without additional verification  

### Are session identifiers properly invalidated on the server-side after logout or timeout?
- [ ] Yes — server-side session state is **completely destroyed** upon logout  
- [ ] No — session remains valid on the server even if the client-side cookie is deleted  
- [ ] No — session **never expires** or has an excessively long timeout period

---

## WSTG-SESS-02 — Testing for Cookies Attributes

Session management often relies on HTTP cookies to maintain state, making the security attributes of these cookies a primary target for attackers. By omitting or misconfiguring attributes such as `HttpOnly`, `Secure`, and `SameSite`, applications expose session tokens to theft via Cross-Site Scripting (XSS), interception over unencrypted channels, or Cross-Site Request Forgery (CSRF) attacks. Pentesters examine these flags to determine if an attacker can exfiltrate session identifiers from the browser's document object model or force the browser to send them in cross-origin requests. Proper attribute configuration is a fundamental defense-in-depth measure to ensure that sensitive tokens remain confined to authorized, secure contexts.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analysis of Cookie Flag Implementation
| Attribute | Present | Missing |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Is the `HttpOnly` flag applied to sensitive session cookies?
- [ ] Yes — applied to **all** sensitive session cookies *(Most secure)*  
- [ ] Yes — applied only to some session cookies  
- [ ] No — flag is **not applied**, allowing access via client-side scripts *(Critical)*  

### Is the `Secure` flag enforced for cookies transmitted over HTTPS?
- [ ] Yes — applied to **all** cookies transmitted over TLS  
- [ ] No — flag is **not applied**, allowing cookies to be sent over plaintext HTTP  

### Is the `SameSite` attribute used to mitigate CSRF risks?
- [ ] Yes — set to `Strict` or `Lax` for session cookies  
- [ ] Yes — set to `None` but with the `Secure` flag present  
- [ ] No — attribute is **missing** or set to `None` **without** `Secure`  

### Are the `Domain` and `Path` attributes restricted to the minimum necessary scope?
- [ ] Yes — scoped to the **specific** subdomain and application path  
- [ ] No — scope is too **broad** (e.g., set to a parent domain or root `/`), increasing the attack surface  

### Are sensitive session cookies expired properly via the `Expires` or `Max-Age` attributes?
- [ ] Yes — appropriate expiration dates are set  
- [ ] No — cookies are persistent with excessively **long** lifetimes  
- [ ] No — cookies are session-only but **lack** server-side timeout enforcement

---

## WSTG-SESS-03 — Session Fixation

Session fixation occurs when an application fails to invalidate or rotate the session identifier after a user successfully authenticates, allowing an attacker to force a known session token onto a victim. If the application preserves the pre-authentication session ID after login, an attacker can provide a victim with a specifically crafted link containing a fixed session ID and subsequently hijack the authenticated session. This vulnerability typically manifests in login forms or via session identifiers accepted through URL parameters and cookies. From an attacker's perspective, exploitation involves "fixing" the session via a malicious link or a header injection vulnerability and waiting for the victim to provide credentials, effectively bypassing the need to steal a live token.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### Does the session identifier change after successful authentication?
- [ ] Yes — a new session identifier **is issued** and the old one is invalidated  *(Most secure)*  
- [ ] Yes — a new session identifier **is issued** but the old one **remains valid**  
- [ ] No — the session identifier **remains the same** before and after authentication  *(Critical)*  

### Does the application accept session identifiers provided via URL parameters?
- [ ] No — session IDs are **exclusively** managed via cookies  
- [ ] Yes — session IDs are accepted in the URL but are **ignored** or **rotated** upon login  
- [ ] Yes — session IDs in the URL are **accepted** and **persisted** after authentication  

### Can an attacker-defined session ID be forced onto the application?
- [ ] No — the application **rejects** invalid or non-existent session IDs provided by the user  
- [ ] Yes — the application **accepts** and initializes a session using any arbitrary ID provided in a cookie  
- [ ] Yes — the application **accepts** and initializes a session using any arbitrary ID provided in a URL parameter  

### Are pre-authentication sessions correctly terminated?
- [ ] Yes — the anonymous session is **fully destroyed** upon the login event  
- [ ] No — the anonymous session is **not destroyed**, allowing potential session harvesting  

### Is the session cookie protected against client-side injection?
- [ ] Yes — `HttpOnly` and `Secure` flags are **applied** to prevent script-based fixation  
- [ ] No — flags are **not applied**, allowing session IDs to be set via Cross-Site Scripting (XSS)

---

## WSTG-SESS-04 — Testing for Exposed Session Variables

Exposed session variables occur when sensitive session identifiers or state-related data are transmitted via insecure channels such as URL query strings, Referer headers, or system logs. This exposure significantly increases the risk of session hijacking, as identifiers can be cached in browser history, logged by intermediate proxies, or leaked to third-party sites via the Referer header. Pentesters identify these exposures by monitoring all outbound requests and inspecting application logs or server configurations for inadvertent data leakage. In the real world, attackers exploit these leaks by harvesting valid session tokens from shared machines or analyzing traffic patterns to bypass authentication and gain unauthorized access to user accounts.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the exposed variable is a session token that allows immediate account takeover.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### Is the session identifier transmitted in the URL query string?
- [ ] No — session identifiers are **not** present in the URL query string  
- [ ] Yes — identifiers are present but session is short-lived or low-risk  
- [ ] Yes — unique session IDs **are** present in the URL query string *(Critical)*  

### Does the application leak session tokens through the Referer header?
- [ ] No — `Referrer-Policy` prevents leakage or no third-party links exist  
- [ ] Yes — session tokens **are** transmitted to third-party domains via the Referer header  

### Are session variables stored in server-side or application logs?
- [ ] No — session variables are masked or excluded from logs  
- [ ] Yes — session variables **are** recorded in application or web server logs in plain text  

### Does the application use GET requests for state-changing operations?
- [ ] No — application uses `POST` or other non-idempotent methods for sensitive operations  
- [ ] Yes — `GET` is used, causing session data to be cached in browser history and intermediate proxies  

### Is the `Cache-Control` header configured to prevent caching of session-related data?
- [ ] Yes — `Cache-Control: no-store` (or similar) **is applied** to sensitive responses  
- [ ] Yes — controls are in place but bypass **is possible** via browser edge cases  
- [ ] No — sensitive responses containing session data **can** be cached by the browser

---

## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) is a vulnerability where an attacker trick a victim's browser into performing an unwanted action on a different website where the victim is currently authenticated. This exploit leverages the browser's behavior of automatically attaching "ambient" credentials, such as session cookies or Authorization headers, to outgoing requests. Attackers typically target state-changing operations like changing passwords, updating email addresses, or executing financial transfers by hosting malicious scripts or hidden forms on a third-party site. Successful exploitation can lead to full account takeover or unauthorized data modification without the user's knowledge or consent.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the vulnerable action allows for account takeover, privilege escalation, or unauthorized financial transactions.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Tools:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Are anti-CSRF tokens implemented for state-changing requests?
- [ ] Yes — unique, cryptographically strong tokens are required for all state-changing actions  
- [ ] Yes — tokens are present but are **not** unique per session or are predictable  
- [ ] No — anti-CSRF tokens are **not** implemented  

### Is the server-side validation of the anti-CSRF token robust?
- [ ] Yes — server strictly validates the token and bypass **not possible**  
- [ ] Yes — validation is performed but **can** be bypassed by removing the token parameter  
- [ ] Yes — validation is performed but **can** be bypassed by providing a blank or dummy token  
- [ ] Yes — validation is performed but token is **not** tied to the user's session  

### Does the application rely on easily bypassable methods for CSRF protection?
- [ ] No — application does not rely on weak headers or origin checks alone  
- [ ] Yes — protection relies solely on the `Referer` or `Origin` header which **can** be spoofed or stripped  
- [ ] Yes — protection relies on checking the `X-Requested-With` header which **can** be bypassed via CORS misconfigurations  

### Are session cookies configured with the `SameSite` attribute?
- [ ] Yes — `SameSite` is set to `Strict` or `Lax` on all session-related cookies  
- [ ] No — `SameSite` attribute is **missing**, defaulting to browser-specific behavior  
- [ ] No — `SameSite` is explicitly set to `None` without the `Secure` flag or additional mitigations  

### Can the CSRF protection be bypassed by changing the HTTP request method?
- [ ] No — protection is enforced regardless of the HTTP method used  
- [ ] Yes — tokens are only validated on `POST` requests, but the action **can** be performed via `GET`  
- [ ] Yes — changing the method (e.g., to `PUT` or `DELETE`) bypasses the token check

---

## WSTG-SESS-06 — Testing for Logout Functionality

Logout functionality is a critical security control designed to terminate a user's session and invalidate associated session identifiers on both the client and server sides. Failure to properly implement logout allows session fixation or hijacking attacks, as an attacker who gains access to a machine after a user has "logged out" could potentially reuse the still-active session token. Pentesters evaluate this by checking if the session cookie is cleared from the browser, if the server-side session state is explicitly destroyed, and if back-button navigation allows access to cached sensitive data. A secure logout implementation ensures that once the action is triggered, no subsequent requests with the old session token are authorized by the application.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Is a functional logout mechanism present and accessible?
- [ ] Yes — logout button exists and triggers a termination request  
- [ ] No — logout button is **missing** or does **not** trigger a termination event  

### Is the session identifier invalidated on the server side?
- [ ] Yes — server rejects all subsequent requests using the old session token  
- [ ] No — server **continues to accept** the old session token after logout  

### Does the application clear session cookies in the browser upon logout?
- [ ] Yes — cookies are overwritten with an expired date or empty value  
- [ ] Yes — cookies remain but are **no longer valid** on the server  
- [ ] No — cookies remain in the browser and **retain** their original values  

### Can sensitive authenticated content be accessed via the browser 'Back' button after logout?
- [ ] No — `Cache-Control: no-store` or similar headers prevent viewing sensitive pages post-logout  
- [ ] Yes — sensitive pages are cached and **can** be viewed by navigation after the session is terminated

---

## WSTG-SESS-07 — Testing Session Timeout

Session timeout testing evaluates whether an application effectively terminates a user's session after a predefined period of inactivity or total duration. This control is vital for mitigating the risk of session hijacking, particularly on shared workstations or in scenarios where an attacker captures a session identifier via network interception or XSS. Pentesters analyze both idle timeouts, which trigger after a period of inactivity, and absolute timeouts, which limit the total lifespan of a session regardless of activity. From an attacker's perspective, indefinite or excessively long sessions provide an extended window of opportunity to maintain unauthorized access and bypass the need for re-authentication.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### Does the application implement an idle session timeout?
- [ ] Yes — session expires after a short period (e.g., 15-30 minutes) of inactivity  
- [ ] Yes — session expires but timeout duration is **excessively long** (e.g., > 24 hours)  
- [ ] No — session **remains active** indefinitely during inactivity  

### Is the session timeout enforced on the server-side?
- [ ] Yes — server rejects requests with expired tokens regardless of client-side state  
- [ ] No — timeout is **only** enforced via client-side JavaScript or meta-refresh  

### Does the application implement an absolute session timeout?
- [ ] Yes — session is terminated after a fixed maximum duration regardless of activity  
- [ ] No — session **can** be extended indefinitely as long as there is continuous activity  

### Is the session identifier invalidated on the server upon timeout?
- [ ] Yes — session token **cannot** be reused once the timeout threshold is reached  
- [ ] No — session token **is still valid** on the server even after the client is redirected to the login page  

### Can the session be kept alive indefinitely using automated heartbeat requests?
- [ ] No — absolute timeout or secondary controls **prevent** infinite session extension  
- [ ] Yes — periodic background requests (e.g., AJAX) **can** maintain the session indefinitely

---

## WSTG-SESS-08 — Session Puzzling

Session Puzzling, also known as Session Variable Overloading, occurs when an application uses the same session variable for multiple purposes across different modules or fails to properly clear session states during workflow transitions. Attackers exploit this behavior by populating session variables in one context—such as an unauthenticated profile preview or a multi-step registration—and then navigating to a protected area where the application incorrectly trusts those pre-existing variables. This can lead to critical impacts including authentication bypass, privilege escalation, or unauthorized access to sensitive data by tricking the application into believing a session is in a higher-privileged state than it actually is. The vulnerability is most prevalent in complex, stateful applications where session management logic is inconsistently applied across different functional components.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### Does the application use the same session variable names for different purposes across separate modules?
- [ ] No — variable names are unique or scopes are isolated  
- [ ] Yes — shared variable names exist but state is cleared between modules  
- [ ] Yes — shared variable names exist and state **is not** cleared  

### Can unauthenticated actions influence session variables used in authenticated contexts?
- [ ] No — session variables are initialized only **after** successful authentication  
- [ ] Yes — session variables can be set before login but do **not** affect post-login state  
- [ ] Yes — session variables set during pre-authentication **are** trusted post-authentication *(Critical)*  

### Does the application re-initialize the session identifier and its associated variables upon a change in privilege level?
- [ ] Yes — session is fully reset and a new ID **is issued**  
- [ ] Partial — new ID is issued but some session variables **persist**  
- [ ] No — session ID and all variables **remain** unchanged  

### Can administrative privileges be gained by manipulating session variables via an unprivileged account?
- [ ] No — privilege variables **cannot** be modified by users  
- [ ] Yes — variables can be modified but the application **validates** them against the backend database  
- [ ] Yes — variables can be modified and the application **trusts** the session state implicitly *(Critical)*  

### Is the session state cleared immediately upon completing a specific workflow (e.g., password reset, checkout)?
- [ ] Yes — session variables for the workflow are destroyed upon completion  
- [ ] No — workflow variables **persist** and can be reused in other application areas

---

## WSTG-SESS-09 — Testing for Session Hijacking

Session hijacking occurs when an attacker captures, predicts, or fixes a valid session identifier (SID) to gain unauthorized access to a user's active session. This vulnerability typically manifests through insufficient transport layer security, lack of cookie security flags, or predictable SID generation algorithms that allow an attacker to bypass authentication entirely. From an attacker's perspective, successful exploitation provides full impersonation of the victim without requiring their credentials, often achieved through network sniffing, Cross-Site Scripting (XSS), or session fixation attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Are session identifiers protected during transit?
- [ ] Yes — `Secure` flag is **enabled** and HSTS is strictly enforced  
- [ ] Yes — `Secure` flag is **enabled** but HSTS is **disabled**  
- [ ] No — `Secure` flag is **not applied**, allowing SID interception over unencrypted channels *(High)*  

### Is the session identifier protected from client-side script access?
- [ ] Yes — `HttpOnly` flag is **applied** to all session-related cookies  
- [ ] No — `HttpOnly` flag is **not applied**, allowing SID exfiltration via XSS *(Critical)*  

### Does the application implement session binding to client attributes?
- [ ] Yes — session is bound to client attributes (IP/User-Agent) and reuse **not possible**  
- [ ] Yes — session binding is present but bypass **is possible** via header spoofing  
- [ ] No — no session binding exists; SID **is valid** when used from any source  

### Is the session identifier sufficiently random to prevent prediction?
- [ ] Yes — SID uses a cryptographically secure pseudo-random number generator (CSPRNG)  
- [ ] No — SID length is insufficient or follows a **predictable** sequence/pattern  

### Are concurrent sessions managed to limit the attack window?
- [ ] No — application strictly enforces a single active session per user  
- [ ] Yes — multiple concurrent sessions are **enabled** but monitored  
- [ ] Yes — unlimited concurrent sessions **are possible** across different devices/locations

---

## WSTG-SESS-10 — Testing JSON Web Tokens

JSON Web Tokens (JWTs) are commonly used for stateless session management and identity propagation, but their security relies entirely on the correct implementation of signing algorithms and strict claim verification. Attackers frequently attempt to bypass authentication by exploiting "none" algorithm flaws, brute-forcing weak HS256 secret keys, or performing algorithm-switching attacks where an asymmetric public key is used as a symmetric HMAC secret. These vulnerabilities typically manifest in session cookies or Authorization headers, potentially allowing an attacker to escalate privileges or impersonate any user by modifying claims such as `role` or `user_id` without invalidating the token's cryptographic integrity.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Tools:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### Is the "none" algorithm accepted by the server?
- [ ] No — server **rejects** tokens using `alg: none` in the header  
- [ ] Yes — server **accepts** tokens with `alg: none` and an empty signature *(Critical)*  

### How is the JWT signature verified and protected against brute-force?
- [ ] Yes — strong asymmetric keys (RS256/ES256) or high-entropy HMAC secrets are used  
- [ ] Yes — HS256 is used but the secret **can** be brute-forced due to low entropy or default values  
- [ ] No — signature verification **is not applied** or **can** be bypassed via algorithm switching (RS256 to HS256)  

### Are standard claims (exp, nbf, iat) strictly enforced by the backend?
- [ ] Yes — `exp` (expiration) is present and **cannot** be bypassed  
- [ ] Yes — `exp` is present but the server **does not** enforce it during validation  
- [ ] No — expiration claims are **missing** or ignored, allowing for indefinite session replay  

### Does the implementation prevent header-based key injection (kid, jku, x5u)?
- [ ] Yes — headers are sanitized and only trusted internal keys/sources are **allowed**  
- [ ] No — the `kid` (Key ID) header is vulnerable to directory traversal or SQL injection to point to a known file  
- [ ] No — the `jku` or `x5u` headers **can** be pointed to attacker-controlled URLs to load malicious keys

---

## WSTG-SESS-11 — Testing for Concurrent Sessions

Concurrent session testing evaluates whether an application permits a single user account to maintain multiple simultaneous active sessions across different browsers, devices, or IP addresses. Failure to restrict concurrent sessions increases the window of opportunity for attackers to utilize hijacked session tokens or compromised credentials without alerting the legitimate user or terminating existing sessions. This behavior is typically assessed by authenticating from multiple sources and monitoring session stability, often revealing weaknesses in session management logic or a lack of security-focused business rules. From an attacker's perspective, the lack of concurrency control allows for stealthy persistence after an initial compromise and facilitates parallel automated attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Are concurrent sessions limited by the application policy?
- [ ] Yes — only one session **is permitted** per user; new logins invalidate old ones *(Most secure)*  
- [ ] Yes — a fixed number of concurrent sessions **is enforced** and cannot be exceeded  
- [ ] No — an unlimited number of concurrent sessions **is possible**  

### How does the application respond when the session limit is reached?
- [ ] The oldest active session **is automatically terminated** (session fixation/hijacking mitigation)  
- [ ] The user **is prompted** to terminate existing sessions before the new session is authorized  
- [ ] The new login attempt **is blocked** until a manual logout occurs from another device  
- [ ] No action is taken — multiple sessions remain **enabled** simultaneously  

### Does the application provide a session management interface for the user?
- [ ] Yes — users **can** view active sessions (IP, device, time) and remotely terminate them  
- [ ] Yes — users **can** view active sessions but **cannot** terminate them remotely  
- [ ] No — users **cannot** see or manage other active sessions associated with their account  

### Is the user notified when concurrent login activity is detected?
- [ ] Yes — an alert (email, SMS, or in-app) **is triggered** for logins from new devices or locations  
- [ ] No — no notification **is provided** when concurrent sessions are established

---

## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Reflected Cross Site Scripting (XSS) occurs when an application includes untrusted data in an HTTP response without sufficient validation or encoding, causing the payload to be executed in the victim's browser context. Attackers deliver malicious payloads via social engineering, typically through crafted URLs or forms, to hijack user sessions, exfiltrate sensitive cookies, or perform unauthorized actions on behalf of the user. This vulnerability is commonly found in search parameters, error messages, and any endpoint that reflects input directly back to the user interface. Exploitation relies on the victim interacting with a malicious link, making it a non-persistent but highly effective attack vector for targeting specific administrative or authenticated users.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Are user-supplied inputs reflected in the response body?
- [ ] No — input is **never** reflected back to the user  
- [ ] Yes — input is reflected but properly encoded and bypass **not possible**  
- [ ] Yes — input is reflected **without** any encoding or sanitization  

### Is context-aware output encoding implemented?
- [ ] Yes — proper encoding (HTML, Attribute, JavaScript) is **applied** based on the specific reflection context  
- [ ] Yes — encoding is **applied** but is insufficient for the specific context (e.g., HTML encoding inside a `<script>` tag)  
- [ ] No — encoding is **not applied**  

### Can input validation or Web Application Firewall (WAF) filters be bypassed?
- [ ] No — filters effectively block all common and obfuscated XSS payloads  
- [ ] Yes — filters are present but bypass **is possible** using character encoding, non-standard tags, or polyglots  
- [ ] No — no filters or validation mechanisms are in place  

### What is the execution context of the reflected input?
- [ ] Safe — input is reflected in a non-executable location (e.g., within standard `<div>` or `<span>` tags)  
- [ ] Risky — input is reflected inside HTML attributes or within the `src`/`href` of tags  
- [ ] Critical — input is reflected directly inside `<script>` blocks, event handlers, or template literals  

### Are modern browser security headers present to mitigate XSS impact?
- [ ] Yes — `Content-Security-Policy` (CSP) is **enabled** with a restrictive script-src  
- [ ] Yes — `Content-Security-Policy` (CSP) is **enabled** but contains `unsafe-inline` or weak whitelists  
- [ ] No — no CSP or legacy `X-XSS-Protection` headers are present

---

## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Stored Cross Site Scripting (XSS), also known as Persistent XSS, occurs when an application receives data from a user and stores it in a persistent database, file system, or other storage medium without adequate validation or encoding. When a victim subsequently navigates to a page that retrieves and displays this unsanitized data, the malicious script executes within their browser context under the application's origin. This vulnerability is particularly dangerous because it does not require a victim to click a specially crafted link; any user viewing the affected page becomes a target, potentially leading to session hijacking, unauthorized actions, or credential harvesting. Attackers typically target comment sections, user profiles, message boards, and administrative logs where input is consistently rendered for other users or administrators.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Tools:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Are there entry points that store user input for later display to other users?
- [ ] No — application does not store user-controllable input for later rendering  
- [ ] Yes — input is stored (e.g., profiles, comments) but is **not** rendered in an HTML context  
- [ ] Yes — input is stored and rendered to other users or administrators  

### Is output encoding or sanitization applied when stored data is rendered?
- [ ] Yes — context-aware output encoding **is applied** and bypass **not possible**  *(Most secure)*  
- [ ] Yes — sanitization (e.g., DOMPurify) **is applied** but bypass **is possible** via configuration errors  
- [ ] No — data is rendered directly into the DOM **without** encoding or sanitization *(High)*  

### Can the input validation or sanitization be bypassed using alternative payloads?
- [ ] No — filters handle various encodings, non-standard tags, and event handlers securely  
- [ ] Yes — bypass **is possible** using HTML entity encoding or specific tags (e.g., `<svg>`, `<img>`)  
- [ ] Yes — bypass **is possible** via polyglot payloads or mutation-based XSS (mXSS)  

### Does a Content Security Policy (CSP) provide a secondary layer of defense?
- [ ] Yes — a strict CSP is **enabled** and prevents execution of inline scripts and unauthorized sources  
- [ ] Yes — a CSP is **enabled** but is misconfigured (e.g., `unsafe-inline` or broad `script-src`)  
- [ ] No — no CSP is **enabled** for the application  

### Is it possible to achieve "Stored XSS to RCE" or other high-impact chains (Blind XSS)?
- [ ] No — impact is limited to the client-side browser context  
- [ ] Yes — stored XSS **can** be used to target administrators (Blind XSS) in internal panels  
- [ ] Yes — stored XSS **can** be chained with other vulnerabilities (e.g., CSRF, sensitive data exfiltration)

---

## WSTG-INPV-03 — Testing for HTTP Verb Tampering

HTTP Verb Tampering exploits weaknesses in how web servers and application frameworks authorize access to specific resources based on the HTTP method used. Attackers attempt to bypass security constraints by substituting standard methods like `GET` or `POST` with alternatives such as `HEAD`, `PUT`, `OPTIONS`, or even arbitrary, non-standard strings that the backend might process inconsistently. This vulnerability typically occurs when security configurations—such as Java EE web.xml filters or .NET authorization rules—explicitly list allowed methods but fail to account for others, or when a "deny-by-method" approach is used instead of "deny-all." Successful exploitation can result in unauthorized access to administrative functions, data modification, or information disclosure regarding the server's internal configuration.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if administrative functions or data modification (PUT/DELETE) can be performed without authentication.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### Does the application respond to non-standard or arbitrary HTTP methods?
- [ ] No — application rejects all methods except those explicitly required  
- [ ] Yes — application responds to standard methods (OPTIONS, TRACE) but **cannot** bypass security  
- [ ] Yes — application accepts arbitrary strings (e.g., FOO, TEST) and processes them as `GET` or `POST` requests  

### Can authentication/authorization be bypassed by changing the HTTP method?
- [ ] No — security controls are applied globally regardless of the HTTP verb  
- [ ] Yes — controls are in place but bypass **is possible** using the `HEAD` method  
- [ ] Yes — security controls are **not applied** to alternative methods, allowing unauthorized access to restricted endpoints  

### Are dangerous methods such as `PUT` or `DELETE` enabled on the server?
- [ ] No — dangerous methods are **disabled** or return a 405 Method Not Allowed  
- [ ] Yes — methods are enabled but require valid, high-privilege authentication  
- [ ] Yes — methods are **enabled** and allow unauthorized file upload or resource deletion *(Critical)*  

### Does the `OPTIONS` method reveal sensitive information about allowed verbs?
- [ ] No — `OPTIONS` is **disabled** or returns a generic response  
- [ ] Yes — `OPTIONS` returns the `Allow` header but does not expose restricted internal methods  
- [ ] Yes — `OPTIONS` reveals internal-only or administrative methods that aid in further exploitation  

### Is the `TRACE` method enabled, potentially allowing Cross-Site Tracing (XST)?
- [ ] No — `TRACE` and `TRACK` methods are **disabled**  
- [ ] Yes — `TRACE` is **enabled** and reflects back HTTP headers, including sensitive cookies/tokens

---

## WSTG-INPV-04 — Testing for HTTP Parameter Pollution

HTTP Parameter Pollution (HPP) occurs when an application receives multiple HTTP parameters with the same name and processes them in an inconsistent or insecure manner. By supplying duplicate parameters, an attacker can manipulate the application's internal logic, potentially bypassing Web Application Firewalls (WAF), input validation filters, or access control mechanisms. This vulnerability is highly dependent on the underlying web server or application framework, which may choose the first, last, or a concatenated version of the repeated parameters. Exploitation typically targets endpoints that pass parameters to back-end APIs, database queries, or URL redirection mechanisms, leading to impacts ranging from cross-site scripting (XSS) to unauthorized data exfiltration.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### How does the application handle multiple HTTP parameters with the same name?
- [ ] No — duplicate parameters are **rejected** or ignored by the server  
- [ ] Yes — the server consistently selects only the **first** or **last** occurrence  
- [ ] Yes — the server **concatenates** values, potentially allowing for injection  

### Can HPP be leveraged to bypass security controls like a WAF or input filter?
- [ ] No — security controls correctly inspect **all** occurrences of a parameter  
- [ ] Yes — a WAF or filter only inspects the **first** occurrence, allowing a malicious **second** occurrence to pass  
- [ ] Yes — concatenation allows an attacker to **split** a payload across multiple parameters to evade detection  

### Does the application reflect polluted parameters in the response without proper sanitization?
- [ ] No — parameters are sanitized or encoded before being reflected in the UI  
- [ ] Yes — pollution results in reflected content, but sanitization **is applied**  
- [ ] Yes — pollution results in reflected content and sanitization **is not applied**, leading to XSS or logic errors  

### Are parameters passed to internal API calls or back-end systems vulnerable to pollution?
- [ ] No — back-end requests use safe, non-dynamic construction methods  
- [ ] Yes — HPP allows an attacker to **inject** or **overwrite** parameters in the back-end request structure  

### Does the application exhibit different behavior when parameters are polluted in GET vs POST requests?
- [ ] No — behavior is consistent across all HTTP methods  
- [ ] Yes — only GET parameters are vulnerable to pollution  
- [ ] Yes — only POST (body) parameters are vulnerable to pollution

---

## WSTG-INPV-05 — SQL Injection

SQL Injection occurs when user-supplied input is incorporated into SQL queries without proper parameterization or sanitization, allowing attackers to manipulate query logic. Successful exploitation can lead to unauthorized data exfiltration, authentication bypass, data modification, and in some cases remote code execution via database features such as `xp_cmdshell` or `LOAD_FILE()`. This vulnerability commonly affects login forms, search functionality, REST API parameters, HTTP headers, and any endpoint that constructs dynamic SQL queries from user-controlled input. From an attacker's perspective, identifying an injection point is the primary gateway to full database compromise and potential lateral movement within the infrastructure.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Test Status** | Not Performed |
| **Severity** | Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Tools:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Are user-controllable parameters processed using secure database access methods?
- [ ] Yes — application uses ORM or parameterized queries exclusively and bypass **not possible**  
- [ ] Yes — parameterized queries are used but bypass **is possible** via edge cases (e.g., `ORDER BY` clauses)  
- [ ] No — raw string concatenation is used to build SQL queries **without** parameterization  

### Can the authentication mechanism be circumvented via SQL injection?
- [ ] No — login parameters are **not** vulnerable to injection  
- [ ] Yes — authentication bypass **is possible** using tautology-based payloads (e.g., `' OR 1=1 --`)  

### Is data exfiltration possible via in-band or out-of-band techniques?
- [ ] No — no data exfiltration identified  
- [ ] Yes — UNION-based or error-based exfiltration **is possible**  
- [ ] Yes — blind (Boolean or Time-based) exfiltration **is possible**  
- [ ] Yes — out-of-band (DNS/HTTP) exfiltration **is possible**  

### Do application functions exhibit second-order SQL injection vulnerabilities?
- [ ] No — stored data is safely handled when retrieved for later queries  
- [ ] Yes — user input is stored and later used in an unsafe SQL query **without** validation  

### Are database service account privileges restricted to the minimum required?
- [ ] Yes — service account has **least privilege** (e.g., limited to specific tables/actions)  
- [ ] No — service account has elevated privileges (e.g., `DROP TABLE`, `FILE` privileges, or `xp_cmdshell` **enabled**)

---

## WSTG-INPV-06 — Testing for LDAP Injection

LDAP Injection occurs when an application incorporates user-supplied data into LDAP (Lightweight Directory Access Protocol) filters without proper sanitization or escaping, allowing an attacker to manipulate the query logic. By injecting special characters such as asterisks, parentheses, and logical operators, attackers can modify the search filter to bypass authentication mechanisms or exfiltrate sensitive directory information including usernames, group memberships, and organizational attributes. This vulnerability typically manifests in features like corporate directory searches, employee portals, or single sign-on (SSO) systems that interface with Active Directory or OpenLDAP. From an attacker's perspective, successful exploitation often involves blind techniques to enumerate the directory structure or attribute values bit-by-bit when direct query output is suppressed by the application.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### Does the application utilize LDAP for authentication or directory searches?
- [ ] No — LDAP is **not used** in the application architecture  
- [ ] Yes — LDAP is used and all inputs are strictly sanitized or parameterized  
- [ ] Yes — LDAP is used and user input is concatenated into filters **without** proper escaping  

### Are LDAP meta-characters properly escaped or filtered in user input?
- [ ] Yes — characters such as `(`, `)`, `&`, `|`, `*`, and `\` are **not allowed** or are correctly escaped  
- [ ] No — meta-characters **can** be injected to alter the structure of the LDAP filter  

### Can the authentication mechanism be bypassed via injection?
- [ ] No — login logic is **not susceptible** to query manipulation  
- [ ] Yes — authentication **can** be bypassed using logical OR (`|`) or wildcard (`*`) injection in the username or password fields  

### Is blind data exfiltration possible from the directory service?
- [ ] No — search results are limited and no timing or boolean differences are observed  
- [ ] Yes — directory attributes **can** be enumerated bit-by-bit via boolean-based blind injection techniques  
- [ ] Yes — full directory records **are** reflected in the application response due to query manipulation  

### Are secondary application components (e.g., mailers, SSO) vulnerable to LDAP-based attribute injection?
- [ ] No — LDAP attributes are handled securely across all components  
- [ ] Yes — an attacker **can** modify their own attributes (e.g., email, group membership) via injection to escalate privileges or redirect communications

---

## WSTG-INPV-07 — XML Injection

XML Injection occurs when an application incorporates user-supplied data into an XML document or stream without proper validation, sanitization, or escaping of XML meta-characters. By injecting specific XML tags or structural elements, an attacker can manipulate the document's logic, bypass authentication mechanisms, or interfere with the integrity of the data processed by the backend. This vulnerability is commonly found in SOAP-based web services, REST APIs that accept XML, SAML assertions, and applications that generate XML configuration files or reports dynamically. From an attacker's perspective, the goal is to break out of the intended data context to alter the document structure, potentially leading to unauthorized privilege escalation or the execution of unintended business logic.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Test Status** | Not Performed |
| **Severity** | High / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Tools:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### Does the application accept and process XML-formatted input from the user?
- [ ] No — application does **not** process XML input  
- [ ] Yes — XML input is processed via APIs (SOAP/REST)  
- [ ] Yes — XML is processed via file uploads (SVG, DOCX, etc.)  

### Are XML meta-characters (e.g., `<`, `>`, `&`, `'`, `"`) properly escaped or sanitized?
- [ ] Yes — all meta-characters are **properly** escaped or sanitized *(Most secure)*  
- [ ] Yes — some filtering is applied but bypass **is possible** using encoding  
- [ ] No — raw input is embedded directly into XML structures **without** sanitization  

### Is server-side schema validation (XSD/DTD) enforced for all XML inputs?
- [ ] Yes — strict schema validation is **enabled** and prevents structural changes  
- [ ] Yes — validation is **enabled** but does **not** prevent tag injection within data fields  
- [ ] No — schema validation is **disabled** or not implemented  

### Can an attacker inject new XML tags to alter the document structure or logic?
- [ ] No — structure remains intact regardless of input  
- [ ] Yes — tags can be injected but **cannot** alter business logic  
- [ ] Yes — tag injection **is possible** and allows for logic bypass or data modification *(Critical)*  

### Can the XML parser be manipulated using CDATA sections or XML comments?
- [ ] No — parser correctly handles or rejects CDATA/comment markers  
- [ ] Yes — injection of `<![CDATA[...]]>` or `<!-- ... -->` **is possible** to hide or bypass filters

---

## WSTG-INPV-08 — Testing for SSI Injection

Server-Side Includes (SSI) Injection occurs when an application fails to sanitize user input before incorporating it into HTML files processed by the server's SSI engine. Attackers exploit this by injecting SSI directives, such as `<!--#exec cmd="id" -->`, to execute arbitrary shell commands, read local files, or exfiltrate environment variables. This vulnerability is typically found in applications serving legacy file extensions like `.shtml`, `.shtm`, or `.stm`, or where the web server is explicitly configured to parse SSI within standard HTML files. Successful exploitation grants the attacker the same permissions as the web server process, often leading to a full system compromise or sensitive data exposure.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Does the web server support and process SSI directives?
- [ ] No — server **does not** support SSI or file extensions like `.shtml` are **disabled**  
- [ ] Yes — SSI **is enabled** but user input is **not** reflected in processed pages  
- [ ] Yes — SSI **is enabled** and user input **is** reflected in processed pages  

### Can arbitrary system commands be executed via the `#exec` directive?
- [ ] No — `#exec` directive is **disabled** or restricted by server configuration  
- [ ] Yes — command execution **is possible** via `#exec cmd` or `#exec cgi` payloads  

### Is the exfiltration of sensitive files or environment variables possible?
- [ ] No — directives like `#include` or `#config` are **disabled**  
- [ ] Yes — reading local files (e.g., `/etc/passwd`) **is possible** via `#include virtual`  
- [ ] Yes — server environment variables **can** be exfiltrated via `#printenv` or `#echo`  

### Are input validation or WAF controls effective against SSI payloads?
- [ ] Yes — strict input validation **is applied** and bypass **not possible**  
- [ ] Yes — validation/WAF **is applied** but bypass **is possible** using character encoding or different SSI syntax  
- [ ] No — no validation or WAF protection is present for SSI characters (`<`, `!`, `#`, `-`, `"`)

---

## WSTG-INPV-09 — Testing for XPath Injection

XPath Injection occurs when an application uses user-supplied information to construct an XPath query for XML data, allowing an attacker to interfere with the query's logic. By injecting specific syntax such as single quotes, brackets, and logical operators, an attacker can navigate the XML document structure, bypass authentication, or exfiltrate sensitive data nodes. This vulnerability is commonly found in web services, configuration management interfaces, and legacy systems that rely on XML-based data stores rather than traditional SQL databases. From an attacker's perspective, this is often exploited using boolean-based or error-based techniques to systematically map out the XML tree and retrieve hidden values from the backend.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Tools:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### Are user inputs used to construct XPath queries properly sanitized or parameterized?
- [ ] Yes — inputs are **not** used in XPath queries or are strictly parameterized  
- [ ] Yes — robust input validation/encoding **is applied** and bypass **not possible**  
- [ ] Yes — some validation **is applied** but bypass **is possible** using encoded characters  
- [ ] No — user input is directly concatenated into XPath queries *(Critical)*  

### Does the application reveal XML/XPath structural details via error messages?
- [ ] No — generic error pages are shown and **no** XML details are leaked  
- [ ] Yes — error messages reveal the presence of XML processing but **no** path details  
- [ ] Yes — verbose error messages reveal XPath syntax, node names, or file paths  

### Is it possible to bypass authentication or authorization logic using XPath logic?
- [ ] No — authentication logic does **not** rely on XML/XPath or is securely implemented  
- [ ] Yes — login or permission checks can be bypassed using logic-based payloads like `' or 1=1 or '`  

### Can the XML document structure or data be enumerated via blind injection?
- [ ] No — application is **not** susceptible to boolean-based or time-based inference  
- [ ] Yes — node names and data **can** be exfiltrated using boolean-based inference  
- [ ] Yes — full XML tree traversal and data extraction **is possible**

---

## WSTG-INPV-10 — IMAP SMTP Injection

IMAP and SMTP injection vulnerabilities arise when a web application improperly filters user-supplied data before incorporating it into commands sent to a mail server. By injecting Carriage Return and Line Feed (CRLF) characters, an attacker can terminate the intended command and append arbitrary mail instructions, such as adding additional recipients (CC/BCC) or modifying the message body. These flaws are most frequently found in web-to-mail gateways, contact forms, and email clients that communicate directly with mail servers via protocols like IMAP4 or SMTP. Successful exploitation allows for the unauthorized relaying of spam, the exfiltration of sensitive email contents, and in some cases, the total compromise of the mail server's integrity.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### Does the application process user-supplied input to construct IMAP or SMTP commands?
- [ ] No — application does **not** interact with mail servers via user input  
- [ ] Yes — application interacts with mail servers but uses a secure API or library  
- [ ] Yes — application constructs raw mail commands using user-controlled parameters  

### Is input validated to prevent the injection of Carriage Return (`\r`) and Line Feed (`\n`) sequences?
- [ ] Yes — CRLF characters are **strictly** filtered, encoded, or rejected  
- [ ] Yes — input is validated against a strict allowlist of characters  
- [ ] No — CRLF characters are **not** filtered, allowing command termination and injection  

### Can an attacker inject additional mail headers such as `Bcc:` or `Cc:`?
- [ ] No — header modification is **not possible** due to strict input constraints  
- [ ] Yes — injection of additional headers **is possible** via CRLF sequences  
- [ ] Yes — mail relaying to external domains **is enabled** and exploitable *(Critical)*  

### Does the application allow the manipulation of IMAP commands to access unauthorized mailboxes?
- [ ] No — application does **not** use IMAP or strictly limits mailbox access to the authenticated user  
- [ ] Yes — IMAP command injection **is possible** but limited to metadata enumeration  
- [ ] Yes — IMAP command injection allows for the reading or deletion of other users' emails  

### Is the backend mail server vulnerable to command pipelining?
- [ ] No — mail server rejects batched commands or multiple commands in a single session  
- [ ] Yes — mail server processes multiple commands injected via a single input field

---

## WSTG-INPV-11 — Code Injection

Code injection occurs when an application evaluates user-supplied code snippets within its runtime environment, typically through unsafe language-specific functions like `eval()`, `exec()`, or `system()`. This vulnerability differs from command injection because it targets the programming language's execution context (e.g., PHP, Python, Node.js) rather than the underlying operating system shell. Attackers exploit these points to execute arbitrary logic, exfiltrate environment variables, or achieve full remote code execution (RCE) on the server. Pentesters should investigate features that process mathematical expressions, server-side templates, or dynamic configuration parameters where input might be interpreted as executable code.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Test Status** | Not Performed |
| **Severity** | Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Tools:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Do any application features evaluate dynamic input via language-specific evaluation functions?
- [ ] No — dynamic evaluation functions **are not** present in the codebase  
- [ ] Yes — functions like `eval()`, `exec()`, or `include()` are used but do **not** accept user input  
- [ ] Yes — functions are used and process user-supplied input  

### Are input validation or sanitization mechanisms applied to input before evaluation?
- [ ] Yes — input is strictly validated against a **hardcoded whitelist**  
- [ ] Yes — input is sanitized via blacklisting, but bypass **is possible**  
- [ ] No — input is passed directly to the evaluation function and **no controls** are applied  

### Is the execution environment sandboxed or restricted to prevent system-level access?
- [ ] Yes — the execution occurs in a highly restricted sandbox where system calls **cannot** be made  
- [ ] Yes — some restrictions exist, but sandbox escape **is possible**  
- [ ] No — the environment allows full access to the underlying language APIs and OS functions *(Critical)*  

### Can the code injection be used to exfiltrate sensitive information?
- [ ] No — execution is blind and no out-of-band communication **is possible**  
- [ ] Yes — environment variables or local files **can** be exfiltrated via HTTP/DNS requests  
- [ ] Yes — results of the executed code are **directly reflected** in the application response  

### Does the application allow for remote file inclusion or dynamic library loading?
- [ ] No — only local, predefined code paths can be executed  
- [ ] Yes — the application **can** be forced to load and execute code from a remote or attacker-controlled source

---

## WSTG-INPV-12 — Command Injection

Command injection occurs when an application passes unsafe user-supplied data, such as form inputs, HTTP headers, or cookies, to a system shell. This vulnerability allows an attacker to execute arbitrary operating system commands on the server, typically with the privileges of the web application process. From an attacker's perspective, this is exploited by using shell metacharacters like semicolons, pipes, or backticks to chain malicious commands to the intended execution flow. The impact is frequently catastrophic, leading to full system compromise, unauthorized data exfiltration, or lateral movement within the internal network.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Test Status** | Not Performed |
| **Severity** | Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Tools:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### Does the application invoke system-level commands or shell executables?
- [ ] No — application does **not** interact with the OS shell  
- [ ] Yes — application uses internal APIs or high-level libraries for system tasks  
- [ ] Yes — application directly invokes shell commands via system calls like `system()`, `exec()`, or `popen()`  

### Is user input validated or sanitized before being passed to system calls?
- [ ] Yes — strict allow-listing and input validation **is applied**  
- [ ] Yes — sanitization or escaping is used but bypass **is possible**  
- [ ] No — user input is passed directly to the shell **without** validation  

### Can shell metacharacters be used to manipulate the command execution flow?
- [ ] No — metacharacters are correctly filtered or escaped  
- [ ] Yes — in-band execution where command output is reflected in the response **is possible**  
- [ ] Yes — blind execution where no output is reflected **is possible**  

### Is out-of-band (OOB) exfiltration possible from the host?
- [ ] No — server has no egress or OOB signals (DNS/HTTP) fail  
- [ ] Yes — data exfiltration via DNS or HTTP-based OOB signals **is possible**  

### Does the application process run with elevated system privileges?
- [ ] No — application runs with a dedicated, low-privilege service account  
- [ ] Yes — application runs as `root`, `Administrator`, or a high-privilege user *(Critical)*

---

## WSTG-INPV-13 — Format String Injection

Format string injection occurs when a web application passes user-controlled input directly into the format string argument of a variadic function, such as C's `printf` family or low-level logging wrappers. While less common in modern managed-code web frameworks, this vulnerability remains critical in legacy CGI applications, native extensions, or edge-case logging systems interacting with low-level system libraries. Attackers exploit this by supplying format specifiers like `%x` or `%p` to leak sensitive memory addresses and data from the stack, or the `%n` specifier to perform arbitrary memory writes. Successful exploitation typically results in complete system compromise via remote code execution or, at minimum, an impactful denial-of-service condition through process crashes.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### Does the application process user input through native code or legacy CGI interfaces?
- [ ] No — application is built entirely on managed code frameworks (e.g., JVM, CLR)  
- [ ] Yes — application uses JNI, native C/C++ extensions, or legacy binary CGIs where format string vulnerabilities **can** exist  

### Are user-supplied inputs passed as the primary argument to format-aware functions?
- [ ] No — inputs are passed as data arguments, and format strings are **static** and **hardcoded**  
- [ ] Yes — user input is directly concatenated into the format string argument, but sanitization **is applied**  
- [ ] Yes — user input is directly used as the format string and no sanitization **is applied**  

### Is it possible to leak memory contents using format specifiers?
- [ ] No — input is validated and specifiers like `%p`, `%x`, or `%s` are **not processed**  
- [ ] Yes — supply of `%p` or `%x` specifiers results in the reflection of stack or heap memory addresses  
- [ ] Yes — sensitive data (e.g., pointers, stack cookies) **can** be exfiltrated via format specifiers  

### Can the application be crashed or manipulated using the `%n` specifier?
- [ ] No — specifiers that permit memory writes are **disabled** or blocked by the compiler/environment  
- [ ] Yes — the application crashes (DoS) when `%s%s%s%s` or similar strings are provided  
- [ ] Yes — arbitrary memory writes via `%n` are **possible**, potentially leading to Remote Code Execution (RCE)  

### Are modern binary protections (ASLR, DEP, Stack Canaries) bypassable through this injection?
- [ ] No — the environment is hardened and memory leakage **cannot** be used to bypass protections  
- [ ] Yes — format string injection **can** be used to leak base addresses, making ASLR/DEP bypass **possible**

---

## WSTG-INPV-14 — Testing for Incubated Vulnerabilities

Incubated vulnerabilities, also referred to as persistent or second-order vulnerabilities, occur when a malicious payload is stored by the application in a "cold" state and later executed or processed in a different context. This attack pattern typically targets back-end systems, administrative consoles, or automated reporting tools that retrieve data from a shared database or filesystem without adequate re-validation or output encoding. Pentesters must trace the lifecycle of user input from the point of entry (the "incubator") to every location where that data is eventually rendered or utilized by other components or secondary applications. Successful exploitation often leads to high-impact outcomes like administrative session hijacking via stored XSS or data exfiltration through second-order SQL injection in internal reporting modules.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Tools:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Are there endpoints where user-controlled input is stored for subsequent retrieval by other users or processes?
- [ ] No — application does **not** store user input for later use  
- [ ] Yes — input **is** stored in database fields, log files, or configuration settings  

### Is input validation or sanitization applied before data is written to the persistent storage layer?
- [ ] Yes — strict allow-list validation **is applied** at the point of entry  
- [ ] Yes — generic sanitization **is applied** but bypass **is possible** via encoding or non-standard characters  
- [ ] No — input is stored in its raw, unvalidated form  

### Is stored data properly encoded or sanitized when it is rendered in administrative or secondary interfaces?
- [ ] Yes — context-aware output encoding **is applied** everywhere the data is rendered  
- [ ] Yes — encoding **is applied** in some locations but **missing** in others (e.g., internal admin dashboard)  
- [ ] No — data is rendered without any encoding or sanitization, allowing for payload execution  

### Can payloads stored in the database affect back-end batch processes or out-of-band monitoring systems?
- [ ] No — back-end processes use safe parsing methods or parameterized queries  
- [ ] Yes — stored payloads **can** trigger interactions in back-end logic (e.g., CSV injection, command injection in logs)  
- [ ] Yes — stored payloads **can** trigger out-of-band (OOB) requests to attacker-controlled servers

---

## WSTG-INPV-15 — HTTP Splitting Smuggling

HTTP Splitting and Smuggling vulnerabilities arise from discrepancies in how frontend proxies and backend servers interpret and process HTTP request boundaries, specifically concerning `Content-Length` and `Transfer-Encoding` headers. By crafting ambiguous requests, an attacker can "smuggle" a hidden request to the backend or inject CRLF sequences to split a response, leading to cache poisoning, request hijacking, or security control bypass. These flaws typically manifest in complex environments using reverse proxies, load balancers, or CDNs that have inconsistent parsing logic. From an attacker's perspective, this allows for the redirection of user traffic, theft of sensitive session tokens, and the execution of unauthorized actions within the context of other users' sessions.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Tools:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### Is the environment susceptible to request smuggling via CL.TE or TE.CL discrepancies?
- [ ] No — frontend and backend servers handle request boundaries consistently  
- [ ] Yes — discrepancies exist but exploitation is **not possible** due to infrastructure mitigations  
- [ ] Yes — CL.TE or TE.CL smuggling **is possible**, allowing hidden requests to reach the backend *(High)*  
- [ ] Yes — TE.TE (double encoding/obfuscation) **is possible** to bypass frontend filters *(Critical)*  

### Can HTTP Response Splitting be achieved through CRLF injection in headers?
- [ ] No — application properly sanitizes CRLF sequences in all header inputs  
- [ ] Yes — CRLF sequences are reflected in headers but response splitting is **not possible**  
- [ ] Yes — CRLF injection **is possible**, allowing for header injection or cache poisoning  

### Are security controls (WAF/ACLs) bypassed using smuggled requests?
- [ ] No — security controls apply to both outer and smuggled requests  
- [ ] Yes — smuggled requests **can** bypass frontend WAF rules or IP-based ACLs  

### Is it possible to hijack other users' sessions or redirect their traffic?
- [ ] No — request/response streams are isolated and cannot be crossed  
- [ ] Yes — request smuggling **is possible** and allows capturing other users' requests *(Critical)*  
- [ ] Yes — response splitting **is possible** and allows for browser-side cache poisoning or XSS

---

## WSTG-INPV-16 — HTTP Request Smuggling

HTTP Request Smuggling (HRS) occurs when a chain of HTTP servers (such as a load balancer and a back-end web server) interprets the boundaries of a request differently, typically due to conflicting `Content-Length` and `Transfer-Encoding` headers. An attacker exploits this desynchronization to "smuggle" an entry into the back-end server's request buffer, effectively prefixing the next legitimate user's request with an attacker-controlled segment. This technique allows for severe attacks including session hijacking, bypassing security controls, and cache poisoning. Exploitation is usually performed by timing-based analysis or by observing unexpected responses from the back-end when subsequent requests are sent to the server.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Test Status** | Not Performed |
| **Severity** | Critical / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Tools:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Do the front-end and back-end servers handle the `Transfer-Encoding: chunked` header consistently?
- [ ] Yes — both servers handle chunked encoding identically and desynchronization **not possible**  
- [ ] No — servers interpret headers differently but sanitization/normalization **is applied**  
- [ ] No — CL.TE (Content-Length/Transfer-Encoding) desynchronization **is possible** *(Critical)*  
- [ ] No — TE.CL (Transfer-Encoding/Content-Length) desynchronization **is possible** *(Critical)*  

### Can the attacker poison the server or client-side cache using a smuggled request?
- [ ] No — caching is **disabled** or not susceptible to poisoning  
- [ ] Yes — smuggled requests **can** redirect users to malicious domains or inject scripts via cache poisoning  

### Is it possible to capture other users' requests or session tokens via request concatenation?
- [ ] No — smuggling does **not** allow for cross-user data exposure  
- [ ] Yes — smuggling allows appending the next user's request to an attacker-controlled `POST` body, making exfiltration **possible**  

### Does the infrastructure use HTTP/2, and is it susceptible to H2.CL or H2.TE desynchronization?
- [ ] No — application uses HTTP/1.1 only or HTTP/2 is implemented securely without downgrading  
- [ ] Yes — HTTP/2 to HTTP/1.1 cleartext downgrades occur and desynchronization **is possible**  

### Are malformed headers (e.g., `Transfer-Encoding:  chunked` with a space) handled securely?
- [ ] Yes — malformed headers are rejected or normalized across all tiers  
- [ ] No — TE.TE desynchronization **is possible** due to inconsistent handling of malformed or obscured headers

---

## WSTG-INPV-17 — Testing for Host Header Injection

Host Header Injection occurs when an application trusts the user-supplied `Host` header and incorporates it into server-side logic, such as link generation or redirects, without sufficient validation. Attackers manipulate this header to facilitate web cache poisoning, facilitate password reset poisoning by redirecting sensitive tokens to an external domain, or bypass security controls that rely on header-based routing. Successful exploitation allows an attacker to exfiltrate session data, perform account takeovers, or redirect unsuspecting users to malicious infrastructure. This vulnerability is most commonly found in load-balanced environments or applications that dynamically generate absolute URLs based on the incoming request context.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the Host header is used to generate sensitive links in emails, such as password resets.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Tools:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### Does the application reflect the `Host` header value in links, redirects, or scripts?
- [ ] No — application uses hardcoded base URLs or strictly validated configuration  
- [ ] Yes — `Host` header is reflected but strictly validated against a whitelist  
- [ ] Yes — `Host` header is reflected and bypass **is possible** via malformed values (e.g., `expected.com:attacker.com`)  
- [ ] Yes — `Host` header is reflected **without** any validation  

### Are alternative host-related headers processed by the application or infrastructure?
- [ ] No — headers like `X-Forwarded-Host`, `X-Host`, or `Forwarded` are ignored  
- [ ] Yes — alternative headers are processed but do **not** override internal logic  
- [ ] Yes — alternative headers **can** be used to override the primary `Host` header value  

### Can the `Host` header be manipulated to poison system-generated emails (e.g., password resets)?
- [ ] No — email links use a static, trusted domain from the server configuration  
- [ ] Yes — `Host` header influences email links but validation **cannot** be bypassed  
- [ ] Yes — `Host` header **can** be manipulated to redirect password reset tokens to an attacker-controlled domain *(Critical)*  

### Is the application susceptible to Web Cache Poisoning via the `Host` header?
- [ ] No — no caching mechanism is present or the `Host` header is part of the cache key  
- [ ] Yes — a cache is present but it strictly validates or ignores the `Host` header for unkeyed inputs  
- [ ] Yes — a cache exists and **can** be poisoned by an injected `Host` header to serve malicious content to other users  

### Does the server accept arbitrary `Host` headers for virtual host routing?
- [ ] No — server returns an error or a default page for unrecognized `Host` headers  
- [ ] Yes — server accepts any `Host` header, which **is** useful for reconnaissance or internal service enumeration

---

## WSTG-INPV-18 — Testing for Server-Side Template Injection

Server-Side Template Injection (SSTI) occurs when an application embeds user-controlled input into a server-side template engine without sufficient sanitization or escaping. This allows an attacker to inject malicious template directives, which are then executed on the server, potentially leading to full system compromise through remote code execution (RCE). SSTI typically manifests in features like dynamic email generation, profile pages, and notification systems that use engines such as Jinja2, Twig, or FreeMarker. From an attacker's perspective, this vulnerability is often exploited to leak sensitive environment variables, read internal files, or execute system commands by leveraging the template engine's native objects and methods.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Test Status** | Not Performed |
| **Severity** | Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Tools:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### Does the application use a server-side template engine to process user input?
- [ ] No — application does **not** use server-side templates for dynamic content  
- [ ] Yes — templates are used but user input is **not** embedded in template directives  
- [ ] Yes — user input is embedded in templates **without** proper sanitization  

### Are basic arithmetic expressions evaluated when injected into input fields?
- [ ] No — payloads like `{{7*7}}` or `${7*7}` are reflected as literal strings  
- [ ] Yes — expressions are evaluated (e.g., returning `49`), confirming the template engine **is vulnerable**  

### Can the template engine's built-in objects or methods be accessed?
- [ ] No — access to dangerous objects, methods, or configurations is **restricted** or sandboxed  
- [ ] Yes — built-in objects (e.g., `self`, `config`, `__class__`) **can** be accessed to leak sensitive data  
- [ ] Yes — remote code execution (RCE) via system calls or OS libraries **is possible**  

### Is there a sandbox or allowlist mechanism preventing the execution of dangerous directives?
- [ ] Yes — strict allowlist or secure sandbox is in place and bypass **not possible**  
- [ ] Yes — sandbox is present but bypass **is possible** via specific engine-dependent payloads  
- [ ] No — no sandbox or allowlist **is applied** to the template engine

---

## WSTG-INPV-19 — Testing for Server-Side Request Forgery (SSRF)

Server-Side Request Forgery occurs when an attacker can influence a web application to make unauthorized requests from the server to internal or external resources. By manipulating parameters that expect URLs, hostnames, or IP addresses, an attacker can bypass network-level controls like firewalls and ACLs to probe internal network segments, access cloud metadata services (IMDS), or interact with vulnerable internal APIs and databases. This vulnerability typically manifests in features involving image fetching, PDF generation, webhooks, or file uploads via URL. From an attacker's perspective, SSRF is a powerful primitive used to pivot into isolated environments, exfiltrate sensitive configuration data, or potentially achieve remote code execution through interaction with internal services like Redis or Memcached.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Tools:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### Does the application process user-supplied URLs or hostnames?
- [ ] No — no features accept external URLs or hostnames  
- [ ] Yes — feature exists but input **is strictly validated** against an allow-list  
- [ ] Yes — feature exists and accepts user-supplied URLs  

### Are validation controls or blacklists applied to the requested destination?
- [ ] Yes — a strict **allow-list** of trusted domains/IPs is enforced  
- [ ] Yes — a **deny-list** is used (e.g., blocking `127.0.0.1`, `169.254.169.254`)  
- [ ] No — **no validation** or filtering is applied to the input  

### Is it possible to bypass filters via encoding, redirects, or DNS rebinding?
- [ ] No — filters are robust and handle redirects/DNS resolution securely  
- [ ] Yes — bypass **is possible** using URL encoding or alternative IP formats (hex, octal)  
- [ ] Yes — bypass **is possible** via 3xx HTTP redirects to internal targets  
- [ ] Yes — bypass **is possible** via DNS rebinding attacks to resolve to internal IPs  

### Can the application reach internal metadata services or local interfaces?
- [ ] No — local network and cloud metadata endpoints are **not reachable**  
- [ ] Yes — access to localhost (`127.0.0.1`) or loopback services **is possible**  
- [ ] Yes — access to cloud metadata services (e.g., AWS/GCP/Azure IMDS) **is possible** *(Critical)*  

### What is the nature of the SSRF response?
- [ ] Blind — no response or metadata is returned to the user  
- [ ] Partial — response metadata (headers, size, timing) is returned to the user  
- [ ] Full — the full response body from the internal resource **is reflected** to the user

---

## WSTG-INPV-20 — Mass Assignment

Mass Assignment, also known as Overposting or Insecure Deserialization of parameters, occurs when a web application automatically binds incoming HTTP request parameters to internal object properties without proper attribute filtering. This vulnerability is prevalent in modern MVC frameworks and RESTful APIs where JSON, XML, or form-encoded data is directly deserialized into application-side data models. Attackers exploit this behavior by injecting additional, unlisted parameters into a request—such as `isAdmin`, `role`, or `account_balance`—to modify sensitive fields that the developers did not intend to expose to user control. Successful exploitation typically results in privilege escalation, unauthorized data modification, or the bypassing of critical business logic workflows.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Test Status** | Not Performed |
| **Severity** | High / Medium* |

> *Severity becomes High if administrative attributes, permission levels, or billing fields can be modified.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### Does the application use automatic binding for incoming request parameters?
- [ ] No — parameters are manually mapped to specific variables or safe objects  
- [ ] Yes — automatic binding is used but strict **white-lists** or DTOs (Data Transfer Objects) are enforced  
- [ ] Yes — automatic binding is used and sensitive internal attributes **can** be modified  

### Can administrative or permission-related attributes be successfully injected?
- [ ] No — injected parameters are ignored, stripped, or cause a validation error  
- [ ] Yes — extra parameters are accepted but do **not** affect the backend object state  
- [ ] Yes — injecting parameters like `is_admin`, `role`, or `permissions` **successfully** escalates privileges *(Critical)*  

### Is it possible to modify sensitive business logic or financial fields via overposting?
- [ ] No — fields such as `price`, `balance`, or `status` are protected from mass assignment  
- [ ] Yes — sensitive business attributes **can** be modified by adding them to the JSON or form request body  

### Does the application reveal internal object structures that facilitate parameter guessing?
- [ ] No — responses only include intended public fields and documentation is restricted  
- [ ] Yes — internal object fields are visible in GET responses, making parameter discovery **possible**  
- [ ] Yes — API documentation (Swagger/OpenAPI) explicitly lists internal properties that **can** be assigned

---

## WSTG-ERRH-01 — Testing for Improper Error Handling

Improper error handling occurs when a web application discloses sensitive technical details—such as stack traces, database schema information, or internal file paths—through its error responses. Attackers trigger these errors by submitting malformed input, accessing non-existent resources, or inducing server-side exceptions to map the application's underlying architecture and identify potential vectors for further exploitation. This information leakage often serves as a precursor to more severe attacks, including SQL injection or path traversal, by providing the attacker with precise environment specifications. A secure implementation must provide generic user-friendly messages while capturing detailed diagnostics only in secure, server-side logs.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### Does the application use a generic, global error handler for unhandled exceptions?
- [ ] Yes — generic error pages are **enabled** and reveal **no** technical details  
- [ ] Yes — custom error pages are used but leakage **is possible** via specific headers  
- [ ] No — default server error pages (e.g., Tomcat, IIS, Nginx) are **visible**  

### Can technical information be enumerated through malformed input or boundary cases?
- [ ] No — errors are handled gracefully with unique reference IDs for logging  
- [ ] Yes — stack traces or database queries **are disclosed** in responses  
- [ ] Yes — internal file paths, environment variables, or server version strings **are disclosed**  

### Are API responses leaking verbose error objects or debug information?
- [ ] No — APIs return standardized error codes and sanitized JSON messages  
- [ ] Yes — APIs return verbose debug objects or full exception details in the response body  
- [ ] Yes — stack traces are included in the `details` or `exception` fields of the JSON response  

### Does the application behave differently (Time-based or Content-based) when errors occur?
- [ ] No — response signatures and timings are consistent regardless of the error type  
- [ ] Yes — differential responses **can** be used to enumerate valid vs. invalid states (e.g., User Enumeration)

---

## WSTG-ERRH-02 — Testing for Stack Traces

Stack traces are generated when an application fails to gracefully handle an exception, resulting in the disclosure of the internal execution state and call hierarchy to the end user. For a penetration tester, these traces are invaluable as they reveal the application framework, library versions, internal file system paths, and logic flow, which significantly reduces the effort required for reconnaissance. Exploitation involves intentionally triggering errors via malformed input, unexpected data types, or boundary condition violations to force the application into an unstable state and exfiltrate information about the underlying technology stack. This leakage often provides the necessary context to identify vulnerable third-party components or to craft specific exploits for logic flaws discovered within the disclosed code paths.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium* |

> *Severity increases to Medium if the stack trace reveals sensitive architectural details, database queries, or internal configuration parameters.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Are full stack traces returned to the client upon application errors?
- [ ] No — application returns generic error pages or custom error messages  
- [ ] Yes — stack traces are **enabled** but only for specific, non-sensitive modules  
- [ ] Yes — full stack traces are **enabled** and visible in the HTTP response body  

### Do error messages disclose sensitive environmental information?
- [ ] No — error messages contain no technical or environmental details  
- [ ] Yes — messages reveal **internal file paths** or server-side **directory structures**  
- [ ] Yes — messages reveal **database schemas**, **SQL queries**, or **third-party library versions**  

### Can stack traces be triggered by manipulating input parameters?
- [ ] No — malformed input is handled gracefully or rejected by validation  
- [ ] Yes — traces can be triggered by **unexpected data types** (e.g., submitting an array where a string is expected)  
- [ ] Yes — traces are triggered by **null bytes**, **special characters**, or **boundary values**  

### Is a global error handler consistently applied across the application?
- [ ] Yes — a global exception handler is **consistently applied** across all tested endpoints  
- [ ] Yes — a handler is present but **not applied** to legacy, API, or newly added endpoints  
- [ ] No — no global error handling mechanism is **detected**, resulting in default server errors

---

## WSTG-CRYP-01 — Testing for Weak Transport Layer Security

Testing for weak transport layer security involves analyzing the implementation and configuration of SSL/TLS to ensure data confidentiality and integrity during transit. Attackers target misconfigurations such as outdated protocols (SSLv2, TLS 1.0), weak cipher suites (NULL, RC4, or anonymous), and invalid or weakly signed certificates to perform Man-in-the-Middle (MitM) attacks. This test typically targets the application's web server or load balancer endpoints where encrypted communication is established. Successful exploitation allows an adversary to intercept sensitive data, hijack user sessions, or downgrade connections to insecure states through protocol downgrade or traffic decryption.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Test Status** | Not Performed |
| **Severity** | High / Medium* |

> *Severity is High if sensitive data (PII, credentials, session tokens) is transmitted; Medium for general informational traffic.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Are outdated or insecure TLS/SSL protocols supported?
- [ ] No — only TLS 1.2 and TLS 1.3 are **enabled**  
- [ ] Yes — TLS 1.0 or TLS 1.1 are **enabled**  
- [ ] Yes — SSLv2 or SSLv3 are **enabled** *(Critical)*  

### Are weak or insecure cipher suites accepted by the server?
- [ ] No — only strong, modern ciphers (e.g., AES-GCM, ChaCha20) are **supported**  
- [ ] Yes — ciphers with weak encryption (e.g., 3DES, RC4) or low bit-length are **supported**  
- [ ] Yes — NULL, anonymous (ADH), or export ciphers are **supported** *(Critical)*  

### Is the server certificate valid and trusted?
- [ ] Yes — certificate is valid, matches the domain, and is signed by a **trusted CA**  
- [ ] No — certificate is **expired** or uses a **weak signature algorithm** (e.g., SHA-1)  
- [ ] No — certificate is **self-signed** or domain name **mismatch** occurs  

### Does the server support Secure Renegotiation and prevent Downgrade attacks?
- [ ] Yes — Secure Renegotiation is **supported** and TLS Fallback SCSV is **enabled**  
- [ ] No — Secure Renegotiation is **not supported**, potentially allowing MitM injection  
- [ ] No — Server is vulnerable to **POODLE** or **ROBOT** attacks  

### Is HTTP Strict Transport Security (HSTS) properly implemented?

| Header | Present | Missing |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Yes — HSTS header is present with a long `max-age` and `includeSubDomains` **enabled**  
- [ ] Yes — HSTS header is present but missing `includeSubDomains` or has a **short max-age**  
- [ ] No — HSTS header is **not present**

---

## WSTG-CRYP-02 — Testing for Padding Oracle

Padding Oracle vulnerabilities occur when an application's decryption process leaks information about whether the padding of a decrypted ciphertext is valid or invalid. By observing these side-channel responses—such as distinct HTTP status codes, specific error messages, or response timing variations—an attacker can decrypt data without knowing the encryption key and potentially re-encrypt arbitrary plaintext. This vulnerability typically manifests in encrypted session cookies, authentication tokens, or URL parameters using block ciphers in Cipher Block Chaining (CBC) mode with PKCS#7 padding. Exploitation allows for complete loss of confidentiality and can lead to integrity compromise through the construction of forged encrypted blocks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical* |

> *Severity is Critical if the vulnerability resides in session management, identity tokens, or PII encryption.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Is a block cipher used in CBC mode for sensitive data?
- [ ] No — application uses authenticated encryption (AES-GCM/ChaCha20-Poly1305)  
- [ ] Yes — application uses block ciphers but padding **is not** required (e.g., Stream ciphers or CTR mode)  
- [ ] Yes — application uses CBC mode and padding **is applied**  

### Does the server disclose padding validity through side channels?
- [ ] No — server returns uniform error messages and consistent response times  
- [ ] Yes — server returns different HTTP status codes (e.g., 200 vs 500) for padding errors  
- [ ] Yes — server returns specific error messages (e.g., "Invalid Padding", "Decryption failed")  
- [ ] Yes — server exhibits measurable timing differences during decryption failure  

### Is automated decryption of ciphertext possible?
- [ ] No — side channels are **not** stable enough for exploitation  
- [ ] Yes — ciphertext **can** be partially decrypted (last few blocks)  
- [ ] Yes — full plaintext recovery **is possible** using tools like `PadBuster`  

### Can the attacker perform bit-flipping or forge valid ciphertexts?
- [ ] No — integrity checks (HMAC) are verified **before** decryption  
- [ ] Yes — no integrity check is present, but payload construction **is not** feasible  
- [ ] Yes — arbitrary ciphertext construction **is possible**, allowing for privilege escalation or data injection

---

## WSTG-CRYP-03 — Testing for Sensitive Information Sent Via Unencrypted Channels

Transmitting sensitive data via unencrypted channels, such as plain HTTP, exposes information to interception and modification via Man-in-the-Middle (MITM) attacks. This vulnerability typically occurs when web applications fail to enforce Transport Layer Security (TLS) across all endpoints, particularly in legacy components, login forms, or during the initial transition from HTTP to HTTPS. From an attacker's perspective, this allows for the passive sniffing of authentication credentials, session tokens, and personally identifiable information (PII) on compromised or untrusted network segments like public Wi-Fi. Ensuring that all sensitive data is encapsulated within a robust TLS tunnel is fundamental to maintaining the confidentiality and integrity of user interactions and preventing session hijacking.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Test Status** | Not Performed |
| **Severity** | High / Medium |

> *Severity becomes High if authentication credentials, session identifiers, or PII are transmitted in cleartext.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### Does the application allow communication over unencrypted HTTP for sensitive endpoints?
- [ ] No — all application traffic is strictly forced over HTTPS *(Most secure)*  
- [ ] Yes — non-sensitive pages are accessible over HTTP but sensitive endpoints **are not**  
- [ ] Yes — sensitive endpoints (login, checkout, profile) **are** accessible over unencrypted HTTP *(High Risk)*  

### Is there a global redirection from HTTP to HTTPS?
- [ ] Yes — application performs a 301/302 redirect to HTTPS for all HTTP requests  
- [ ] Yes — redirection is only performed for specific sensitive endpoints  
- [ ] No — no redirection from HTTP to HTTPS **is applied**  

### Is HTTP Strict Transport Security (HSTS) implemented?
- [ ] Yes — HSTS is **enabled** with a long `max-age` and includes the `includeSubDomains` and `preload` directives  
- [ ] Yes — HSTS is **enabled** but lacks the `includeSubDomains` or `preload` directives  
- [ ] No — HSTS is **not enabled** on the application server  

### Are sensitive cookies protected from cleartext transmission?

| Cookie Name | Secure Flag Present | Secure Flag Missing |
|-------------|:-------------------:|:------------------:|
| `SessionID` |                     |                    |
| `AuthToken` |                     |                    |
| `CSRF-Token`|                     |                    |

### Does the application transmit sensitive data to third-party APIs via unencrypted channels?
- [ ] No — all external API calls are performed over encrypted (HTTPS) tunnels  
- [ ] Yes — some non-sensitive telemetry is sent via HTTP  
- [ ] Yes — API keys, PII, or credentials **are** sent to third parties via unencrypted HTTP

---

## WSTG-CRYP-04 — Testing for Weak Cryptographic Primitives

Weak cryptographic primitives involve the use of outdated or insecure algorithms, such as MD5, SHA-1, DES, or RC4, which are susceptible to collision attacks, frequency analysis, or brute-force decryption. These weaknesses typically occur in password hashing, data encryption at rest, session token generation, or digital signature verification within the application backend. From an attacker's perspective, identifying these primitives allows for the compromise of data integrity and confidentiality, such as cracking intercepted hashes to retrieve cleartext passwords or forging authentication tokens. Modern applications should instead utilize industry-standard, collision-resistant, and high-entropy primitives like Argon2, Bcrypt, or AES-GCM to ensure robust protection against cryptanalysis.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Are deprecated hashing algorithms such as MD5 or SHA-1 used for sensitive data storage?
- [ ] No — application uses modern collision-resistant algorithms such as Argon2 or Bcrypt  
- [ ] Yes — deprecated algorithms are used but **only** for non-security-sensitive integrity checks  
- [ ] Yes — deprecated algorithms **are used** for passwords or sensitive tokens *(High)*  

### Are weak symmetric encryption ciphers like DES, 3DES, or RC4 currently employed?
- [ ] No — AES (128-bit or higher) in a secure mode like GCM or CBC is used  
- [ ] Yes — legacy ciphers are **enabled** for backward compatibility but are **not** the default  
- [ ] Yes — legacy ciphers are the primary method for protecting data at rest or in transit  

### Does the application implement "roll-your-own" or non-standard cryptographic logic?
- [ ] No — application strictly adheres to standard, peer-reviewed cryptographic libraries  
- [ ] Yes — custom wrappers exist but the underlying primitives **are** industry standard  
- [ ] Yes — proprietary cryptographic algorithms or custom logic **is applied** to sensitive data *(Critical)*  

### Are cryptographic salts and initialization vectors (IVs) managed according to best practices?
- [ ] Yes — salts are unique per-user and IVs are unpredictable for every operation  
- [ ] Yes — salts are used but are **not** unique across all records  
- [ ] No — salts are static, hardcoded, or absent, and IVs **are reused** across multiple sessions  

### Is the bit-length of asymmetric keys (RSA, Diffie-Hellman) sufficient for modern standards?
- [ ] Yes — RSA/DH keys are 2048 bits or higher, or Elliptic Curve (ECC) is used  
- [ ] No — keys are less than 2048 bits but bypass **is not** trivial  
- [ ] No — keys are 1024 bits or smaller, making them **vulnerable** to factorization or computational attacks

---

## WSTG-BUSL-01 — Test Business Logic Data Validation

Business logic data validation testing focuses on the application's ability to identify and reject data that is syntactically correct but logically invalid within the context of the business domain. Attackers exploit these flaws by submitting unexpected values—such as negative quantities in a shopping cart, dates in the past for future-dated events, or extremely large integers—to bypass constraints and manipulate the application's state. These vulnerabilities often reside in multi-step workflows, checkout processes, and account management modules where the server-side logic fails to verify the contextual integrity of user-supplied data. Successful exploitation can lead to financial loss, integrity compromise, and unauthorized access to restricted features or data.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Test Status** | Not Performed |
| **Severity** | High / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### Does the application enforce logical range limits on numeric inputs?
- [ ] Yes — application correctly rejects negative, zero, or excessively large values where inappropriate *(Most secure)*  
- [ ] Yes — application rejects some invalid ranges but **is** vulnerable to integer overflow or boundary bypass  
- [ ] No — application accepts any numeric value regardless of the business context *(Critical)*  

### Are server-side validation checks consistent across all layers of the application?
- [ ] Yes — validation is applied consistently at both the API/service and database layers  
- [ ] Yes — validation **is applied** at the UI/Frontend but bypass via direct API request **is possible**  
- [ ] No — validation is **not applied** or is inconsistent, allowing for logical data corruption  

### Can the business logic be subverted by manipulating data in multi-step workflows?
- [ ] No — state is maintained securely on the server and data from previous steps **cannot** be tampered with  
- [ ] Yes — validation occurs in early steps but final submission **does not** re-verify the integrity of the data  
- [ ] Yes — workflow bypass **is possible** by skipping steps or submitting out-of-order data  

### Does the application properly handle "edge case" data that bypasses business constraints?
- [ ] Yes — constraints are robust and handle unusual but syntactically valid inputs (e.g., leap years, time zone shifts)  
- [ ] Yes — some edge cases are handled but specific combinations **can** lead to unexpected behavior  
- [ ] No — edge cases directly lead to logic failures, such as free purchases or unauthorized status changes

---

## WSTG-BUSL-02 — Test Ability to Forge Requests

Forging requests within a business logic context involves manipulating application-defined parameters to perform actions outside the intended workflow or authorization level. Attackers target multi-step processes, such as checkout systems or account registration, where the state is maintained via client-side parameters that the server fails to re-validate against a trusted backend source. By altering hidden fields, JSON values, or REST parameters like prices, quantities, or internal status flags, an attacker can bypass payment requirements, escalate privileges, or trigger unintended state changes. This vulnerability typically manifests in stateful web forms or APIs where the server implicitly trusts the client's representation of the business state during a transaction.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical* |

> *Severity becomes Critical if the forged request allows for financial loss, unauthorized administrative actions, or full account takeover.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### Does the application validate transactional parameters against a server-side "source of truth"?
- [ ] Yes — all sensitive parameters (price, role, ID) are validated against the database and bypass **not possible**  
- [ ] Yes — validation is applied but can be bypassed via parameter pollution or alternative encoding  
- [ ] No — the application trusts client-side values to determine the cost or outcome of a transaction *(Critical)*  

### Can business-critical values (e.g., quantities, amounts) be modified to unauthorized values?
- [ ] No — negative values, zero values, or excessively high values are rejected by the server  
- [ ] Yes — the server accepts modified values but only within a limited range  
- [ ] Yes — negative or zero values **can** be submitted, leading to financial or logic bypasses  

### Are hidden fields or API parameters used to maintain state across multi-step processes?
- [ ] No — state is maintained securely via server-side sessions or signed tokens  
- [ ] Yes — state is passed via the client but integrity checks (HMAC/Signatures) are **enabled** and robust  
- [ ] Yes — state is passed via the client and integrity checks are **disabled** or can be stripped  

### Is it possible to forge requests that skip mandatory intermediate steps in a workflow?
- [ ] No — the server enforces a strict sequence of operations for every transaction  
- [ ] Yes — steps **can** be skipped but the final action still requires valid data from previous steps  
- [ ] Yes — the final "submit" or "complete" request **can** be forged directly to bypass authorization or payment steps

---

## WSTG-BUSL-03 — Test Integrity Checks

Integrity check testing focuses on verifying that the application ensures data remains unaltered and trustworthy as it moves through various business processes or between the client and server. Attackers target this logic by tampering with critical parameters—such as product prices, quantities, user privileges, or transaction states—within HTTP requests, hidden fields, or cookies. If the application lacks server-side verification or robust integrity controls like Hashing or HMACs, an adversary can manipulate these values to commit fraud, escalate their own authority, or bypass intended business constraints. This test is vital for any application handling financial transactions, sensitive state transitions, or multi-step workflows where the output of one stage serves as the trusted input for the next.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the integrity failure allows for financial theft, unauthorized privilege escalation, or modification of persistent records.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Are sensitive business parameters exposed to client-side tampering?
- [ ] No — sensitive values are managed exclusively on the server-side  
- [ ] Yes — values like price, quantity, or status flags **are** exposed but encrypted  
- [ ] Yes — values **are** exposed in plain text within hidden fields, URL parameters, or cookies  

### Is an integrity mechanism (e.g., HMAC, Checksum) applied to client-controlled parameters?
- [ ] Yes — a robust integrity check **is applied** and verified on every request  
- [ ] Yes — an integrity check **is applied** but uses a weak/predictable algorithm or hardcoded secret  
- [ ] No — no integrity checks **are applied** to sensitive parameters  

### Can the integrity check be bypassed or recalculated by an attacker?
- [ ] No — signature verification is strictly enforced and bypass **not possible**  
- [ ] Yes — signature verification can be bypassed by omitting the signature parameter  
- [ ] Yes — the signature **can** be recalculated because the secret key or logic is exposed/weak  

### Does the application perform backend re-validation of data against a trusted source?
- [ ] Yes — the server re-validates all values against the database and bypass **not possible**  
- [ ] Yes — the server performs partial validation but bypass **is possible** through specific edge cases  
- [ ] No — the server trusts client-provided data without backend verification *(Critical)*  

### Is it possible to tamper with the state of a multi-step process?
- [ ] No — session state is managed server-side and cannot be manipulated  
- [ ] Yes — state information is passed via the client and tampering **is possible** to skip steps or repeat actions

---

## WSTG-BUSL-04 — Test for Process Timing

Process timing attacks involve measuring the time an application takes to process specific requests to infer sensitive information about internal states or data existence. By analyzing millisecond-level discrepancies in response times—often caused by early-exit conditional logic, database lookups, or non-constant-time cryptographic comparisons—attackers can perform side-channel analysis to enumerate valid usernames, verify password fragments, or confirm the existence of specific records. These vulnerabilities typically occur at authentication endpoints, password reset forms, and resource-heavy API filters where backend logic branches based on input validity. From an attacker's perspective, these timing differences serve as a "boolean oracle" that reveals truths about the backend data even when the application returns identical, generic error messages to the user.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Test Status** | Not Performed |
| **Severity** | Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Tools:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### Does the application exhibit varying response times for valid vs. invalid resource requests?
- [ ] No — response times are identical regardless of input validity  
- [ ] Yes — negligible timing differences exist but are **not** statistically significant  
- [ ] Yes — consistent timing differences are **observable** and provide a reliable oracle  

### Are constant-time comparison functions or artificial delays implemented to normalize response times?
- [ ] Yes — constant-time algorithms are **applied** to all sensitive comparison operations *(Most secure)*  
- [ ] Yes — artificial delays or jitter are **enabled**, but the underlying logic remains non-constant-time  
- [ ] No — no timing mitigations are **applied**, allowing direct measurement of logic branches  

### Can an attacker use statistical analysis to isolate the timing signal from network noise?
- [ ] No — network jitter and application noise make timing analysis **not possible**  
- [ ] Yes — with a sufficient sample size, the timing signal **can** be isolated from noise  
- [ ] Yes — the timing delta is large enough that statistical normalization is **not** required  

### Is account enumeration possible via the login or password reset functionality?
- [ ] No — account existence cannot be determined through timing  
- [ ] Yes — timing discrepancies allow a remote attacker to **enumerate** valid usernames or emails  

### Do resource-intensive operations (e.g., file processing, complex searches) leak data existence via timing?
- [ ] No — resource consumption is uniform regardless of whether a record is found  
- [ ] Yes — existence of sensitive records **can** be inferred by measuring the duration of backend processing

---

## WSTG-BUSL-05 — Test Number of Times a Function Can Be Used Limits

This test evaluates whether an application enforces business logic constraints on the frequency and total number of times a specific action or function can be executed by a single user, session, or IP address. Failure to implement these limits allows attackers to abuse high-value functions—such as sending SMS OTPs, triggering password reset emails, applying discount codes, or performing heavy data exports—leading to financial loss, resource exhaustion, or reputational damage. These vulnerabilities are typically found in transactional endpoints or communication features and are exploited through automated scripts that repeat the action far beyond intended business thresholds. From an attacker's perspective, this is a prime target for SMS pumping, mail bombing, or brute-forcing workflows that lack explicit rate-limiting or count-based throttles.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the function involves financial transactions, SMS costs, or impacts system-wide availability.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### Does the application define and enforce limits on sensitive business functions?
- [ ] Yes — limits are strictly enforced and bypass **not possible**  
- [ ] Yes — limits are enforced but are too high to prevent abuse  
- [ ] No — no limits are applied to the function execution  

### Are rate limits and usage counters enforced server-side?
- [ ] Yes — enforcement is strictly server-side and **cannot** be manipulated  
- [ ] No — enforcement relies on client-side logic (e.g., JavaScript or hidden fields)  
- [ ] No — no enforcement exists at any level  

### Can the usage limits be bypassed through header or session manipulation?
- [ ] No — bypass via `X-Forwarded-For`, `Origin`, or session rotation is **not possible**  
- [ ] Yes — limits **can** be bypassed by spoofing IP headers or clearing cookies  
- [ ] Yes — limits **can** be bypassed by creating multiple accounts or sessions  

### Does exceeding the limit trigger an appropriate defensive response?
- [ ] Yes — application returns `429 Too Many Requests` and logs the event *(Most secure)*  
- [ ] Yes — application returns an error but does **not** log the potential abuse  
- [ ] No — application continues to process requests but ignores the output  
- [ ] No — application provides no response or crashes under load  

### Is the impact of abusing the function significant to the business?
- [ ] No — function abuse has negligible cost or resource impact  
- [ ] Yes — abuse **can** lead to minor resource consumption or user annoyance  
- [ ] Yes — abuse **can** lead to significant financial costs (e.g., SMS fees) or service denial *(Critical)*

---

## WSTG-BUSL-06 — Testing for the Circumvention of Work Flows

Workflow circumvention involves manipulating an application's logic to bypass intended sequences or prerequisites within multi-step processes. Attackers identify sensitive endpoints—such as payment confirmations, administrative approvals, or MFA completion screens—and attempt to access them directly, skipping mandatory intermediate steps. This vulnerability occurs when server-side logic fails to maintain a robust state machine, relying instead on the assumption that a user follows the UI-driven path. Successful exploitation can lead to critical business logic failures, including unauthorized transactions, privilege escalation, and the bypass of identity verification mechanisms.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the bypass allows for unauthorized financial transactions, privilege escalation, or administrative access.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### Does the application contain multi-stage business processes?
- [ ] No — application consists only of single-request actions  
- [ ] Yes — multi-stage processes (e.g., shopping carts, wizard forms, registration) **are** present  

### Is the sequence of steps strictly enforced by the server?
- [ ] Yes — server-side state tracking ensures steps **cannot** be skipped  
- [ ] Yes — client-side controls suggest a sequence, but server **does not** validate progress  
- [ ] No — the application **does not** enforce any specific order of operations  

### Can an attacker reach the final stage of a process by directly requesting the terminal URL or endpoint?
- [ ] No — direct access to terminal steps is **not possible** without prior completion  
- [ ] Yes — terminal steps (e.g., `/api/v1/checkout/complete`) **can** be reached by direct request  

### Does the application verify that prerequisite data from previous steps is present and valid?
- [ ] Yes — server validates all previous step data before proceeding  
- [ ] No — server **is** vulnerable to "skipping" steps where data is expected but not verified  

### Can security controls like MFA or identity verification be bypassed by manipulating the workflow?
- [ ] No — critical controls **cannot** be circumvented via flow manipulation  
- [ ] Yes — bypass of MFA or identity checks **is possible** by jumping to the post-verification step

---

## WSTG-BUSL-07 — Test Defenses Against Application Misuse

Defenses against application misuse evaluate the application's ability to identify, log, and respond to anomalous user behavior that deviates from intended business logic. Unlike traditional signature-based security, these defenses focus on detecting patterns such as rapid-fire requests, credential stuffing, or sequential resource enumeration that signify automated abuse. From an attacker's perspective, this test determines the resilience of the application against automation and "low and slow" attacks designed to exfiltrate data or exhaust resources without triggering standard security alerts. Failure to implement these defenses often results in massive data scraping, account takeovers, or denial of service through legitimate but resource-intensive application features.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Are rate-limiting or throttling mechanisms enforced on sensitive endpoints?
- [ ] Yes — strict rate limiting **is applied** and enforced across all sensitive endpoints *(Most secure)*  
- [ ] Yes — rate limiting **is applied** but can be bypassed via IP rotation or header manipulation  
- [ ] Yes — rate limiting **is applied** only to authentication endpoints, leaving others exposed  
- [ ] No — no rate limiting or throttling **is applied** to any application features  

### Does the application detect and block automated interaction patterns?
- [ ] Yes — behavioral analysis or CAPTCHAs **are enabled** and effectively block bots  
- [ ] Yes — anti-automation **is enabled** but bypass **is possible** using headless browsers or session cycling  
- [ ] No — application **cannot** distinguish between a human user and an automated script  

### Is anomalous behavior logged and followed by a defensive response?
- [ ] Yes — misuse triggers automated blocking and alerts the security team  
- [ ] Yes — misuse is logged for audit but **no** automated defensive action is taken  
- [ ] No — the application **does not** log or respond to unusual activity patterns  

### Can defenses be bypassed by manipulating client-side identifiers or headers?
- [ ] No — defenses rely on server-side state and verified telemetry  
- [ ] Yes — controls **can** be bypassed by clearing cookies or rotating `User-Agent` strings  
- [ ] Yes — controls **can** be bypassed by injecting headers like `X-Forwarded-For` or `X-Real-IP`

---

## WSTG-BUSL-08 — Test Upload of Unexpected File Types

Uploading unexpected file types allows attackers to bypass business logic constraints by submitting files that the application does not intend to process, potentially leading to remote code execution, cross-site scripting, or denial of service. Attackers probe these vulnerabilities by modifying file extensions, MIME types, and magic bytes to trick the server-side validation logic into accepting malicious content. This typically occurs in profile picture uploads, document management systems, or support ticket attachments where insufficient validation exists on the back-end. Successful exploitation can compromise the host server if the uploaded file is executed or lead to client-side attacks if the file is served back to other users with an incorrect Content-Type.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### Does the application restrict file uploads to a specific set of extensions?
- [ ] Yes — a strict whitelist of extensions is enforced and bypass **not possible**  
- [ ] Yes — extension whitelist is present but bypass **is possible** via double extensions or null bytes  
- [ ] No — any file extension **can** be uploaded  

### Is the file content validated beyond the file extension?
- [ ] Yes — server-side logic verifies Magic Bytes and performs deep content inspection  
- [ ] Yes — server-side logic verifies the MIME type via the `Content-Type` header only  
- [ ] No — no content validation **is applied** after the extension check  

### Can an attacker execute code by uploading a web shell or script?
- [ ] No — uploaded files are stored in a non-executable directory or a storage bucket (S3/Azure Blob)  
- [ ] Yes — files are stored in a web-accessible directory but execution **is disabled** via server configuration  
- [ ] Yes — uploaded malicious scripts **can** be executed directly via a predictable URL path *(Critical)*  

### Are client-side attacks like XSS possible via uploaded file types?
- [ ] No — files are served with `Content-Disposition: attachment` and `X-Content-Type-Options: nosniff`  
- [ ] Yes — SVG or HTML files **can** be uploaded and rendered in the browser, leading to XSS  
- [ ] Yes — image metadata (EXIF) **is processed** and reflected in the UI without sanitization

---

## WSTG-BUSL-09 — Test Upload of Malicious Files

File upload functionality allows users to submit data to the server, providing a direct vector for introducing malicious content into the application's environment. Attackers exploit weak validation logic to upload web shells, malware, or specially crafted files—such as SVG or HTML—to achieve Remote Code Execution (RCE) or Cross-Site Scripting (XSS). This vulnerability is typically found in features such as profile settings, document management systems, and attachment handlers where the server fails to properly verify file types, contents, and execution permissions. Successful exploitation often leads to full system compromise, data exfiltration, or the use of the server as a pivot point for internal network attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### Does the application provide functionality to upload files?
- [ ] No — no file upload functionality exists  
- [ ] Yes — functionality exists for authenticated users  
- [ ] Yes — functionality is available to **unauthenticated** users *(Critical)*  

### Is file extension validation enforced on the server side?
- [ ] Yes — a strict allowlist of extensions is **applied** and bypass **not possible**  
- [ ] Yes — an allowlist is used but bypass **is possible** via case sensitivity or double extensions  
- [ ] Yes — a blocklist is used, which **can** be bypassed with alternative extensions (e.g., `.phtml`, `.asa`)  
- [ ] No — no extension validation is **applied** on the server side  

### Is the file content validated beyond the extension or MIME type?
- [ ] Yes — the application verifies magic bytes and performs deep content inspection  
- [ ] Yes — the application only checks the `Content-Type` header, which **is** easily spoofed  
- [ ] No — the application relies entirely on the file extension for validation  

### Are uploaded files stored in a directory with execution permissions?
- [ ] No — files are stored on a dedicated storage service (e.g., S3) or a non-executable directory  
- [ ] Yes — files are stored on the web server but execution is **disabled** via configuration  
- [ ] Yes — files are stored in a web-accessible directory and execution **is enabled** *(Critical)*  

### Can security filters be bypassed via filename manipulation?
- [ ] No — filenames are sanitized or randomly regenerated by the server  
- [ ] Yes — filters **can** be bypassed using null bytes (e.g., `shell.php%00.jpg`)  
- [ ] Yes — path traversal characters (e.g., `../../`) **can** be used to upload files to arbitrary directories

---

## WSTG-BUSL-10 — Test Payment Functionality

Testing payment functionality involves identifying business logic flaws that allow an attacker to bypass payment gateways, manipulate order totals, or spoof successful transaction signals. These vulnerabilities typically reside in the communication between the application, the client browser, and third-party payment processors such as Stripe, PayPal, or internal ledger systems. An attacker may attempt to modify price parameters in transit, submit negative quantities to reduce the total cost, or replay successful transaction callbacks to fulfill orders without actual payment. Successful exploitation leads to direct financial loss, inventory discrepancy, and potential compromise of the application's e-commerce integrity.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### Is the transaction amount validated on the server side prior to processing?
- [ ] Yes — amount is strictly validated against the server-side product database *(Most secure)*  
- [ ] Yes — amount is validated but logic bypass **is possible** via parameter pollution or currency switching  
- [ ] No — transaction amount **can** be modified in the client-side request and is honored by the backend  

### Can the quantity of items be manipulated to result in a zero or negative total?
- [ ] No — negative or zero quantities are rejected by the application validation logic  
- [ ] Yes — negative quantities are accepted in the cart but the total is corrected at checkout  
- [ ] Yes — negative quantities **can** be used to reduce the overall order total or obtain a credit *(Critical)*  

### Does the application securely handle payment gateway callbacks or webhooks?
- [ ] Yes — callbacks require a valid cryptographic signature that **cannot** be spoofed  
- [ ] Yes — signatures are checked but the secret is weak or the verification **can** be bypassed  
- [ ] No — application relies on unauthenticated or unverified callbacks to confirm payment status  

### Is the application vulnerable to race conditions during the checkout or payment phase?
- [ ] No — transactional integrity and database locking mechanisms are **enabled**  
- [ ] Yes — race conditions **are possible** but do not impact the final price or fulfillment  
- [ ] Yes — concurrent requests **can** lead to multiple fulfillments for a single payment or "double-spending" of gift cards  

### Are discount codes or loyalty points subject to manipulation or reuse?
- [ ] No — codes are validated for one-time use and logic constraints are **applied**  
- [ ] Yes — codes can be reused or applied to ineligible items, but impact is limited  
- [ ] Yes — attackers **can** apply multiple conflicting codes or bypass expiration and usage limits

---

## WSTG-CLNT-01 — Testing for DOM Based Cross Site Scripting

DOM-based Cross-Site Scripting (DOM XSS) occurs when an application contains client-side JavaScript that processes data from an untrusted source in an unsafe way, usually by writing the data back to the Document Object Model (DOM) via a dangerous sink. Unlike reflected or stored XSS, the vulnerability exists entirely within the client-side code; the payload is often not sent to the server at all, as it is processed by sinks like `innerHTML`, `eval()`, or `document.write()`. Attackers exploit this by crafting URLs with malicious fragments or parameters that, when loaded by a victim's browser, execute arbitrary JavaScript in the context of the user's session. This can lead to the exfiltration of session tokens, sensitive data theft, and unauthorized actions performed on behalf of the authenticated user.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Tools:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Are untrusted sources used to capture user-controlled data in JavaScript?
- [ ] No — JavaScript does **not** use `location.hash`, `location.search`, or `document.referrer`  
- [ ] Yes — untrusted sources are used but data is **not** passed to any execution sinks  
- [ ] Yes — untrusted sources are used and data flows directly into client-side logic  

### Is user-controlled data passed to dangerous DOM sinks?
- [ ] No — sinks such as `innerHTML`, `outerHTML`, `document.write()`, or `eval()` are **not** used  
- [ ] Yes — dangerous sinks are present but the data is strictly **sanitized** using a secure library like DOMPurify  
- [ ] Yes — dangerous sinks are present and data is processed with **insufficient** or custom regex-based filtering  
- [ ] Yes — dangerous sinks are present and data is passed **without** any sanitization or encoding *(Critical)*  

### Can the execution sink be reached via a URL-based payload?
- [ ] No — source-to-sink flow **cannot** be triggered via external input  
- [ ] Yes — flow is **possible** but requires complex user interaction  
- [ ] Yes — flow is **possible** and can be triggered via a simple URL fragment or query parameter  

### Are modern security headers or browser-based mitigations neutralizing the risk?
- [ ] Yes — a strict `Content-Security-Policy` (CSP) with `script-src` and `trusted-types` is **enabled** and prevents execution  
- [ ] Yes — a CSP is present but `unsafe-inline` or weak hashes make a bypass **possible**  
- [ ] No — no CSP is present to mitigate DOM-based execution  

### Does the application use client-side frameworks with built-in protections?
- [ ] Yes — framework (e.g., Angular, React) automatically encodes data and bypass is **not possible**  
- [ ] Yes — framework is used but "escape hatches" like `dangerouslySetInnerHTML` are **enabled**  
- [ ] No — the application uses vanilla JavaScript or legacy libraries (e.g., old jQuery versions) without auto-escaping

---

## WSTG-CLNT-02 — Testing for JavaScript Execution

JavaScript execution testing focuses on identifying instances where an application improperly handles user-supplied data, leading to the execution of unauthorized scripts within the victim's browser context. This vulnerability typically results in Cross-Site Scripting (XSS), which allows attackers to exfiltrate session cookies, manipulate the Document Object Model (DOM), or perform actions on behalf of the user. Penetration testers examine sinks such as `innerHTML`, `document.write()`, and `eval()` where unsanitized data from sources like URL fragments, `localStorage`, or `window.name` may be processed. Successful exploitation compromises the integrity and confidentiality of the user's session and can serve as a pivot for more complex client-side attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Test Status** | Not Performed |
| **Severity** | High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Tools:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### Does the application process user-supplied data in dangerous JavaScript sinks?
- [ ] No — all data is sanitized or used in safe sinks like `textContent`  
- [ ] Yes — data is processed in dangerous sinks but sanitization/encoding **is applied**  
- [ ] Yes — data is processed in dangerous sinks and sanitization **is not applied**  

### Is a Content Security Policy (CSP) present to mitigate script execution?
- [ ] Yes — a restrictive CSP is **enabled** and bypass **is not possible**  
- [ ] Yes — a CSP is **enabled** but bypass **is possible** via `unsafe-inline` or insecure CDNs  
- [ ] No — no CSP is **enabled**  

### Can JavaScript be executed via URL parameters or fragments (DOM-based XSS)?
- [ ] No — input is properly encoded and execution **is not possible**  
- [ ] Yes — input is reflected in the DOM but execution **is prevented** by modern framework protections  
- [ ] Yes — input is directly executed in the browser, and exploitation **is possible**  

### Are event handlers or URI schemes vulnerable to script injection?
- [ ] No — input is sanitized to remove event attributes and `javascript:` schemes  
- [ ] Yes — event handlers **can** be injected but filters limit payload complexity  
- [ ] Yes — arbitrary JavaScript execution via `onerror`, `onload`, or `href` attributes **is possible**

---

## WSTG-CLNT-03 — HTML Injection

HTML Injection occurs when an application fails to sanitize user-supplied input before rendering it within the browser, allowing an attacker to inject arbitrary HTML tags into the web page's Document Object Model (DOM). While similar to Cross-Site Scripting (XSS), the primary objective of HTML Injection is to manipulate the visual presentation of the page or to facilitate phishing attacks by injecting malicious forms and deceptive content. Attackers target parameters that are reflected in the UI, such as search terms, user profile fields, or error messages, to trick victims into disclosing sensitive credentials or interacting with unauthorized third-party links. This vulnerability represents a significant risk to the integrity of the application's user interface and can be a precursor to more complex social engineering or session-related attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Are user-controlled inputs reflected in the DOM without proper HTML encoding?
- [ ] No — all reflected input is strictly HTML-encoded before being rendered  
- [ ] Yes — some characters **are** encoded but bypasses for certain tags **are possible**  
- [ ] Yes — input is reflected raw, and HTML injection **is possible**  

### Can structural HTML elements be injected to alter the page layout?
- [ ] No — the application filters or strips structural tags like `<div>`, `<table>`, or `<iframe>`  
- [ ] Yes — structural elements **can** be injected but do **not** significantly impact the UI  
- [ ] Yes — significant UI defacement **is possible** via injected tags  

### Is it possible to inject functional phishing elements like forms or malicious links?
- [ ] No — `<form>`, `<input>`, and `<a>` tags are effectively blocked or sanitized  
- [ ] Yes — malicious links **can** be injected to redirect users to external sites  
- [ ] Yes — functional `<form>` elements **can** be injected to harvest credentials *(Medium)*  

### Do security headers or Content Security Policies (CSP) mitigate the impact of injection?
- [ ] Yes — a restrictive CSP is in place and `form-action` or `frame-src` **is applied**  
- [ ] No — security headers are present but the CSP **is disabled** or contains unsafe-inline/wildcards  
- [ ] No — no CSP or relevant security headers **are enabled**

---

## WSTG-CLNT-04 — Testing for Client-Side URL Redirect

Client-side URL redirection occurs when a web application accepts user-controlled input—typically through URL parameters or hash fragments—and uses it within a client-side redirection sink like `window.location` or `document.location.href`. This vulnerability is primarily leveraged by attackers to conduct sophisticated phishing campaigns, as the victim initially trusts the legitimate domain before being silently redirected to a malicious site. Beyond phishing, client-side redirects can often be chained to exfiltrate sensitive data such as OAuth access tokens or session identifiers if the redirection happens while those values are present in the URL or the `Referer` header. In modern Single Page Applications (SPAs), this vulnerability frequently appears in routing logic, "return to" parameters after authentication, or language-switching functionalities.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Test Status** | Not Performed |
| **Severity** | Medium* |

> *Severity becomes High if the redirect can be used to exfiltrate OAuth tokens, session IDs, or bypass security controls in an authentication flow.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### Does the application use client-side scripts to handle redirects based on user input?
- [ ] No — redirection is handled exclusively server-side via `3xx` HTTP status codes  
- [ ] Yes — application uses JavaScript sinks like `window.location` to navigate based on URL parameters  
- [ ] Yes — application uses a client-side framework router (e.g., React Router, Vue Router) that processes user-supplied paths  

### Are input validation or whitelisting controls implemented for the destination URL?
- [ ] Yes — a strict **whitelist** of allowed domains/paths is enforced and bypass **not possible**  
- [ ] Yes — validation is present but relies on weak regex or blacklists, and bypass **is possible**  
- [ ] No — application accepts any arbitrary string as a redirection target  

### Can the redirect logic be bypassed using common obfuscation techniques?
- [ ] No — controls correctly handle encoded characters, protocol-relative URLs (`//`), and backslashes  
- [ ] Yes — bypass **is possible** using URL encoding or double-encoding  
- [ ] Yes — bypass **is possible** using protocol-relative URLs or `@` characters (e.g., `https://expected.com@attacker.com`)  
- [ ] Yes — bypass **is possible** using specific browser quirks or null byte injection  

### Does the redirection process expose sensitive data to the destination site?
- [ ] No — no sensitive data is present in the URL or transmitted via headers during redirect  
- [ ] Yes — sensitive data (e.g., session tokens, API keys) **is leaked** via the `Referer` header to the external site  
- [ ] Yes — sensitive data present in the URL fragment (`#`) **is accessible** to the destination site's scripts  

### Is it possible to execute JavaScript via the redirect sink (DOM-based XSS)?
- [ ] No — sink only allows navigation to `http` or `https` protocols  
- [ ] Yes — the redirect sink allows `javascript:` or `data:` URIs, making DOM-based XSS **possible**

---

## WSTG-CLNT-05 — Testing for CSS Injection

CSS Injection occurs when an application allows user-supplied input to influence the Cascading Style Sheets (CSS) of a web page without proper sanitization or escaping. While often perceived as a cosmetic issue, attackers can leverage CSS injection to exfiltrate sensitive data, such as CSRF tokens or session IDs, by using attribute selectors and background-image properties to trigger external requests. This vulnerability typically occurs in endpoints where user preferences (e.g., custom themes, font colors) are reflected inside `<style>` blocks or inline `style` attributes. From an attacker's perspective, successful exploitation can lead to UI redressing, phishing through unauthorized layout modification, or stealthy data exfiltration in environments where strict Content Security Policies might otherwise block JavaScript-based attacks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if the injection point allows the exfiltration of sensitive attributes like CSRF tokens or if it can be used for UI redressing in sensitive workflows.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### Does the application reflect user-controlled input within CSS contexts?
- [ ] No — input is **not** reflected in style tags, attributes, or CSS files  
- [ ] Yes — input is reflected but strictly limited to safe alphanumeric values  
- [ ] Yes — input is reflected within a `style` attribute or `<style>` block  

### Are CSS-specific metacharacters and functions properly sanitized?
- [ ] Yes — characters such as `{`, `}`, `:`, and functions like `url()` are **properly escaped**  
- [ ] Yes — sanitization is in place but bypass **is possible** via encoding or CSS comments  
- [ ] No — no sanitization is applied to CSS metacharacters  

### Is data exfiltration possible using CSS selectors?
- [ ] No — selectors **cannot** trigger external requests or leak data  
- [ ] Yes — partial data exfiltration **is possible** via attribute selectors and `background-image`  
- [ ] Yes — full exfiltration of sensitive tokens **is possible** using automated brute-force techniques  

### Does a Content Security Policy (CSP) mitigate the risk of CSS injection?
- [ ] Yes — `style-src` is restricted to 'self' and `img-src` or `connect-src` **is restricted**  
- [ ] Yes — CSP is present but uses 'unsafe-inline' or allows wildcard external domains  
- [ ] No — no CSP is implemented to prevent external resource loading via CSS  

### Can the vulnerability be used to perform UI redressing or phishing?
- [ ] No — injection scope is too limited to modify the layout significantly  
- [ ] Yes — UI modification **is possible**, allowing for the overlay of malicious elements  
- [ ] Yes — full page layout control **is possible**, facilitating highly convincing phishing attacks

---

## WSTG-CLNT-06 — Testing for Client-Side Resource Manipulation

Client-side resource manipulation occurs when an application allows user-controlled input to influence the source or path of resources loaded by the browser, such as scripts, stylesheets, or iframes. By manipulating URI fragments, query parameters, or other DOM sources, an attacker can coerce the application into loading malicious external content or executing unauthorized code. This vulnerability is typically found in JavaScript logic that dynamically populates the `src` or `href` attributes of HTML elements without sufficient validation against a trusted allow-list. From an attacker's perspective, this is exploited by crafting a URL that redirects resource loading to an attacker-controlled server, potentially resulting in DOM-based Cross-Site Scripting (XSS), sensitive data exfiltration, or UI redressing.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### Does the application use client-side sinks to dynamically load or embed resources?
- [ ] No — resources are loaded via static HTML or server-side logic only  
- [ ] Yes — client-side JavaScript dynamically populates resource attributes like `src`, `href`, or `data`  

### Are validation controls or allow-lists implemented for resource URI sources?
- [ ] Yes — strict allow-lists and origin validation **are applied** to all dynamic resources  
- [ ] Yes — validation is present but relies on weak blacklists or easily bypassed regex  
- [ ] No — no validation controls are applied to user-controlled resource paths  

### Is it possible to bypass URI validation using alternative protocols or encoding?
- [ ] No — bypass of resource filters is **not possible**  
- [ ] Yes — bypass **is possible** using different protocols such as `data:`, `vbscript:`, or `javascript:`  
- [ ] Yes — bypass **is possible** using encoded characters or null bytes to circumvent simple string matches  

### Can an attacker redirect resource loading to an external, untrusted domain?
- [ ] No — resource paths are restricted to the same origin or a fixed subdirectory  
- [ ] Yes — the application **can** be forced to load scripts or styles from an arbitrary external domain  

### What is the maximum impact of the resource manipulation?
- [ ] Low — manipulation only affects non-executable elements like images or non-sensitive text  
- [ ] Medium — manipulation allows for CSS injection or significant UI redressing  
- [ ] High — manipulation leads to DOM-based XSS or execution of arbitrary JavaScript *(Critical)*

---

## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) is a browser-based mechanism that allows a server to explicitly permit access to its resources from different origins, effectively relaxing the Same-Origin Policy (SOP). Misconfigurations occur when an application overly trusts arbitrary origins, often by reflecting the `Origin` header in the `Access-Control-Allow-Origin` response header or by using poorly implemented regex whitelists. From an attacker's perspective, a permissive CORS policy can be exploited by hosting a malicious script on a controlled domain that makes authenticated requests to the vulnerable application, allowing the attacker to exfiltrate sensitive data, such as CSRF tokens or PII, directly from the response body. This vulnerability is most critical on API endpoints that process authenticated sessions and return non-public information.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Tools:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### Does the application implement CORS headers on authenticated endpoints?
- [ ] No — CORS is **not enabled**; browser defaults to strict Same-Origin Policy  
- [ ] Yes — CORS headers are present and restricted to a strict **whitelist**  
- [ ] Yes — CORS headers are present but configured **permissively**  

### How does the server handle the `Access-Control-Allow-Origin` (ACAO) header?
- [ ] ACAO is set to a specific, **trusted** static domain  
- [ ] ACAO is set to `*` (wildcard) and credentials **cannot** be sent  
- [ ] ACAO is set to `*` (wildcard) and credentials **can** be sent *(Note: Most browsers block this)*  
- [ ] ACAO **dynamically reflects** the value of the `Origin` header from the request  

### Is the `Access-Control-Allow-Credentials` (ACAC) header set to `true`?
- [ ] No — ACAC is **not present** or set to `false`  
- [ ] Yes — ACAC is set to `true`, but ACAO is **restricted** to trusted origins  
- [ ] Yes — ACAC is set to `true` and ACAO **reflects** arbitrary origins *(Critical)*  

### Can the origin whitelist be bypassed via subdomain or null origin manipulation?
- [ ] No — whitelist is strictly enforced and **cannot** be bypassed  
- [ ] Yes — whitelist accepts **any** subdomain of the target (e.g., `attacker.target.com`)  
- [ ] Yes — whitelist accepts the `null` origin via sandboxed iframes  
- [ ] Yes — whitelist is vulnerable to regex bypasses (e.g., `target.com.attacker.com` or `target.com-safe.com`)  

### Does the CORS configuration allow sensitive data exfiltration?
- [ ] No — exposed endpoints do **not** contain sensitive or session-specific data  
- [ ] Yes — authenticated endpoints return PII, credentials, or CSRF tokens that **can** be read by an unauthorized origin

---

## WSTG-CLNT-08 — Testing for Cross Site Flashing

Cross-Site Flashing (XSF) is a client-side vulnerability occurring when a Flash (SWF) file improperly handles user-controlled input, allowing an attacker to execute malicious ActionScript code or bridge into the browser's JavaScript environment. By manipulating parameters such as `FlashVars` or URL query strings passed to sinks like `ExternalInterface.call`, `getURL`, or `loadMovie`, an attacker can perform actions in the context of the vulnerable domain, including session hijacking and data exfiltration. This vulnerability is primarily found in legacy enterprise applications that still host SWF files, where insufficient input validation allows the Flash movie to be repurposed as a vector for Cross-Site Scripting (XSS) or to bypass Same-Origin Policy (SOP) via permissive cross-domain configurations.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Are legacy Adobe Flash (.swf) files hosted on the application?
- [ ] No — no Flash files are hosted or referenced within the application scope  
- [ ] Yes — SWF files are present but serve static content only  
- [ ] Yes — SWF files are present and accept dynamic parameters via `FlashVars` or URL strings  

### Is input passed to ActionScript sinks sanitized?
- [ ] Yes — all inputs are strictly validated and ActionScript sinks **cannot** be manipulated  
- [ ] Yes — validation is applied but bypass **is possible** via specific encoding techniques  
- [ ] No — input is passed directly to sensitive sinks like `ExternalInterface.call` or `getURL` *(Critical)*  

### Can the Flash file be used to execute arbitrary JavaScript (XSS)?
- [ ] No — `ExternalInterface` is **disabled** or the `allowScriptAccess` parameter is set to `never`  
- [ ] Yes — `allowScriptAccess` is set to `sameDomain` but the SWF is hosted on the target domain  
- [ ] Yes — `allowScriptAccess` is set to `always`, allowing XSS from any domain  

### Does the `crossdomain.xml` policy prevent unauthorized cross-origin requests?
- [ ] Yes — `crossdomain.xml` is restrictive and only allows trusted, specific origins  
- [ ] No — `crossdomain.xml` file **is missing**  
- [ ] Yes — policy is overly permissive (e.g., `<allow-access-from domain="*" />`)  

### Can the SWF file be forced to load external, attacker-controlled movies?
- [ ] No — the application **cannot** be forced to load external SWF files  
- [ ] Yes — the `loadMovie` or `loadMovieNum` functions accept unvalidated external URLs

---

## WSTG-CLNT-09 — Testing for Clickjacking

Clickjacking, also known as UI redressing, occurs when an attacker uses transparent or opaque layers to trick a user into clicking on a button or link on another page when they were intending to click on the top-level page. This vulnerability compromises the integrity of user actions, potentially leading to unauthorized data modification, account changes, or financial transfers performed on behalf of the victim. Attackers typically exploit this by embedding the target website within an `<iframe>` on a controlled malicious site and overlaying it with deceptive content or enticing lures. It occurs most frequently on pages performing sensitive state-changing actions such as "Delete Account" buttons, password reset forms, or administrative configuration panels.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if sensitive actions (e.g., fund transfers, account deletion, or privilege escalation) can be triggered via clickjacking.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Tools:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### Does the application implement server-side headers to prevent unauthorized framing?
- [ ] Yes — `X-Frame-Options` or `Content-Security-Policy` **are applied** and prevent framing *(Most secure)*  
- [ ] Yes — headers are present but **misconfigured** (e.g., `X-Frame-Options: ALLOW-FROM` which lacks broad browser support)  
- [ ] No — no framing protection headers **are applied**  

### Is the `Content-Security-Policy` (CSP) `frame-ancestors` directive implemented?
- [ ] Yes — `frame-ancestors 'none'` or `'self'` **is applied**  
- [ ] Yes — `frame-ancestors` is present but **allows** untrusted or overly broad origins  
- [ ] No — directive is **missing** or CSP is not implemented  

### Is the `X-Frame-Options` (XFO) header correctly configured for legacy browser support?
- [ ] Yes — `DENY` or `SAMEORIGIN` **is applied**  
- [ ] Yes — XFO is present but **ignored** by modern browsers because a CSP `frame-ancestors` directive exists  
- [ ] No — XFO header is **missing** or set to an insecure value  

### Can the framing protections be bypassed using known techniques?
- [ ] No — bypass **not possible** via standard techniques (double-framing, etc.)  
- [ ] Yes — protection **can** be bypassed due to weak regex or logic in the `frame-ancestors` white-list  
- [ ] Yes — protection **can** be bypassed because the application relies solely on JavaScript-based frame-busting  

### Is a sensitive action exploitable via a clickjacking proof-of-concept?
- [ ] No — framing is blocked or no sensitive actions are reachable  
- [ ] Yes — UI redressing **is possible** but requires complex user interaction  
- [ ] Yes — UI redressing **is possible** and allows for immediate state-changing actions *(Critical)*

---

## WSTG-CLNT-10 — Testing WebSockets

WebSocket testing focuses on the full-duplex communication channel established between the client and server, which bypasses traditional HTTP request-response patterns. Pentesters evaluate the security of the handshake process, the presence of Cross-Site WebSocket Hijacking (CSWSH) protections, and the rigor of input validation applied to message payloads. Exploitation typically involves hijacking an active session to exfiltrate data in real-time or injecting malicious payloads that trigger server-side vulnerabilities like Command Injection or SQLi through the persistent socket. Since many traditional Web Application Firewalls (WAFs) do not inspect WebSocket traffic effectively, this protocol often provides a stealthy vector for bypassing perimeter defenses.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Test Status** | Not Performed |
| **Severity** | High / Medium |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Tools:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### Is the WebSocket connection established over a secure channel?
- [ ] Yes — `wss://` (WebSocket Secure) is enforced for all connections  
- [ ] No — `ws://` is used, allowing for person-in-the-middle (PITM) eavesdropping  

### Is the application protected against Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] No — the server strictly validates the `Origin` header during the handshake  
- [ ] Yes — `Origin` header validation is weak or **can** be bypassed via null or spoofed origins  
- [ ] Yes — no `Origin` header validation is performed, allowing cross-site attackers to initiate a connection *(Critical)*  

### Is input validation and sanitization applied to WebSocket message payloads?
- [ ] Yes — all incoming messages are sanitized and validated against a schema  
- [ ] Yes — some validation exists but bypass **is possible** using encoded or non-standard payloads  
- [ ] No — messages are processed directly, allowing for injection (XSS, SQLi, RCE) or logic manipulation  

### Are authentication and authorization checks performed on every WebSocket message?
- [ ] Yes — the session is verified for every message or the protocol is stateless with tokens  
- [ ] Yes — authentication happens at the handshake, but authorization for specific actions **is not** enforced per message  
- [ ] No — authentication is only checked at the handshake, allowing session hijacking to grant full access  

### Does the server implement rate limiting or resource constraints on the WebSocket?
- [ ] Yes — rate limiting and maximum message sizes are **enabled** and enforced  
- [ ] No — no limits exist, allowing for Denial of Service (DoS) via message flooding or large payload sizes

---

## WSTG-CLNT-11 — Test Web Messaging

Web Messaging, or `postMessage`, enables cross-origin communication between window objects, such as a parent page and a spawned popup or an embedded iframe. This mechanism is critical for modern web functionality but introduces significant risk if the receiving application fails to verify the sender's identity or improperly handles the message payload. Attackers exploit these vulnerabilities by hosting a malicious site that embeds the target application and sends crafted messages to trigger unintended actions, exfiltrate sensitive data, or execute DOM-based Cross-Site Scripting (XSS). Successful exploitation occurs when message listeners do not strictly validate the `origin` property or pass `message.data` directly into dangerous execution sinks.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### Does the application utilize the `postMessage` API for cross-document communication?
- [ ] No — `postMessage` listeners are **not** present in the client-side code  
- [ ] Yes — `postMessage` is used for internal component communication  
- [ ] Yes — `postMessage` is used to communicate with third-party domains or widgets  

### Is the origin of incoming messages strictly validated?
- [ ] Yes — origin is checked against a strict whitelist using `===` or identical comparison *(Most secure)*  
- [ ] Yes — origin is validated using a regex, but the logic **is** susceptible to bypass (e.g., `^https://trusted.com`)  
- [ ] No — the `origin` property is **not** checked, allowing any domain to send messages  
- [ ] No — a wildcard `*` is used in the `targetOrigin` of the sender, potentially leaking data to malicious listeners  

### Is the message payload sanitized before being processed by the application?
- [ ] Yes — data is treated as plain text and no dangerous sinks are used  
- [ ] Yes — data is parsed (e.g., `JSON.parse`) and validated before use, and bypass **not possible**  
- [ ] No — message data is passed directly into dangerous sinks like `innerHTML`, `eval()`, or `setTimeout()`  
- [ ] No — message data is used to navigate the window or change the `location.href` without validation  

### Can an attacker trigger unauthorized actions or exfiltrate data via a malicious message?
- [ ] No — even with a spoofed message, no sensitive actions or data can be accessed  
- [ ] Yes — a crafted message **can** trigger sensitive state changes (e.g., password change, profile update)  
- [ ] Yes — a crafted message **can** result in DOM-based XSS or exfiltration of CSRF tokens/session data

---

## WSTG-CLNT-12 — Test Browser Storage

Testing browser storage involves analyzing how an application utilizes client-side mechanisms such as LocalStorage, SessionStorage, IndexedDB, and WebSQL to persist data. Insecurely stored sensitive information—including PII, authentication tokens, or session identifiers—can be compromised if an attacker achieves Cross-Site Scripting (XSS) or gains physical access to the user's device. Pentesters must determine if data is stored in cleartext, if it remains accessible after logout, and if the storage lifecycle is appropriately managed to prevent data leakage. From an attacker's perspective, these storage mechanisms are lucrative targets for exfiltrating state-maintaining information that allows for session hijacking or long-term account persistence.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High* |

> *Severity becomes High if session tokens or sensitive PII are stored in LocalStorage and are accessible via XSS.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### Does the application store sensitive information in browser storage?
- [ ] No — application does **not** use browser storage or only stores non-sensitive UI state  
- [ ] Yes — sensitive data is stored but **is encrypted** using robust client-side cryptography  
- [ ] Yes — sensitive data (tokens, PII, secrets) is stored in **cleartext** *(Medium)*  

### Is stored data accessible to unauthorized scripts (XSS risk)?
- [ ] No — data is stored in HttpOnly cookies (not browser storage) or storage is **not utilized**  
- [ ] Yes — LocalStorage/SessionStorage is used, making data **completely accessible** via any XSS payload *(High)*  

### Is browser storage appropriately cleared upon user logout?
- [ ] Yes — all application-related storage is **explicitly cleared** during the logout process  
- [ ] No — storage persists after logout, but does **not** contain sensitive session data  
- [ ] No — authentication tokens or PII **persist** in storage after the session is terminated *(Medium)*  

### Does the application use IndexedDB or WebSQL for large/sensitive datasets?
- [ ] No — these storage mechanisms are **not used**  
- [ ] Yes — data is stored and **properly sanitized** before being rendered in the DOM  
- [ ] Yes — sensitive datasets are stored in **cleartext** within IndexedDB/WebSQL structures  

### Can the integrity of the stored data be manipulated to affect application logic?
- [ ] No — application validates or signs data before processing it from storage  
- [ ] Yes — modifying storage values (e.g., user roles, flags) **is possible** and results in altered application behavior

---

## WSTG-CLNT-13 — Testing for Cross Site Script Inclusion

Cross-Site Script Inclusion (XSSI) occurs when a web application exports sensitive data within a dynamically generated JavaScript file or JSONP response, which an attacker-controlled site can then include via a `<script>` tag. Since script inclusion bypasses the Same-Origin Policy (SOP), an attacker can execute the script in their own context and override global objects or variables to exfiltrate the sensitive information. This vulnerability typically manifests in authenticated endpoints that return user-specific data like email addresses, API keys, or session metadata in script format. From an attacker's perspective, exploitation involves crafting a malicious page that references the vulnerable script and captures the resulting global variable changes or function calls.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Medium / High |

> *Severity becomes High if leaked data includes CSRF tokens, session identifiers, or highly sensitive PII.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Do any authenticated endpoints return dynamic JavaScript or JSONP containing sensitive information?
- [ ] No — all scripts are static and **cannot** contain user-specific data  
- [ ] Yes — dynamic scripts exist but **do not** contain sensitive information  
- [ ] Yes — dynamic scripts contain PII or tokens and **are** accessible across origins  

### Is the `X-Content-Type-Options: nosniff` header implemented on sensitive script responses?
- [ ] Yes — header is **applied** and prevents MIME-type sniffing  
- [ ] No — header is **missing**, potentially allowing non-script content to be executed as script  

### Are sensitive scripts protected by anti-XSSI mechanisms such as non-executable prefixes or custom headers?
- [ ] Yes — scripts require a custom header or token that **cannot** be sent via a standard `<script>` tag  
- [ ] Yes — scripts use non-executable prefixes (e.g., `)]}'`) that **cannot** be easily bypassed  
- [ ] No — scripts are directly accessible via GET requests using ambient credentials *(Critical)*  

### Is exfiltration of sensitive data possible by overriding global variables or prototypes?
- [ ] No — sensitive data is scoped locally or protected via modern browser features  
- [ ] Yes — sensitive data is assigned to global variables and **can** be captured  
- [ ] Yes — data is leaked through JSONP callbacks that **can** be intercepted

---

## WSTG-CLNT-14 — Testing for Reverse Tabnabbing

Reverse Tabnabbing is a client-side vulnerability where a page opened via `target="_blank"` retains a functional reference to the parent page through the `window.opener` object. This allows an attacker-controlled destination site to programmatically redirect the original, trusted tab to a malicious URL, typically a phishing page designed to harvest credentials. The attack exploits the user's inherent trust in the original tab, as they are unlikely to notice that the page they just left has navigated to a different domain. Modern security practices require the use of the `rel="noopener"` or `rel="noreferrer"` attributes on anchor tags, or setting the `opener` property to null in JavaScript, to prevent this cross-window communication.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Test Status** | Not Performed |
| **Severity** | Low / Medium* |

> *Severity is Low in most modern browsers as they now default `target="_blank"` to `rel="noopener"`. Severity becomes Medium if the application targets legacy browsers or uses `window.open()` without setting the `opener` property to null.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Are links present that open in a new window or tab?
- [ ] No — no links use `target="_blank"` or equivalent JavaScript-based redirection  
- [ ] Yes — `target="_blank"` is used exclusively on internal, trusted links  
- [ ] Yes — `target="_blank"` is used on external links or user-supplied URLs  

### Is the `window.opener` relationship restricted on anchor tags?
- [ ] Yes — `rel="noopener"` or `rel="noreferrer"` is **consistently applied** to all links with `target="_blank"` *(Most secure)*  
- [ ] Yes — attributes are applied but are **missing** on high-risk or user-generated links  
- [ ] No — attributes are **not applied**, allowing the child page to access `window.opener`  

### Is the application vulnerable to Reverse Tabnabbing via JavaScript `window.open()`?
- [ ] No — `window.open()` is not used or explicitly sets the `opener` property to null  
- [ ] Yes — `window.open()` is used and the `opener` reference **remains accessible** to the child window  

### Can an attacker control the destination of a link that opens in a new tab?
- [ ] No — all link destinations are hardcoded and point to trusted domains  
- [ ] Yes — link destinations are user-supplied (e.g., social media profiles, bio links) and **not** sanitized for tabnabbing protections

---

## WSTG-APIT-01 — API Reconnaissance

API reconnaissance is the systematic process of identifying and mapping the API attack surface to uncover endpoints, supported methods, and underlying data structures. Attackers perform this phase to discover undocumented (shadow) APIs, deprecated versions with legacy vulnerabilities, and exposed documentation files like Swagger or OpenAPI definitions that reveal internal business logic. By analyzing predictable URL patterns, client-side JavaScript files, and directory brute-force results, an adversary can build a comprehensive map of the API to identify targets for Broken Object Level Authorization (BOLA) or Mass Assignment attacks. This reconnaissance typically targets common paths such as `/api/v1/`, `/swagger.json`, or `/graphql` to gain a roadmap of the application's backend functionality.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | Informational / Low |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Tools:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Are API documentation files (Swagger, OpenAPI, WSDL) publicly accessible?
- [ ] No — API documentation is **not** accessible or is restricted by authentication  
- [ ] Yes — documentation is accessible but requires valid credentials  
- [ ] Yes — API documentation (e.g., `/swagger-ui.html`) is **publicly accessible** without authentication  

### Can undocumented or "shadow" API endpoints be discovered via fuzzing?
- [ ] No — directory and endpoint fuzzing yielded no undocumented paths  
- [ ] Yes — undocumented endpoints were found but they **cannot** be accessed without authorization  
- [ ] Yes — undocumented endpoints were found and **can** be accessed without valid authorization  

### Does the application reveal multiple API versions (e.g., /v1/, /v2/, /beta/)?
- [ ] No — only the current, hardened API version is accessible  
- [ ] Yes — legacy versions exist but implement the **same** security controls as the current version  
- [ ] Yes — legacy versions (e.g., `/v1/`) are accessible and **lack** the security controls of newer versions  

### Do client-side resources (JavaScript/Mobile Apps) leak API endpoint structures or keys?
- [ ] No — client-side code does **not** contain hardcoded API paths or sensitive keys  
- [ ] Yes — client-side code contains API endpoint maps but **no** sensitive keys  
- [ ] Yes — API keys, secrets, or internal-only endpoint URLs **are** hardcoded in client-side resources  

### Are hidden parameters or headers discoverable on known endpoints?
- [ ] No — parameter fuzzing (e.g., via Arjun) did **not** reveal hidden inputs  
- [ ] Yes — hidden parameters (e.g., `debug=true`, `admin=1`) were discovered but are **not** functional  
- [ ] Yes — hidden parameters were discovered and **can** be used to alter application behavior

---

## WSTG-APIT-02 — API Broken Object Level Authorization

Broken Object Level Authorization (BOLA), also known as Insecure Direct Object Reference (IDOR), occurs when an API fails to validate whether a user has the appropriate permissions to access or manipulate a specific resource identified by an ID. Attackers exploit this by systematically enumerating or guessing identifiers—such as numeric IDs or UUIDs—in request paths, query parameters, or JSON bodies to access data belonging to other users. This vulnerability is the most common and impactful issue in modern API security, potentially leading to mass data exfiltration, unauthorized modification, or complete account takeover. From an attacker's perspective, the goal is to identify endpoints that accept object identifiers and test if the server-side authorization logic is missing or can be bypassed by manipulating those IDs.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Tools:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Are object identifiers predictable or enumerable within API requests?
- [ ] No — identifiers are long, random, and cryptographically secure (e.g., UUIDv4)  
- [ ] Yes — identifiers are sequential integers (e.g., `101`, `102`)  
- [ ] Yes — identifiers follow a predictable pattern or are derived from public information  

### Does the API perform server-side validation of object ownership for every request?
- [ ] Yes — authorization checks are applied for every request and bypass **not possible**  
- [ ] Yes — checks are applied but bypass **is possible** via parameter pollution or method tunneling  
- [ ] No — application relies solely on the user being authenticated without checking specific resource ownership  

### Is it possible to access or modify another user's resource by changing the identifier?
- [ ] No — unauthorized access to other users' resources is **not possible**  
- [ ] Yes — unauthorized **read** access (Horizontal BOLA) **is possible**  
- [ ] Yes — unauthorized **modification** or **deletion** (Horizontal BOLA) **is possible**  

### Does the API allow accessing administrative or system-level objects by substituting IDs?
- [ ] No — administrative resources are protected by secondary authorization layers  
- [ ] Yes — accessing or modifying system-level resources (Vertical BOLA) **is possible**  

### Can the authorization check be bypassed by moving the identifier to a different part of the request?
- [ ] No — authorization logic is consistent regardless of parameter location  
- [ ] Yes — bypass **is possible** when the ID is moved from the URL path to the JSON body or headers  
- [ ] Yes — bypass **is possible** when multiple IDs are provided and the server processes the unauthorized one

---

## WSTG-APIT-99 — Testing GraphQL

GraphQL is a query language for APIs that allows clients to request specific data structures, but misconfigurations often lead to significant security risks including information disclosure and resource exhaustion. Vulnerabilities typically arise from enabled introspection, lack of query depth/complexity limits, and broken object-level authorization (BOLA) within the resolver functions. Attackers exploit these endpoints by using introspection to map the entire schema, leveraging circular fragments to trigger Denial of Service (DoS), or bypassing authorization by accessing unauthorized fields through nested queries. Testing focuses on the `/graphql`, `/v1/graphql`, or `/graphiql` endpoints to ensure that the implementation enforces strict access controls and resource management.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Test Status** | Not Performed |
| **Severity** | High / Critical* |

> *Severity becomes Critical if broken object-level authorization (BOLA) allows unauthorized access to PII or administrative mutations.

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Tools:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### Is the GraphQL introspection system enabled?
- [ ] No — introspection is **disabled** and the schema cannot be mapped  
- [ ] Yes — introspection is **enabled** but requires administrative authentication  
- [ ] Yes — introspection is **enabled** and publicly accessible *(High)*  

### Are resource limits (depth and complexity) enforced on queries?
- [ ] Yes — strict query depth and complexity limits are **applied** and enforced  
- [ ] Yes — limits are **applied** but can be bypassed using aliases or fragments  
- [ ] No — no limits are **applied**, allowing for recursive queries and DoS  

### Is Broken Object-Level Authorization (BOLA) present in resolvers?
- [ ] No — authorization is **applied** at the field and object level for every query  
- [ ] Yes — authorization is **applied** to some fields but sensitive objects are exposed  
- [ ] Yes — authorization is **not applied**, allowing access to any object via ID manipulation  

### Are GraphQL mutations properly protected against unauthorized use?
- [ ] No — mutations are **not** present in the schema  
- [ ] Yes — mutations require valid authentication and strict authorization checks  
- [ ] Yes — mutations are **possible** for unauthenticated users or lack authorization  

### Do error messages leak sensitive implementation or schema details?
- [ ] No — error messages are generic and do **not** reveal internal logic  
- [ ] Yes — error messages reveal underlying database types or suggestions via "Did you mean...?"  
- [ ] Yes — full stack traces and sensitive configuration data are **revealed** in responses