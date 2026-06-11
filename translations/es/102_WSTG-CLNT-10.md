## WSTG-CLNT-10 — Pruebas de WebSockets

Las pruebas de WebSocket se centran en el canal de comunicación full-duplex establecido entre el cliente y el servidor, el cual omite los patrones tradicionales de solicitud-respuesta de HTTP. Los pentesters evalúan la seguridad del proceso de handshake, la presencia de protecciones contra Cross-Site WebSocket Hijacking (CSWSH) y el rigor de la validación de entradas aplicada a los payloads de los mensajes. La explotación suele implicar el secuestro de una sesión activa para la exfiltración de datos en tiempo real o la inyección de payloads maliciosos que activen vulnerabilidades en el lado del servidor, como Command Injection o SQLi, a través del socket persistente. Dado que muchos Web Application Firewalls (WAF) tradicionales no inspeccionan el tráfico WebSocket de manera efectiva, este protocolo a menudo proporciona un vector sigiloso para evadir las defensas perimetrales.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Herramientas:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### ¿Se establece la conexión WebSocket a través de un canal seguro?
- [ ] Sí — se obliga el uso de `wss://` (WebSocket Secure) para todas las conexiones  
- [ ] No — se utiliza `ws://`, lo que permite la interceptación por parte de un person-in-the-middle (PITM)  

### ¿Está la aplicación protegida contra Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] No — el servidor valida estrictamente el encabezado `Origin` durante el handshake  
- [ ] Sí — la validación del encabezado `Origin` es débil o **puede** ser evadida mediante orígenes nulos o suplantados  
- [ ] Sí — no se realiza ninguna validación del encabezado `Origin`, lo que permite a atacantes cross-site iniciar una conexión *(Crítico)*  

### ¿Se aplica validación y saneamiento de entradas a los payloads de los mensajes WebSocket?
- [ ] Sí — todos los mensajes entrantes son saneados y validados contra un esquema  
- [ ] Sí — existe cierta validación, pero la evasión **es posible** utilizando payloads codificados o no estandarizados  
- [ ] No — los mensajes se procesan directamente, lo que permite la inyección (XSS, SQLi, RCE) o la manipulación de la lógica  

### ¿Se realizan comprobaciones de autenticación y autorización en cada mensaje WebSocket?
- [ ] Sí — se verifica la sesión para cada mensaje o el protocolo es sin estado (stateless) mediante tokens  
- [ ] Sí — la autenticación ocurre en el handshake, pero la autorización para acciones específicas **no se aplica** por mensaje  
- [ ] No — la autenticación solo se comprueba en el handshake, lo que permite que el secuestro de sesión otorgue acceso completo  

### ¿Implementa el servidor limitación de tasa (rate limiting) o restricciones de recursos en el WebSocket?
- [ ] Sí — el Rate Limiting y los tamaños máximos de mensaje están **habilitados** y se aplican  
- [ ] No — no existen límites, lo que permite la Denegación de Servicio (DoS) mediante la inundación de mensajes o tamaños de payload elevados  

---