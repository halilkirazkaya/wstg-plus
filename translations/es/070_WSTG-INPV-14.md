## WSTG-INPV-14 — Pruebas de Vulnerabilidades Incubadas

Las vulnerabilidades incubadas, también conocidas como vulnerabilidades persistentes o de segundo orden (second-order vulnerabilities), ocurren cuando la aplicación almacena un payload malicioso en un estado "frío" (cold state) y luego este es ejecutado o procesado en un contexto diferente. Este patrón de ataque generalmente se dirige a sistemas back-end, consolas administrativas o herramientas de reportes automatizados que recuperan datos de una base de datos o sistema de archivos compartidos sin una re-validación adecuada o codificación de salida (output encoding). Los Pentesters deben rastrear el ciclo de vida de la entrada del usuario desde el punto de entrada (el "incubador") hasta cada ubicación donde esos datos son eventualmente renderizados o utilizados por otros componentes o aplicaciones secundarias. Una explotación exitosa a menudo conduce a resultados de alto impacto, como el secuestro de sesiones administrativas (session hijacking) a través de Stored XSS o la exfiltración de datos mediante SQL Injection de segundo orden en módulos de reportes internos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Herramientas:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### ¿Existen endpoints donde se almacena la entrada controlada por el usuario para su posterior recuperación por otros usuarios o procesos?
- [ ] No — la aplicación **no** almacena la entrada del usuario para su uso posterior  
- [ ] Sí — la entrada **se** almacena en campos de la base de datos, archivos de registro (logs) o ajustes de configuración  

### ¿Se aplica validación o sanitización de la entrada antes de que los datos se escriban en la capa de almacenamiento persistente?
- [ ] Sí — se **aplica** una validación estricta de lista blanca (allow-list) en el punto de entrada  
- [ ] Sí — se **aplica** una sanitización genérica, pero el bypass **es posible** a través de codificación o caracteres no estándar  
- [ ] No — la entrada se almacena en su forma original, sin validar (raw)  

### ¿Los datos almacenados se codifican o sanitizan adecuadamente cuando se renderizan en interfaces administrativas o secundarias?
- [ ] Sí — se **aplica** codificación de salida sensible al contexto (context-aware output encoding) en todos los lugares donde se renderizan los datos  
- [ ] Sí — la codificación **se aplica** en algunas ubicaciones pero **falta** en otras (p. ej., panel de administración interno)  
- [ ] No — los datos se renderizan sin ninguna codificación o sanitización, permitiendo la ejecución del payload  

### ¿Pueden los payloads almacenados en la base de datos afectar los procesos por lotes (batch) del back-end o los sistemas de monitoreo fuera de banda (out-of-band)?
- [ ] No — los procesos de back-end utilizan métodos de análisis (parsing) seguros o consultas parametrizadas (parameterized queries)  
- [ ] Sí — los payloads almacenados **pueden** desencadenar interacciones en la lógica del back-end (p. ej., CSV Injection, Command Injection en logs)  
- [ ] Sí — los payloads almacenados **pueden** desencadenar solicitudes fuera de banda (OOB) hacia servidores controlados por el atacante  

---