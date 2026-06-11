## WSTG-CONF-10 — Subdomain Takeover

El Subdomain Takeover (secuestro de subdominio) ocurre cuando una entrada DNS (típicamente un registro CNAME, pero ocasionalmente registros A o MX) apunta a un recurso retirado o inexistente en un proveedor de la nube de terceros o un servicio de alojamiento. Los atacantes identifican estos registros DNS "dangling" (huérfanos) buscando mensajes de error específicos de proveedores como AWS, GitHub o Azure que indican que un recurso ya no está reclamado. Al registrar el nombre del recurso abandonado en la plataforma del proveedor, el atacante obtiene el control total sobre el subdominio, lo que le permite alojar contenido malicioso, exfiltrar cookies de sesión con ámbito (scope) en el dominio base y omitir las protecciones de Content Security Policy (CSP) o CORS. Esta vulnerabilidad es particularmente peligrosa porque aprovecha la confianza asociada con la estructura de dominio legítima de la organización para facilitar ataques de phishing y robo de datos de alto impacto.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Alta / Crítica* |

> *La severidad se vuelve Crítica si el subdominio puede utilizarse para secuestrar cookies de sesión del dominio base o eludir redireccionamientos de SSO/OAuth.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Herramientas:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### ¿Utiliza la aplicación servicios de alojamiento de terceros o servicios en la nube a través de subdominios?
- [ ] No — todos los subdominios apuntan a un espacio de direcciones IP controlado por la organización  
- [ ] Sí — los subdominios apuntan a proveedores externos (S3, GitHub Pages, Heroku, etc.)  

### ¿Existen registros DNS "dangling" que apunten a recursos inexistentes?
- [ ] No — todos los registros DNS identificados resuelven a recursos activos y controlados  
- [ ] Sí — existen registros CNAME o A para recursos que **ya no existen** en el proveedor  
- [ ] Sí — los registros DNS resuelven a NXDOMAIN o a páginas de error específicas del proveedor *(ej., "NoSuchBucket")*  

### ¿Se puede reclamar con éxito el recurso "dangling" identificado?
- [ ] No — el proveedor tiene protecciones contra la reclamación de nombres abandonados o se requiere verificación de cuenta  
- [ ] Sí — el nombre del recurso **puede** ser registrado en la plataforma del tercero por cualquier usuario  

### ¿Utiliza el dominio base cookies con ámbito de "Domain" que podrían verse comprometidas?
- [ ] No — las cookies están estrictamente limitadas a subdominios específicos o utilizan flags `Host-Only`  
- [ ] Sí — las cookies sensibles (IDs de sesión, JWTs) tienen como ámbito el dominio principal y **pueden** ser exfiltradas a través de un subdominio secuestrado  

### ¿Puede utilizarse el subdominio para omitir cabeceras de seguridad o flujos de autenticación?
- [ ] No — no hay dependencias de seguridad en los subdominios identificados  
- [ ] Sí — el subdominio está en la lista blanca (whitelist) en las directivas `script-src` o `connect-src` de CSP  
- [ ] Sí — el subdominio es una URI de redireccionamiento válida para configuraciones de OAuth o SSO  

---