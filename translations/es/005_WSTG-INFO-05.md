## WSTG-INFO-05 — Revisión del contenido de la página web para detectar fugas de información

La revisión del contenido de la página web implica la inspección manual y automatizada de las respuestas de la aplicación, incluyendo archivos HTML, JavaScript y CSS, para identificar información sensible expuesta involuntariamente a los usuarios. Los atacantes analizan minuciosamente el código fuente del lado del cliente en busca de comentarios de desarrolladores, campos de formulario ocultos, rutas internas del servidor, claves API (API keys) integradas en el código e información de depuración (debugging) que pueda ayudar en fases posteriores de explotación. Esta fuga ocurre a menudo cuando los desarrolladores olvidan eliminar metadatos o código de depuración antes de pasar a producción, revelando potencialmente fallos de lógica o detalles de la infraestructura del backend. Desde la perspectiva de un atacante, esto proporciona un método de bajo esfuerzo para mapear la estructura interna de la aplicación y descubrir puntos de entrada ignorados sin activar alertas de seguridad ni interactuar con la lógica del backend del servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Baja / Media* |

> *La severidad se vuelve Alta si se descubren credenciales integradas en el código (hardcoded), claves privadas o variables de entorno internas.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Herramientas:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### ¿Existen comentarios de desarrolladores que contengan información sensible en el código fuente?
- [ ] No — no se encontraron comentarios o solo se hallaron comentarios genéricos no sensibles  
- [ ] Sí — existen comentarios pero **no** contienen lógica ni datos sensibles  
- [ ] Sí — los comentarios **revelan** rutas internas, consultas SQL o instrucciones administrativas  
- [ ] Sí — los comentarios **revelan** credenciales o notas sensibles de los desarrolladores *(Alta)*  

### ¿Los campos de entrada HTML ocultos contienen datos sensibles o información de estado?
- [ ] No — no se encontraron campos ocultos o solo contienen tokens no sensibles  
- [ ] Sí — los campos ocultos se utilizan para el estado pero están **cifrados** o **firmados**  
- [ ] Sí — los campos ocultos contienen contraseñas en **texto plano**, roles de usuario o IDs internos  

### ¿Hay secretos integrados (hardcoded), claves API o direcciones IP internas en los archivos JavaScript?
- [ ] No — no se encontraron cadenas sensibles ni secretos en los archivos JS  
- [ ] Sí — los archivos JS contienen claves API públicas con las restricciones **adecuadas**  
- [ ] Sí — los archivos JS contienen claves API **no protegidas**, IPs internas o endpoints privados  
- [ ] Sí — los archivos JS contienen credenciales administrativas o claves privadas **hardcoded** *(Crítica)*  

### ¿La aplicación revela versiones del framework o rutas de archivos internas en el código fuente?
- [ ] No — la información del framework y las rutas de archivos están **minificadas** u **ofuscadas**  
- [ ] Sí — los nombres y versiones del framework son **visibles** pero están parcheados  
- [ ] Sí — las rutas de archivos internas o rutas absolutas del servidor están **expuestas** en el código fuente  

### ¿Es el código fuente propenso a fugas debido a la falta de minificación u ofuscación?
- [ ] No — el código de producción está **completamente minificado** y se han eliminado los comentarios  
- [ ] Sí — el código está minificado pero los comentarios **permanecen** en el fuente  
- [ ] Sí — los mapas de fuente (archivos `.map`) son **públicamente accesibles**, lo que permite la reconstrucción completa del código fuente  

---