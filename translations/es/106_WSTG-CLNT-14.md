## WSTG-CLNT-14 — Testing for Reverse Tabnabbing

Reverse Tabnabbing es una vulnerabilidad de lado del cliente en la que una página abierta mediante `target="_blank"` conserva una referencia funcional a la página de origen (página padre) a través del objeto `window.opener`. Esto permite que un sitio de destino controlado por un atacante redirija mediante programación la pestaña original y de confianza a una URL maliciosa, típicamente una página de Phishing diseñada para recolectar credenciales. El ataque explota la confianza inherente del usuario en la pestaña original, ya que es poco probable que note que la página que acaba de dejar ha navegado hacia un dominio diferente. Las prácticas de seguridad modernas requieren el uso de los atributos `rel="noopener"` o `rel="noreferrer"` en las etiquetas de anclaje, o establecer la propiedad `opener` como null en JavaScript, para prevenir esta comunicación entre ventanas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Bajo / Medio* |

> *La severidad es Baja en la mayoría de los navegadores modernos, ya que ahora establecen por defecto `target="_blank"` como `rel="noopener"`. La severidad pasa a ser Media si la aplicación se dirige a navegadores antiguos o utiliza `window.open()` sin establecer la propiedad `opener` como null.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### ¿Existen enlaces que se abran en una nueva ventana o pestaña?
- [ ] No — ningún enlace utiliza `target="_blank"` o una redirección equivalente basada en JavaScript  
- [ ] Sí — `target="_blank"` se utiliza exclusivamente en enlaces internos y de confianza  
- [ ] Sí — `target="_blank"` se utiliza en enlaces externos o en URL proporcionadas por el usuario  

### ¿Está restringida la relación `window.opener` en las etiquetas de anclaje?
- [ ] Sí — `rel="noopener"` o `rel="noreferrer"` se **aplican de forma consistente** en todos los enlaces con `target="_blank"` *(Más seguro)*  
- [ ] Sí — los atributos se aplican pero **faltan** en enlaces de alto riesgo o generados por el usuario  
- [ ] No — los atributos **no se aplican**, lo que permite que la página hija acceda a `window.opener`  

### ¿Es la aplicación vulnerable a Reverse Tabnabbing a través de `window.open()` de JavaScript?
- [ ] No — no se utiliza `window.open()` o se establece explícitamente la propiedad `opener` como null  
- [ ] Sí — se utiliza `window.open()` y la referencia `opener` **permanece accesible** para la ventana hija  

### ¿Puede un atacante controlar el destino de un enlace que se abre en una nueva pestaña?
- [ ] No — todos los destinos de los enlaces están predefinidos (hardcoded) y apuntan a dominios de confianza  
- [ ] Sí — los destinos de los enlaces son proporcionados por el usuario (por ejemplo, perfiles de redes sociales, enlaces de biografía) y **no** están saneados con protecciones contra Tabnabbing  

---