## WSTG-CLNT-01 — Testing for DOM Based Cross Site Scripting

El Cross-Site Scripting basado en DOM (DOM XSS) ocurre cuando una aplicación contiene JavaScript del lado del cliente que procesa datos de una fuente no confiable de manera insegura, generalmente escribiendo los datos de vuelta en el Document Object Model (DOM) a través de un sink peligroso. A diferencia del XSS reflejado o almacenado, la vulnerabilidad existe completamente dentro del código del lado del cliente; el Payload a menudo no se envía al servidor en absoluto, ya que es procesado por sinks como `innerHTML`, `eval()` o `document.write()`. Los atacantes explotan esto creando URLs con fragmentos o parámetros maliciosos que, al ser cargados por el navegador de la víctima, ejecutan JavaScript arbitrario en el contexto de la sesión del usuario. Esto puede llevar a la exfiltración de tokens de sesión, el robo de datos sensibles y acciones no autorizadas realizadas en nombre del usuario autenticado.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Herramientas:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### ¿Se utilizan fuentes no confiables para capturar datos controlados por el usuario en JavaScript?
- [ ] No — JavaScript **no** utiliza `location.hash`, `location.search` o `document.referrer`  
- [ ] Sí — se utilizan fuentes no confiables pero los datos **no** se pasan a ningún sink de ejecución  
- [ ] Sí — se utilizan fuentes no confiables y los datos fluyen directamente hacia la lógica del lado del cliente  

### ¿Se pasan datos controlados por el usuario a sinks peligrosos del DOM?
- [ ] No — **no** se utilizan sinks tales como `innerHTML`, `outerHTML`, `document.write()` o `eval()`  
- [ ] Sí — existen sinks peligrosos pero los datos están estrictamente **sanitizados** utilizando una librería segura como DOMPurify  
- [ ] Sí — existen sinks peligrosos y los datos se procesan con un filtrado basado en regex **insuficiente** o personalizado  
- [ ] Sí — existen sinks peligrosos y los datos se pasan **sin** ninguna sanitización o codificación *(Crítico)*  

### ¿Es posible alcanzar el sink de ejecución a través de un payload basado en URL?
- [ ] No — el flujo de la fuente al sink (source-to-sink) **no** puede activarse mediante una entrada externa  
- [ ] Sí — el flujo es **posible** pero requiere una interacción compleja del usuario  
- [ ] Sí — el flujo es **posible** y puede activarse a través de un fragmento de URL o un parámetro de consulta simple  

### ¿Los encabezados de seguridad modernos o las mitigaciones basadas en el navegador están neutralizando el riesgo?
- [ ] Sí — una `Content-Security-Policy` (CSP) estricta con `script-src` y `trusted-types` está **habilitada** y previene la ejecución  
- [ ] Sí — existe una CSP pero `unsafe-inline` o hashes débiles hacen que un Exploit sea **posible**  
- [ ] No — no existe una CSP para mitigar la ejecución basada en DOM  

### ¿Utiliza la aplicación frameworks del lado del cliente con protecciones integradas?
- [ ] Sí — el framework (ej. Angular, React) codifica automáticamente los datos y el bypass **no es posible**  
- [ ] Sí — se utiliza el framework pero las "vías de escape" como `dangerouslySetInnerHTML` están **habilitadas**  
- [ ] No — la aplicación utiliza JavaScript puro (vanilla) o librerías legadas (ej. versiones antiguas de jQuery) sin auto-escaping  

---