## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

El Stored Cross Site Scripting (XSS), también conocido como Persistent XSS, ocurre cuando una aplicación recibe datos de un usuario y los almacena en una base de datos persistente, sistema de archivos u otro medio de almacenamiento sin la validación o codificación adecuada. Cuando una víctima navega posteriormente a una página que recupera y muestra estos datos no saneados, el script malicioso se ejecuta dentro del contexto de su navegador bajo el origen de la aplicación. Esta vulnerabilidad es particularmente peligrosa porque no requiere que la víctima haga clic en un enlace especialmente diseñado; cualquier usuario que visualice la página afectada se convierte en un objetivo, lo que puede conducir al secuestro de sesión (session hijacking), acciones no autorizadas o recolección de credenciales. Los atacantes suelen dirigirse a secciones de comentarios, perfiles de usuario, foros de mensajes y registros administrativos donde la entrada se renderiza de manera constante para otros usuarios o administradores.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Herramientas:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### ¿Existen puntos de entrada que almacenen la entrada del usuario para su posterior visualización por otros usuarios?
- [ ] No — la aplicación no almacena entrada controlable por el usuario para su posterior renderizado  
- [ ] Sí — la entrada se almacena (ej. perfiles, comentarios) pero **no** se renderiza en un contexto HTML  
- [ ] Sí — la entrada se almacena y se renderiza para otros usuarios o administradores  

### ¿Se aplica codificación de salida (output encoding) o sanitización cuando se renderizan los datos almacenados?
- [ ] Sí — se **aplica** una codificación de salida sensible al contexto y **no es posible** realizar un bypass *(Más seguro)*  
- [ ] Sí — se **aplica** sanitización (ej. DOMPurify) pero la evasión (bypass) **es posible** debido a errores de configuración  
- [ ] No — los datos se renderizan directamente en el DOM **sin** codificación ni sanitización *(Alta)*  

### ¿Se puede evadir la validación de entrada o la sanitización utilizando payloads alternativos?
- [ ] No — los filtros gestionan de forma segura diversas codificaciones, etiquetas no estándar y controladores de eventos  
- [ ] Sí — el bypass **es posible** utilizando codificación de entidades HTML o etiquetas específicas (ej. `<svg>`, `<img>`)  
- [ ] Sí — el bypass **es posible** a través de payloads políglotas o XSS basado en mutación (mXSS)  

### ¿Proporciona una Content Security Policy (CSP) una capa secundaria de defensa?
- [ ] Sí — se ha **habilitado** una CSP estricta que evita la ejecución de scripts inline y fuentes no autorizadas  
- [ ] Sí — se ha **habilitado** una CSP pero está mal configurada (ej. `unsafe-inline` o un `script-src` demasiado permisivo)  
- [ ] No — no se ha **habilitado** ninguna CSP para la aplicación  

### ¿Es posible lograr un "Stored XSS to RCE" u otras cadenas de alto impacto (Blind XSS)?
- [ ] No — el impacto está limitado al contexto del navegador del lado del cliente  
- [ ] Sí — el XSS almacenado **puede** ser utilizado para atacar a administradores (Blind XSS) en paneles internos  
- [ ] Sí — el XSS almacenado **puede** encadenarse con otras vulnerabilidades (ej. CSRF, exfiltración de datos sensibles)  

---