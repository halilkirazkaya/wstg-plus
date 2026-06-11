## WSTG-CLNT-12 — Prueba de Almacenamiento en el Navegador

La prueba del almacenamiento en el navegador implica analizar cómo una aplicación utiliza mecanismos del lado del cliente, tales como LocalStorage, SessionStorage, IndexedDB y WebSQL, para persistir datos. La información sensible almacenada de forma insegura —incluyendo PII, tokens de autenticación o identificadores de sesión— puede verse comprometida si un atacante logra un Cross-Site Scripting (XSS) u obtiene acceso físico al dispositivo del usuario. Los pentesters deben determinar si los datos se almacenan en texto claro, si permanecen accesibles tras el cierre de sesión y si el ciclo de vida del almacenamiento se gestiona adecuadamente para evitar fugas de datos. Desde la perspectiva de un atacante, estos mecanismos de almacenamiento son objetivos valiosos para la exfiltración de información de mantenimiento de estado que permite el secuestro de sesiones (session hijacking) o la persistencia de cuentas a largo plazo.

| Campo | Valor |
|---|---|
| **ID WSTG** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Media / Alta* |

> *La severidad se considera Alta si se almacenan tokens de sesión o PII sensible en LocalStorage y son accesibles mediante XSS.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### ¿Almacena la aplicación información sensible en el almacenamiento del navegador?
- [ ] No — la aplicación **no** utiliza el almacenamiento del navegador o solo almacena estados de la interfaz de usuario (UI) no sensibles.
- [ ] Sí — los datos sensibles se almacenan pero **están cifrados** utilizando criptografía robusta del lado del cliente.
- [ ] Sí — los datos sensibles (tokens, PII, secretos) se almacenan en **texto claro** *(Media)*.

### ¿Son los datos almacenados accesibles para scripts no autorizados (riesgo de XSS)?
- [ ] No — los datos se almacenan en cookies HttpOnly (no en el almacenamiento del navegador) o el almacenamiento **no se utiliza**.
- [ ] Sí — se utiliza LocalStorage/SessionStorage, lo que hace que los datos sean **completamente accesibles** a través de cualquier Payload de XSS *(Alta)*.

### ¿Se limpia adecuadamente el almacenamiento del navegador al cerrar la sesión del usuario?
- [ ] Sí — todo el almacenamiento relacionado con la aplicación se **borra explícitamente** durante el proceso de cierre de sesión (logout).
- [ ] No — el almacenamiento persiste después del cierre de sesión, pero **no** contiene datos de sesión sensibles.
- [ ] No — los tokens de autenticación o PII **persisten** en el almacenamiento después de que la sesión ha finalizado *(Media)*.

### ¿Utiliza la aplicación IndexedDB o WebSQL para conjuntos de datos grandes o sensibles?
- [ ] No — estos mecanismos de almacenamiento **no se utilizan**.
- [ ] Sí — los datos se almacenan y son **saneados adecuadamente** antes de ser renderizados en el DOM.
- [ ] Sí — se almacenan conjuntos de datos sensibles en **texto claro** dentro de las estructuras de IndexedDB/WebSQL.

### ¿Puede manipularse la integridad de los datos almacenados para afectar la lógica de la aplicación?
- [ ] No — la aplicación valida o firma los datos antes de procesarlos desde el almacenamiento.
- [ ] Sí — la modificación de los valores de almacenamiento (p. ej., roles de usuario, flags) **es posible** y resulta en un comportamiento alterado de la aplicación.

---