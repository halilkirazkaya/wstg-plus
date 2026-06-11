## WSTG-INPV-01 — Cross Site Scripting (XSS) Reflejado

El Cross Site Scripting (XSS) Reflejado ocurre cuando una aplicación incluye datos no confiables en una respuesta HTTP sin una validación o codificación suficiente, lo que provoca que el Payload se ejecute en el contexto del navegador de la víctima. Los atacantes distribuyen payloads maliciosos a través de ingeniería social, típicamente mediante URLs o formularios manipulados, para secuestrar sesiones de usuario, exfiltrar cookies sensibles o realizar acciones no autorizadas en nombre del usuario. Esta vulnerabilidad se encuentra comúnmente en parámetros de búsqueda, mensajes de error y cualquier endpoint que refleje la entrada directamente de vuelta a la interfaz de usuario. La explotación depende de que la víctima interactúe con un enlace malicioso, lo que lo convierte en un vector de ataque no persistente pero altamente efectivo para dirigirse a usuarios administrativos o autenticados específicos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Estado de la Prueba** | No realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### ¿Se reflejan las entradas proporcionadas por el usuario en el cuerpo de la respuesta?
- [ ] No — la entrada **nunca** se refleja de vuelta al usuario  
- [ ] Sí — la entrada se refleja pero está codificada correctamente y el bypass **no es posible**  
- [ ] Sí — la entrada se refleja **sin** ninguna codificación o sanitización  

### ¿Se ha implementado la codificación de salida sensible al contexto?
- [ ] Sí — se **aplica** la codificación adecuada (HTML, Atributo, JavaScript) basada en el contexto específico de la reflexión  
- [ ] Sí — se **aplica** codificación pero es insuficiente para el contexto específico (por ejemplo, codificación HTML dentro de una etiqueta `<script>`)  
- [ ] No — **no se aplica** codificación  

### ¿Se pueden omitir (bypass) la validación de entrada o los filtros del Web Application Firewall (WAF)?
- [ ] No — los filtros bloquean eficazmente todos los payloads de XSS comunes y ofuscados  
- [ ] Sí — existen filtros pero el bypass **es posible** utilizando codificación de caracteres, etiquetas no estándar o polyglots  
- [ ] No — no hay filtros ni mecanismos de validación implementados  

### ¿Cuál es el contexto de ejecución de la entrada reflejada?
- [ ] Seguro — la entrada se refleja en una ubicación no ejecutable (por ejemplo, dentro de etiquetas estándar `<div>` o `<span>`)  
- [ ] Riesgoso — la entrada se refleja dentro de atributos HTML o dentro del `src`/`href` de las etiquetas  
- [ ] Crítico — la entrada se refleja directamente dentro de bloques `<script>`, manejadores de eventos o literales de plantilla (template literals)  

### ¿Existen cabeceras de seguridad de navegador modernas para mitigar el impacto de XSS?
- [ ] Sí — `Content-Security-Policy` (CSP) está **habilitado** con un script-src restrictivo  
- [ ] Sí — `Content-Security-Policy` (CSP) está **habilitado** pero contiene `unsafe-inline` o listas blancas (whitelists) débiles  
- [ ] No — no hay cabeceras CSP ni las legadas `X-XSS-Protection` presentes  

---