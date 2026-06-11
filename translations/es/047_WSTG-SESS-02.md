## WSTG-SESS-02 — Testing for Cookies Attributes

La gestión de sesiones a menudo depende de las cookies HTTP para mantener el estado, lo que convierte a los atributos de seguridad de estas cookies en un objetivo principal para los atacantes. Al omitir o configurar incorrectamente atributos como `HttpOnly`, `Secure` y `SameSite`, las aplicaciones exponen los tokens de sesión al robo mediante Cross-Site Scripting (XSS), interceptación a través de canales no cifrados o ataques de Cross-Site Request Forgery (CSRF). Los pentesters examinan estas etiquetas para determinar si un atacante puede exfiltrar identificadores de sesión del modelo de objetos del documento (DOM) del navegador o forzar al navegador a enviarlos en solicitudes de origen cruzado (cross-origin). La configuración adecuada de los atributos es una medida fundamental de defensa en profundidad para garantizar que los tokens sensibles permanezcan limitados a contextos autorizados y seguros.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Herramientas:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Análisis de la Implementación de Etiquetas de Cookies
| Atributo | Presente | Ausente |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### ¿Se aplica la etiqueta `HttpOnly` a las cookies de sesión sensibles?
- [ ] Sí — aplicada a **todas** las cookies de sesión sensibles *(Más seguro)*  
- [ ] Sí — aplicada solo a algunas cookies de sesión  
- [ ] No — la etiqueta **no se aplica**, permitiendo el acceso a través de scripts del lado del cliente *(Crítico)*  

### ¿Se aplica la etiqueta `Secure` para las cookies transmitidas a través de HTTPS?
- [ ] Sí — aplicada a **todas** las cookies transmitidas a través de TLS  
- [ ] No — la etiqueta **no se aplica**, permitiendo que las cookies se envíen a través de HTTP en texto plano  

### ¿Se utiliza el atributo `SameSite` para mitigar los riesgos de CSRF?
- [ ] Sí — configurado como `Strict` o `Lax` para las cookies de sesión  
- [ ] Sí — configurado como `None` pero con la etiqueta `Secure` presente  
- [ ] No — el atributo **está ausente** o configurado como `None` **sin** `Secure`  

### ¿Están los atributos `Domain` y `Path` restringidos al alcance mínimo necesario?
- [ ] Sí — limitado al subdominio y la ruta de la aplicación **específicos**  
- [ ] No — el alcance es demasiado **amplio** (p. ej., configurado para un dominio primario o la raíz `/`), lo que aumenta la superficie de ataque  

### ¿Expiran correctamente las cookies de sesión sensibles a través de los atributos `Expires` o `Max-Age`?
- [ ] Sí — se han configurado fechas de expiración adecuadas  
- [ ] No — las cookies son persistentes con tiempos de vida excesivamente **largos**  
- [ ] No — las cookies son solo de sesión pero **carecen** de la aplicación de tiempo de espera en el lado del servidor  

---