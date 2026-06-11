## WSTG-INPV-19 — Pruebas de Server-Side Request Forgery (SSRF)

La vulnerabilidad Server-Side Request Forgery (SSRF) ocurre cuando un atacante puede influir en una aplicación web para que realice solicitudes no autorizadas desde el servidor hacia recursos internos o externos. Al manipular parámetros que esperan URLs, hostnames o direcciones IP, un atacante puede evadir controles a nivel de red como firewalls y ACLs para sondear segmentos de red internos, acceder a servicios de metadatos en la nube (IMDS) o interactuar con APIs y bases de datos internas vulnerables. Esta vulnerabilidad se manifiesta típicamente en funcionalidades que involucran la obtención de imágenes, generación de PDF, webhooks o carga de archivos mediante URL. Desde la perspectiva de un atacante, el SSRF es una primitiva potente utilizada para pivotar hacia entornos aislados, exfiltrar datos de configuración sensibles o, potencialmente, lograr la ejecución remota de comandos mediante la interacción con servicios internos como Redis o Memcached.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Herramientas:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### ¿Procesa la aplicación URLs o hostnames proporcionados por el usuario?
- [ ] No — ninguna funcionalidad acepta URLs o hostnames externos  
- [ ] Sí — la funcionalidad existe pero la entrada **se valida estrictamente** contra una lista de permitidos (allow-list)  
- [ ] Sí — la funcionalidad existe y acepta URLs proporcionadas por el usuario  

### ¿Se aplican controles de validación o listas de denegación (deny-lists) al destino solicitado?
- [ ] Sí — se aplica una **allow-list** estricta de dominios/IPs de confianza  
- [ ] Sí — se utiliza una **deny-list** (p. ej., bloqueando `127.0.0.1`, `169.254.169.254`)  
- [ ] No — **no se aplica validación** ni filtrado a la entrada  

### ¿Es posible evadir los filtros mediante encoding, redirecciones o DNS rebinding?
- [ ] No — los filtros son robustos y manejan las redirecciones/resolución DNS de forma segura  
- [ ] Sí — la evasión **es posible** utilizando URL encoding o formatos de IP alternativos (hexadecimal, octal)  
- [ ] Sí — la evasión **es posible** mediante redirecciones HTTP 3xx hacia objetivos internos  
- [ ] Sí — la evasión **es posible** mediante ataques de DNS rebinding para resolver hacia IPs internas  

### ¿Puede la aplicación alcanzar servicios de metadatos internos o interfaces locales?
- [ ] No — la red local y los endpoints de metadatos en la nube **no son alcanzables**  
- [ ] Sí — el acceso a localhost (`127.0.0.1`) o servicios de loopback **es posible**  
- [ ] Sí — el acceso a servicios de metadatos en la nube (p. ej., AWS/GCP/Azure IMDS) **es posible** *(Crítico)*  

### ¿Cuál es la naturaleza de la respuesta de SSRF?
- [ ] Blind — no se devuelve ninguna respuesta ni metadatos al usuario  
- [ ] Parcial — se devuelven metadatos de la respuesta (cabeceras, tamaño, tiempos) al usuario  
- [ ] Full — el cuerpo completo de la respuesta del recurso interno **se refleja** al usuario  

---