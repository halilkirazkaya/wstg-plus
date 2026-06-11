## WSTG-SESS-03 — Session Fixation

La fijación de sesión (Session Fixation) ocurre cuando una aplicación no invalida o rota el identificador de sesión después de que un usuario se autentica con éxito, lo que permite a un atacante forzar un token de sesión conocido a una víctima. Si la aplicación conserva el ID de sesión previo a la autenticación después del inicio de sesión, un atacante puede proporcionar a una víctima un enlace específicamente diseñado que contenga un ID de sesión fijo y, posteriormente, secuestrar la sesión autenticada. Esta vulnerabilidad se manifiesta típicamente en formularios de inicio de sesión o a través de identificadores de sesión aceptados mediante parámetros de URL y cookies. Desde la perspectiva de un atacante, la explotación consiste en "fijar" la sesión a través de un enlace malicioso o una vulnerabilidad de inyección de cabeceras y esperar a que la víctima proporcione sus credenciales, evitando eficazmente la necesidad de robar un token activo.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Estado de la Prueba** | No realizado |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### ¿Cambia el identificador de sesión después de una autenticación exitosa?
- [ ] Sí — **se emite** un nuevo identificador de sesión y el anterior es invalidado *(Más seguro)*  
- [ ] Sí — **se emite** un nuevo identificador de sesión, pero el anterior **permanece válido**  
- [ ] No — el identificador de sesión **sigue siendo el mismo** antes y después de la autenticación *(Crítico)*  

### ¿Acepta la aplicación identificadores de sesión proporcionados a través de parámetros de URL?
- [ ] No — los IDs de sesión se gestionan **exclusivamente** mediante cookies  
- [ ] Sí — se aceptan IDs de sesión en la URL, pero son **ignorados** o **rotados** al iniciar sesión  
- [ ] Sí — los IDs de sesión en la URL son **aceptados** y **persisten** después de la autenticación  

### ¿Se puede forzar un ID de sesión definido por el atacante en la aplicación?
- [ ] No — la aplicación **rechaza** IDs de sesión inválidos o inexistentes proporcionados por el usuario  
- [ ] Sí — la aplicación **acepta** e inicializa una sesión utilizando cualquier ID arbitrario proporcionado en una cookie  
- [ ] Sí — la aplicación **acepta** e inicializa una sesión utilizando cualquier ID arbitrario proporcionado en un parámetro de URL  

### ¿Se finalizan correctamente las sesiones previas a la autenticación?
- [ ] Sí — la sesión anónima se **destruye por completo** al producirse el evento de inicio de sesión  
- [ ] No — la sesión anónima **no se destruye**, lo que permite la posible recolección de sesiones  

### ¿Está la cookie de sesión protegida contra la inyección en el lado del cliente?
- [ ] Sí — se **aplican** los flags `HttpOnly` y `Secure` para evitar la fijación basada en scripts  
- [ ] No — los flags **no se aplican**, lo que permite establecer IDs de sesión a través de Cross-Site Scripting (XSS)  

---