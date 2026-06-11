## WSTG-CLNT-02 — Pruebas de ejecución de JavaScript

Las pruebas de ejecución de JavaScript se centran en identificar instancias en las que una aplicación maneja de forma inadecuada los datos suministrados por el usuario, lo que conduce a la ejecución de scripts no autorizados dentro del contexto del navegador de la víctima. Esta vulnerabilidad suele dar como resultado Cross-Site Scripting (XSS), lo que permite a los atacantes exfiltrar cookies de sesión, manipular el Document Object Model (DOM) o realizar acciones en nombre del usuario. Los evaluadores de penetración examinan receptores (sinks) como `innerHTML`, `document.write()` y `eval()` donde pueden procesarse datos no saneados provenientes de fuentes (sources) como fragmentos de URL, `localStorage` o `window.name`. Una explotación exitosa compromete la integridad y confidencialidad de la sesión del usuario y puede servir como pivote para ataques del lado del cliente (client-side) más complejos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Herramientas:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### ¿Procesa la aplicación datos suministrados por el usuario en receptores (sinks) de JavaScript peligrosos?
- [ ] No — todos los datos están saneados o se utilizan en receptores seguros como `textContent`  
- [ ] Sí — los datos se procesan en receptores peligrosos pero **se aplica** saneamiento/codificación  
- [ ] Sí — los datos se procesan en receptores peligrosos y **no se aplica** saneamiento  

### ¿Existe una Content Security Policy (CSP) para mitigar la ejecución de scripts?
- [ ] Sí — una CSP restrictiva está **habilitada** y el bypass **no es posible**  
- [ ] Sí — una CSP está **habilitada** pero el bypass **es posible** a través de `unsafe-inline` o CDNs inseguros  
- [ ] No — no hay ninguna CSP **habilitada**  

### ¿Se puede ejecutar JavaScript a través de parámetros o fragmentos de URL (DOM-based XSS)?
- [ ] No — la entrada está codificada correctamente y la ejecución **no es posible**  
- [ ] Sí — la entrada se refleja en el DOM pero la ejecución **es prevenida** por las protecciones de frameworks modernos  
- [ ] Sí — la entrada se ejecuta directamente en el navegador y la explotación **es posible**  

### ¿Son los manejadores de eventos o los esquemas URI vulnerables a la inyección de scripts?
- [ ] No — la entrada se sanea para eliminar atributos de eventos y esquemas `javascript:`  
- [ ] Sí — los manejadores de eventos **pueden** ser inyectados pero los filtros limitan la complejidad del Payload  
- [ ] Sí — la ejecución de JavaScript arbitrario a través de los atributos `onerror`, `onload` o `href` **es posible**  

---