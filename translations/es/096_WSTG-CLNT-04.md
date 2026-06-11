## WSTG-CLNT-04 — Pruebas de Redirección de URL en el Lado del Cliente

La redirección de URL en el lado del cliente (Client-side URL redirection) ocurre cuando una aplicación web acepta una entrada controlada por el usuario—normalmente a través de parámetros de URL o fragmentos hash—y la utiliza dentro de un sumidero (sink) de redirección en el lado del cliente como `window.location` o `document.location.href`. Esta vulnerabilidad es aprovechada principalmente por los atacantes para realizar campañas de phishing sofisticadas, ya que la víctima confía inicialmente en el dominio legítimo antes de ser redirigida silenciosamente a un sitio malicioso. Más allá del phishing, las redirecciones en el lado del cliente a menudo pueden encadenarse para la exfiltración de datos sensibles, tales como tokens de acceso OAuth o identificadores de sesión, si la redirección ocurre mientras esos valores están presentes en la URL o en la cabecera `Referer`. En las Aplicaciones de Página Única (SPA) modernas, esta vulnerabilidad aparece frecuentemente en la lógica de enrutamiento, en parámetros "return to" tras la autenticación o en funcionalidades de cambio de idioma.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media* |

> *La severidad pasa a ser Alta si la redirección puede utilizarse para la exfiltración de tokens OAuth, IDs de sesión o para evadir controles de seguridad en un flujo de autenticación.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Herramientas:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### ¿Utiliza la aplicación scripts en el lado del cliente para gestionar redirecciones basadas en la entrada del usuario?
- [ ] No — la redirección se gestiona exclusivamente en el lado del servidor mediante códigos de estado HTTP `3xx`  
- [ ] Sí — la aplicación utiliza sinks de JavaScript como `window.location` para navegar basándose en parámetros de URL  
- [ ] Sí — la aplicación utiliza un enrutador de framework de cliente (ej. React Router, Vue Router) que procesa rutas proporcionadas por el usuario  

### ¿Se han implementado controles de validación de entrada o listas blancas (whitelisting) para la URL de destino?
- [ ] Sí — se aplica una **whitelist** estricta de dominios/rutas permitidos y el bypass **no es posible**  
- [ ] Sí — existe validación pero depende de regex débiles o listas negras (blacklists), y el bypass **es posible**  
- [ ] No — la aplicación acepta cualquier cadena arbitraria como destino de redirección  

### ¿Es posible evadir la lógica de redirección utilizando técnicas comunes de ofuscación?
- [ ] No — los controles manejan correctamente los caracteres codificados, las URLs relativas al protocolo (`//`) y las barras invertidas  
- [ ] Sí — el bypass **es posible** mediante URL encoding o doble codificación  
- [ ] Sí — el bypass **es posible** mediante URLs relativas al protocolo o caracteres `@` (ej. `https://expected.com@attacker.com`)  
- [ ] Sí — el bypass **es posible** utilizando comportamientos específicos del navegador (quirks) o inyección de bytes nulos (null byte injection)  

### ¿Expone el proceso de redirección datos sensibles al sitio de destino?
- [ ] No — no hay datos sensibles presentes en la URL ni se transmiten mediante cabeceras durante la redirección  
- [ ] Sí — los datos sensibles (ej. tokens de sesión, claves API) **se filtran** a través de la cabecera `Referer` al sitio externo  
- [ ] Sí — los datos sensibles presentes en el fragmento de URL (`#`) **son accesibles** para los scripts del sitio de destino  

### ¿Es posible ejecutar JavaScript a través del sink de redirección (DOM-based XSS)?
- [ ] No — el sink solo permite la navegación a protocolos `http` o `https`  
- [ ] Sí — el sink de redirección permite URIs `javascript:` o `data:`, lo que hace que el DOM-based XSS **sea posible**  

---