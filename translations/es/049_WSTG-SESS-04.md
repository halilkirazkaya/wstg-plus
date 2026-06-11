## WSTG-SESS-04 — Testing for Exposed Session Variables

Las variables de sesión expuestas ocurren cuando identificadores de sesión sensibles o datos relacionados con el estado se transmiten a través de canales inseguros como cadenas de consulta URL (URL query strings), encabezados Referer o registros del sistema (logs). Esta exposición incrementa significativamente el riesgo de secuestro de sesión (Session Hijacking), ya que los identificadores pueden almacenarse en el historial del navegador, ser registrados por proxies intermedios o filtrarse a sitios de terceros a través del encabezado Referer. Los pentesters identifican estas exposiciones monitoreando todas las peticiones salientes e inspeccionando los registros de la aplicación o las configuraciones del servidor para detectar filtraciones de datos involuntarias. En entornos reales, los atacantes explotan estas vulnerabilidades recolectando tokens de sesión válidos de equipos compartidos o analizando patrones de tráfico para evadir la autenticación y obtener acceso no autorizado a las cuentas de usuario.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Estado de la Prueba** | No Realizado |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si la variable expuesta es un token de sesión que permite la toma de control de la cuenta (Account Takeover) de forma inmediata.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Herramientas:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### ¿Se transmite el identificador de sesión en la cadena de consulta (URL query string)?
- [ ] No — los identificadores de sesión **no** están presentes en la URL query string  
- [ ] Sí — los identificadores están presentes pero la sesión es de corta duración o de bajo riesgo  
- [ ] Sí — identificadores de sesión (Session IDs) únicos **están** presentes en la URL query string *(Crítico)*  

### ¿La aplicación filtra tokens de sesión a través del encabezado Referer?
- [ ] No — la política `Referrer-Policy` evita la filtración o no existen enlaces a terceros  
- [ ] Sí — los tokens de sesión **son** transmitidos a dominios de terceros a través del encabezado Referer  

### ¿Se almacenan las variables de sesión en los registros (logs) de la aplicación o del servidor?
- [ ] No — las variables de sesión están enmascaradas o excluidas de los registros  
- [ ] Sí — las variables de sesión **se registran** en los logs de la aplicación o del servidor web en texto plano  

### ¿Utiliza la aplicación peticiones GET para operaciones que cambian el estado?
- [ ] No — la aplicación utiliza `POST` u otros métodos no idempotentes para operaciones sensibles  
- [ ] Sí — se utiliza `GET`, lo que provoca que los datos de sesión se almacenen en caché en el historial del navegador y en proxies intermedios  

### ¿Está configurado el encabezado `Cache-Control` para evitar el almacenamiento en caché de datos relacionados con la sesión?
- [ ] Sí — se aplica `Cache-Control: no-store` (o similar) a las respuestas sensibles  
- [ ] Sí — existen controles pero es posible un bypass a través de casos de borde (edge cases) del navegador  
- [ ] No — las respuestas sensibles que contienen datos de sesión **pueden** ser almacenadas en caché por el navegador  

---