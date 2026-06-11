## WSTG-CONF-02 — Test Application Platform Configuration

Las pruebas de configuración de la plataforma de la aplicación implican la auditoría del servidor web subyacente, el servidor de aplicaciones y los ajustes del framework para asegurar que estén robustecidos (hardened) contra exploits comunes. Los atacantes buscan activamente credenciales por defecto (default credentials), vulnerabilidades de la plataforma sin parches e interfaces administrativas expuestas para obtener acceso no autorizado o ejecutar código. Las configuraciones erróneas a menudo resultan en la divulgación de variables de entorno sensibles, rutas internas del sistema o la presencia de aplicaciones de ejemplo innecesarias que expanden la superficie de ataque (attack surface). Desde una perspectiva profesional, una plataforma mal configurada sirve como un punto de entrada de alta probabilidad para lograr el acceso inicial al entorno objetivo.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si las interfaces administrativas son accesibles con credenciales por defecto o si los scripts de ejemplo permiten la ejecución remota de código (Remote Code Execution).

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### ¿Están activas las credenciales o cuentas por defecto en la plataforma?
- [ ] No — todas las cuentas por defecto están **deshabilitadas** o se han cambiado las contraseñas  
- [ ] Sí — existen cuentas por defecto pero **no son accesibles** desde la red externa  
- [ ] Sí — las cuentas por defecto están **activas** y son accesibles a través de internet *(Crítico)*  

### ¿Está la plataforma divulgando información sensible de la versión a través de cabeceras o páginas de error?
- [ ] No — las cadenas de versión están **ocultas** o son genéricas  
- [ ] Sí — la información detallada de la versión **se divulga** en las cabeceras HTTP (p. ej., `Server`, `X-Powered-By`)  
- [ ] Sí — se exponen trazas de la pila (stack traces) completas y rutas internas del sistema en mensajes de error detallados  

### ¿Están las interfaces administrativas o de gestión debidamente restringidas?
- [ ] No — las interfaces administrativas **no están expuestas**  
- [ ] Sí — existen interfaces pero están **restringidas** mediante listas de permitidos (allowlisting) de IP o autenticación de múltiples factores (Multi-Factor Authentication)  
- [ ] Sí — las interfaces (p. ej., `/admin`, `/manager`, `/console`) son **públicamente accesibles** con autenticación de un solo factor  

### ¿Existen archivos de ejemplo, scripts o documentación en el servidor de producción?
- [ ] No — todos los archivos no esenciales han sido **eliminados**  
- [ ] Sí — los archivos de ejemplo o la documentación **están presentes** pero no filtran información sensible  
- [ ] Sí — los scripts de ejemplo (p. ej., `info.php`, `examples/`) **están presentes** y proporcionan un vector de ataque directo  

### ¿Está el servidor configurado para admitir métodos HTTP inseguros?
- [ ] No — solo `GET` y `POST` están **habilitados**  
- [ ] Sí — métodos potencialmente arriesgados como `PUT`, `DELETE` o `TRACE` están **habilitados** pero restringidos  
- [ ] Sí — los métodos arriesgados están **habilitados** y permiten la modificación no autorizada de archivos o el robo de credenciales  

---