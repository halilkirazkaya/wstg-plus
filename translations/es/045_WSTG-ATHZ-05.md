## WSTG-ATHZ-05 — Testing for OAuth Weaknesses

Las debilidades en OAuth abarcan una variedad de fallos en la implementación de los protocolos OAuth 2.0 o OpenID Connect (OIDC), que a menudo resultan en un account takeover completo o en el acceso no autorizado a datos. Estas vulnerabilidades suelen manifestarse durante el flujo de autorización (authorization flow), específicamente en relación con la validación de `redirect_uri`, la entropía y verificación del parámetro `state`, y el manejo seguro de los tokens de acceso o de ID. Los atacantes explotan estas debilidades interceptando códigos de autorización, redirigiendo tokens a dominios controlados por el atacante mediante open redirects, o realizando Cross-Site Request Forgery (CSRF) para vincular la sesión de una víctima a la cuenta de un atacante. Debido a que OAuth a menudo sirve como un mecanismo de autenticación primario, una sola configuración incorrecta puede comprometer la integridad de todo el sistema de identidad de los usuarios.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Herramientas:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### ¿Se ha implementado y validado el parámetro `state` para prevenir CSRF?
- [ ] Sí — `state` es obligatorio, único por sesión y se valida estrictamente en el servidor.
- [ ] Sí — `state` está presente pero es estático o predecible entre diferentes sesiones.
- [ ] Sí — `state` está presente pero la aplicación **no** lo valida en el callback.
- [ ] No — el parámetro `state` **no** se utiliza en la solicitud de autorización *(Crítica)*.

### ¿Se valida estrictamente el `redirect_uri` contra una lista blanca (whitelist)?
- [ ] Sí — solo se **aceptan** coincidencias exactas contra una whitelist prerregistrada.
- [ ] Sí — se aplica validación pero es **posible un bypass** mediante path traversal o manipulación de subdominios.
- [ ] Sí — se aplica validación pero es **posible un bypass** mediante parameter pollution o inyección de fragmentos (fragment injection).
- [ ] No — la aplicación **permite** URLs arbitrarias en el parámetro `redirect_uri` *(Crítica)*.

### ¿Puede manipularse el `response_type` para alterar el flujo?
- [ ] No — la aplicación solo acepta el flujo previsto (por ejemplo, `code`) y rechaza otros.
- [ ] Sí — cambiar de `code` a `token` (Implicit flow) **es posible** y filtra tokens en la URL.
- [ ] Sí — los flujos híbridos (hybrid flows) o valores de `response_type` no autorizados están **habilitados** y se procesan.

### ¿Se filtran los tokens de acceso o códigos de autorización mediante cabeceras Referer o el historial del navegador?
- [ ] No — los tokens/códigos se manejan de forma segura y las páginas sensibles utilizan una `Referrer-Policy` adecuada.
- [ ] Sí — los tokens/códigos **se filtran** a dominios de terceros a través de la cabecera `Referer`.
- [ ] Sí — los tokens/códigos **son visibles** en el historial del navegador debido a un almacenamiento en caché inseguro o solicitudes GET.

### ¿Aplica la aplicación el principio de "Mínimo Privilegio" (Least Privilege) para los alcances (scopes) de OAuth?
- [ ] Sí — la aplicación solicita solo los scopes mínimos necesarios para la funcionalidad.
- [ ] No — la aplicación solicita scopes excesivos o administrativos por defecto.
- [ ] No — el usuario **no puede** revisar o excluirse (opt-out) de scopes específicos durante la fase de consentimiento.

---