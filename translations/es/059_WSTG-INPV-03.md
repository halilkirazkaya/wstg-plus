## WSTG-INPV-03 — Pruebas de HTTP Verb Tampering

El HTTP Verb Tampering explota debilidades en la forma en que los servidores web y los frameworks de aplicaciones autorizan el acceso a recursos específicos basándose en el método HTTP utilizado. Los atacantes intentan omitir las restricciones de seguridad sustituyendo métodos estándar como `GET` o `POST` por alternativas como `HEAD`, `PUT`, `OPTIONS`, o incluso cadenas arbitrarias no estándar que el backend podría procesar de manera inconsistente. Esta vulnerabilidad ocurre típicamente cuando las configuraciones de seguridad —como los filtros web.xml de Java EE o las reglas de autorización de .NET— enumeran explícitamente los métodos permitidos pero no tienen en cuenta otros, o cuando se utiliza un enfoque de "denegación por método" (deny-by-method) en lugar de "denegar todo" (deny-all). Una explotación exitosa puede resultar en acceso no autorizado a funciones administrativas, modificación de datos o divulgación de información sobre la configuración interna del servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Estado de la Prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si se pueden realizar funciones administrativas o modificación de datos (PUT/DELETE) sin autenticación.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### ¿Responde la aplicación a métodos HTTP no estándar o arbitrarios?
- [ ] No — la aplicación rechaza todos los métodos excepto aquellos explícitamente requeridos  
- [ ] Sí — la aplicación responde a métodos estándar (OPTIONS, TRACE) pero **no puede** omitir la seguridad  
- [ ] Sí — la aplicación acepta cadenas arbitrarias (ej. FOO, TEST) y las procesa como solicitudes `GET` o `POST`  

### ¿Es posible omitir la autenticación/autorización cambiando el método HTTP?
- [ ] No — los controles de seguridad se aplican globalmente independientemente del verbo HTTP  
- [ ] Sí — existen controles pero la omisión **es posible** utilizando el método `HEAD`  
- [ ] Sí — los controles de seguridad **no se aplican** a métodos alternativos, permitiendo el acceso no autorizado a endpoints restringidos  

### ¿Están habilitados en el servidor métodos peligrosos como `PUT` o `DELETE`?
- [ ] No — los métodos peligrosos están **deshabilitados** o devuelven un 405 Method Not Allowed  
- [ ] Sí — los métodos están habilitados pero requieren una autenticación válida de alto privilegio  
- [ ] Sí — los métodos están **habilitados** y permiten la subida de archivos o eliminación de recursos no autorizada *(Crítico)*  

### ¿Revela el método `OPTIONS` información sensible sobre los verbos permitidos?
- [ ] No — `OPTIONS` está **deshabilitado** o devuelve una respuesta genérica  
- [ ] Sí — `OPTIONS` devuelve el encabezado `Allow` pero no expone métodos internos restringidos  
- [ ] Sí — `OPTIONS` revela métodos de uso interno o administrativos que ayudan a una mayor explotación  

### ¿Está habilitado el método `TRACE`, permitiendo potencialmente Cross-Site Tracing (XST)?
- [ ] No — los métodos `TRACE` y `TRACK` están **deshabilitados**  
- [ ] Sí — `TRACE` está **habilitado** y refleja los encabezados HTTP, incluyendo cookies o tokens sensibles  

---