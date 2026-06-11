## WSTG-ATHN-06 — Pruebas de debilidad en la caché del navegador

La debilidad en la caché del navegador ocurre cuando la información sensible se almacena en la caché local del navegador y puede ser recuperada por un usuario no autorizado con acceso a la misma máquina física. Esta falta de encabezados de control de caché adecuados permite que datos potencialmente sensibles, como información personal, detalles de la cuenta o identificadores de sesión, persistan después de que el usuario haya cerrado la sesión o cerrado el navegador. Los atacantes o usuarios posteriores en terminales compartidos o públicos pueden explotar esto navegando a través del historial del navegador, usando el botón "Atrás" o inspeccionando los archivos de la caché local para exfiltrar respuestas almacenadas en caché. Garantizar la implementación de directivas estrictas como `Cache-Control: no-store` y `Pragma: no-cache` es fundamental para cualquier aplicación que maneje contenido autenticado, con el fin de asegurar que los datos sensibles nunca se escriban en el disco.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Baja / Media* |

> *La severidad pasa a ser Media si se accede a la aplicación frecuentemente desde quioscos compartidos/públicos o si contiene datos PII/PHI altamente sensibles.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Herramientas:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### ¿Están presentes los encabezados de control de caché adecuados en las páginas autenticadas o sensibles?
- [ ] Sí — `Cache-Control: no-store, no-cache` y `Pragma: no-cache` están **presentes** y **configurados correctamente**  
- [ ] Sí — algunos encabezados están presentes pero **falta** `no-store`, permitiendo el almacenamiento en caché en disco  
- [ ] No — no se **aplican** encabezados relacionados con la caché, dependiendo del comportamiento predeterminado del navegador  

### ¿Utiliza la aplicación la directiva `private` para los datos específicos del usuario?
- [ ] Sí — `Cache-Control: private` está **habilitado** para todo el contenido personalizado para evitar el almacenamiento en caché en proxies  
- [ ] No — el contenido está marcado como `public` o carece de la directiva `private`, lo que lo hace **almacenable en caché** por proxies intermedios  

### ¿Es accesible la información sensible a través del botón "Atrás" del navegador después de un cierre de sesión exitoso?
- [ ] No — el navegador solicita una nueva autenticación o la página **no se carga** desde la caché local  
- [ ] Sí — la información sensible **sigue siendo visible** a través del botón "Atrás" después de que la sesión ha finalizado  

### ¿Están protegidos contra el almacenamiento en caché los archivos no-HTML sensibles (por ejemplo, PDFs, exportaciones CSV, respuestas JSON de API)?
- [ ] No — la aplicación no genera ni maneja archivos no-HTML sensibles  
- [ ] Sí — se **aplican** encabezados estrictos de control de caché a todas las descargas de archivos sensibles y endpoints de API  
- [ ] No — las descargas sensibles o las respuestas de la API se **almacenan** en la caché local del navegador o en directorios temporales  

### ¿Utiliza la aplicación `Expires: 0` o una fecha en el pasado para evitar el uso de datos obsoletos?
- [ ] Sí — el encabezado `Expires` está configurado en `0` o en una fecha histórica, **asegurando** que el navegador trate el contenido como inmediatamente obsoleto  
- [ ] No — el encabezado `Expires` **falta** o está configurado en una fecha futura para páginas sensibles  

---