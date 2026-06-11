## WSTG-CONF-07 — Pruebas de HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) es un mecanismo de seguridad que permite a un servidor web informar a los navegadores que solo se debe acceder a él a través de HTTPS, previniendo ataques de degradación de protocolo (protocol downgrade) y el secuestro de cookies (cookie hijacking). Al forzar una conexión cifrada, HSTS mitiga ataques Man-in-the-Middle (MitM) como SSLStrip, donde un atacante intercepta una solicitud HTTP inicial no segura para evitar la transición a TLS. Desde la perspectiva de un atacante, la ausencia de este encabezado o una duración corta de `max-age` proporciona una ventana de oportunidad para interceptar tokens de sesión sensibles o credenciales durante el Handshake inicial en texto claro. Esta prueba se centra en verificar la presencia, duración y alcance del encabezado `Strict-Transport-Security` en todos los endpoints sensibles de la aplicación.

| Campo | Valor |
|---|---|
| **ID WSTG** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Baja / Media* |

> *La severidad aumenta a Media si la aplicación maneja PII, tokens de sesión o credenciales, ya que la falta de HSTS incrementa la tasa de éxito de los ataques MitM.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### ¿Está presente el encabezado `Strict-Transport-Security` en las respuestas HTTPS?
- [ ] Sí — el encabezado **está presente** en todos los endpoints sensibles  
- [ ] Sí — el encabezado **está presente** pero solo en la página de inicio (landing page)  
- [ ] No — el encabezado **no se encuentra** en la aplicación en absoluto  

### ¿Está la directiva `max-age` configurada con una duración suficiente?
- [ ] Sí — `max-age` está configurado para un año o más (ej. `31536000`)  
- [ ] Sí — `max-age` está configurado pero es **demasiado corto** (ej. menos de 6 meses)  
- [ ] No — la directiva `max-age` **no está presente** o está configurada en `0` *(Crítico)*  

### ¿Cubre la política HSTS todos los subdominios?
- [ ] Sí — la directiva `includeSubDomains` **está aplicada**  
- [ ] No — la directiva `includeSubDomains` **no está presente**, dejando a los subdominios vulnerables a la degradación de protocolo  
- [ ] No — la característica no aplica ya que no existen subdominios  

### ¿Está la aplicación registrada para HSTS Preloading?
- [ ] Sí — la directiva `preload` **está presente** y el dominio está en la lista de HSTS preload  
- [ ] Sí — la directiva `preload` **está presente** pero el dominio **aún no está** en la lista de preload  
- [ ] No — la directiva `preload` **no está presente**, permitiendo una ventana para MitM en la primera visita  

### ¿Se envía el encabezado HSTS incorrectamente a través de HTTP en texto claro?
- [ ] No — el encabezado **solo** se envía a través de conexiones seguras HTTPS  
- [ ] Sí — el encabezado **se envía** a través de HTTP, lo cual es ignorado por los navegadores y puede filtrar detalles de configuración  

---