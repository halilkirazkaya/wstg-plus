## WSTG-CLNT-11 — Prueba de Web Messaging

Web Messaging, o `postMessage`, permite la comunicación entre orígenes (cross-origin) entre objetos de ventana, como una página principal y una ventana emergente (popup) o un iframe embebido. Este mecanismo es fundamental para la funcionalidad web moderna, pero introduce un riesgo significativo si la aplicación receptora no verifica la identidad del remitente o gestiona incorrectamente el Payload del mensaje. Los atacantes explotan estas vulnerabilidades alojando un sitio malicioso que incrusta la aplicación objetivo y envía mensajes manipulados para activar acciones no deseadas, exfiltrar datos sensibles o ejecutar Cross-Site Scripting (XSS) basado en DOM. La explotación exitosa ocurre cuando los listeners de mensajes no validan estrictamente la propiedad `origin` o pasan `message.data` directamente a sumideros de ejecución (sinks) peligrosos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Herramientas:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### ¿Utiliza la aplicación la API `postMessage` para la comunicación entre documentos?
- [ ] No — los listeners de `postMessage` **no** están presentes en el código del lado del cliente  
- [ ] Sí — se utiliza `postMessage` para la comunicación interna de componentes  
- [ ] Sí — se utiliza `postMessage` para comunicarse con dominios o widgets de terceros  

### ¿Se valida estrictamente el origen de los mensajes entrantes?
- [ ] Sí — el origen se verifica contra una lista blanca (whitelist) estricta utilizando `===` o una comparación idéntica *(Más seguro)*  
- [ ] Sí — el origen se valida mediante una expresión regular (regex), pero la lógica **es** susceptible de derivación (bypass) (ej. `^https://trusted.com`)  
- [ ] No — la propiedad `origin` **no** se verifica, permitiendo que cualquier dominio envíe mensajes  
- [ ] No — se utiliza un comodín `*` en el `targetOrigin` del remitente, lo que podría filtrar datos a listeners maliciosos  

### ¿Se desinfecta (sanitize) el payload del mensaje antes de ser procesado por la aplicación?
- [ ] Sí — los datos se tratan como texto plano y no se utilizan sumideros (sinks) peligrosos  
- [ ] Sí — los datos se analizan (ej. `JSON.parse`) y se validan antes de su uso, y el bypass **no es posible**  
- [ ] No — los datos del mensaje se pasan directamente a sumideros peligrosos como `innerHTML`, `eval()` o `setTimeout()`  
- [ ] No — los datos del mensaje se utilizan para navegar por la ventana o cambiar el `location.href` sin validación  

### ¿Puede un atacante activar acciones no autorizadas o exfiltrar datos mediante un mensaje malicioso?
- [ ] No — incluso con un mensaje suplantado (spoofed), no se puede acceder a acciones ni datos sensibles  
- [ ] Sí — un mensaje manipulado **puede** activar cambios de estado sensibles (ej. cambio de contraseña, actualización de perfil)  
- [ ] Sí — un mensaje manipulado **puede** dar lugar a un XSS basado en DOM o a la exfiltración de tokens CSRF/datos de sesión  

---