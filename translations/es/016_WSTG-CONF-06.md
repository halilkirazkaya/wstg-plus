## WSTG-CONF-06 — Probar métodos HTTP

La prueba de métodos HTTP consiste en identificar qué verbos son compatibles con el servidor web y la aplicación para asegurar que solo se exponga la funcionalidad necesaria. Más allá de los estándares `GET` y `POST`, los servidores pueden habilitar inadvertidamente métodos peligrosos como `PUT`, `DELETE` o `TRACE`, los cuales pueden permitir a los atacantes cargar archivos maliciosos, eliminar contenido existente o exfiltrar cookies de sesión mediante Cross-Site Tracing (XST). Los pentesters examinan los encabezados de respuesta como `Allow` o `Public` e intentan evadir las restricciones utilizando técnicas de method overriding o verb tampering para acceder a funcionalidades no autorizadas. Esta prueba es fundamental para fortalecer la superficie de ataque y prevenir configuraciones erróneas que podrían conducir al compromiso del servidor o a la manipulación de datos no autorizada.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Baja / Media* |

> *La severidad se considera Alta si `PUT` o `DELETE` permiten la manipulación de archivos no autorizada o si `TRACE` facilita la exfiltración de tokens de sesión.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### ¿Están deshabilitados los métodos HTTP peligrosos como `PUT` o `DELETE` en toda la aplicación?
- [ ] Sí — solo los métodos seguros (GET, POST, HEAD) **están habilitados**  
- [ ] No — `PUT` o `DELETE` **están habilitados** pero requieren una autenticación válida  
- [ ] No — `PUT` o `DELETE` **están habilitados** y son accesibles sin autenticación *(Crítico)*  

### ¿Está deshabilitado el método `TRACE` para prevenir Cross-Site Tracing (XST)?
- [ ] Sí — el método `TRACE` **está deshabilitado** o devuelve un 405 Method Not Allowed  
- [ ] No — el método `TRACE` **está habilitado** pero no refleja encabezados sensibles  
- [ ] No — el método `TRACE` **está habilitado** y refleja encabezados `Cookie` o `Authorization` *(Media)*  

### ¿El servidor admite el HTTP Method Overriding (p. ej., `X-HTTP-Method-Override`)?
- [ ] No — el servidor **no** procesa encabezados de invalidación de método (method override)  
- [ ] Sí — el servidor procesa encabezados de invalidación pero **se aplican** los controles de acceso  
- [ ] Sí — el servidor procesa encabezados de invalidación y **es posible** evadir el control de acceso  

### ¿Responde el servidor de forma segura ante métodos HTTP arbitrarios o malformados?
- [ ] Sí — el servidor devuelve un 405 Method Not Allowed o un 501 Not Implemented  
- [ ] No — el servidor devuelve un 200 OK o un Error 500 para métodos arbitrarios como `JEFF` o `TEST`  
- [ ] No — el uso de métodos arbitrarios permite evadir filtros de autenticación o autorización  

### ¿Está el método `OPTIONS` restringido o configurado para minimizar la divulgación de información?
- [ ] Sí — `OPTIONS` está deshabilitado o devuelve un encabezado `Allow` mínimo  
- [ ] No — `OPTIONS` revela una amplia gama de métodos habilitados, incluyendo DAV o verbos de depuración (debugging)  

---