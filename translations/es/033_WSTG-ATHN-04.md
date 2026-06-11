## WSTG-ATHN-04 — Testing for Bypassing Authentication Schema

La evasión del esquema de autenticación (Bypassing the authentication schema) implica identificar y explotar fallos en la lógica de la aplicación que permiten a un atacante obtener acceso a recursos protegidos sin proporcionar credenciales válidas. Esta vulnerabilidad ocurre típicamente cuando la aplicación depende de controles del lado del cliente, falla al aplicar la autorización del lado del servidor en cada solicitud, o deja "backdoors" administrativos y endpoints de depuración (debug) expuestos en producción. Los atacantes explotan estas debilidades realizando una navegación forzada (forced browsing) hacia URLs sensibles, manipulando parámetros HTTP como `authenticated=true`, o realizando spoofing de cabeceras para engañar a la aplicación y que esta asuma que ya se ha establecido una sesión. Una explotación exitosa resulta en una ruptura completa del perímetro de autenticación, lo que puede conducir a una exfiltración de datos no autorizada o al compromiso total del sistema.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Herramientas:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### ¿Se puede acceder directamente a páginas internas sensibles sin una sesión activa?
- [ ] No — el acceso directo resulta en una redirección al login o una respuesta 403/404  
- [ ] Sí — algunas páginas sensibles son accesibles pero proporcionan datos limitados o no sensibles  
- [ ] Sí — las páginas administrativas o específicas de usuario sensibles **son** totalmente accesibles sin autenticación *(Crítica)*  

### ¿Depende la aplicación de flags o parámetros del lado del cliente para determinar el estado de autenticación?
- [ ] No — el estado de autenticación se gestiona estrictamente a través del estado de la sesión en el servidor  
- [ ] Sí — existen flags del lado del cliente pero su modificación **no** otorga acceso a áreas protegidas  
- [ ] Sí — la modificación de parámetros (ej. `is_authenticated=true`, `user_role=admin`) **permite** la evasión de la autenticación  

### ¿Se puede evadir la autenticación mediante el spoofing o la manipulación de cabeceras HTTP específicas?
- [ ] No — las cabeceras como `X-Forwarded-For`, `Referer` o cabeceras personalizadas no tienen impacto en la autenticación  
- [ ] Sí — la manipulación de cabeceras (ej. `X-Custom-IP-Authorization`, `X-Remote-User`) **es posible** y otorga acceso no autorizado  

### ¿Existen rutas alternativas o artefactos de desarrollo que eludan el flujo de inicio de sesión estándar?
- [ ] No — todos los recursos protegidos están restringidos por una comprobación de autenticación centralizada y lista para producción  
- [ ] Sí — los endpoints heredados, rutas de depuración (ej. `/debug/login`) o versiones de la API olvidadas **son** accesibles sin credenciales  

### ¿Falla la aplicación al re-autenticar o verificar sesiones para acciones de alto privilegio?
- [ ] No — las acciones de alto privilegio o sensibles requieren un token de sesión fresco o válido  
- [ ] Sí — una vez que se encuentra una evasión en un endpoint, esta **se puede** utilizar para realizar acciones administrativas en toda la aplicación  

---