## WSTG-CLNT-13 — Pruebas de Cross Site Script Inclusion

La inclusión de scripts entre sitios (Cross-Site Script Inclusion - XSSI) ocurre cuando una aplicación web exporta datos sensibles dentro de un archivo JavaScript generado dinámicamente o una respuesta JSONP, que un sitio controlado por un atacante puede incluir mediante una etiqueta `<script>`. Dado que la inclusión de scripts omite la Same-Origin Policy (SOP), un atacante puede ejecutar el script en su propio contexto y sobrescribir objetos o variables globales para exfiltrar la información sensible. Esta vulnerabilidad se manifiesta típicamente en endpoints autenticados que devuelven datos específicos del usuario, como direcciones de correo electrónico, API keys o metadatos de sesión en formato de script. Desde la perspectiva de un atacante, la explotación implica crear una página maliciosa que haga referencia al script vulnerable y capture los cambios resultantes en las variables globales o las llamadas a funciones.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta |

> *La severidad se vuelve Alta si los datos filtrados incluyen tokens CSRF, identificadores de sesión o PII altamente sensible.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### ¿Existen endpoints autenticados que devuelvan JavaScript dinámico o JSONP con información sensible?
- [ ] No — todos los scripts son estáticos y **no pueden** contener datos específicos del usuario  
- [ ] Sí — existen scripts dinámicos pero **no contienen** información sensible  
- [ ] Sí — los scripts dinámicos contienen PII o tokens y **son** accesibles entre distintos orígenes  

### ¿Está implementado el encabezado `X-Content-Type-Options: nosniff` en las respuestas de scripts sensibles?
- [ ] Sí — el encabezado está **aplicado** y evita el sniffing de tipos MIME  
- [ ] No — el encabezado está **ausente**, lo que permite potencialmente que contenido que no es script se ejecute como tal  

### ¿Están los scripts sensibles protegidos por mecanismos anti-XSSI, como prefijos no ejecutables o encabezados personalizados?
- [ ] Sí — los scripts requieren un encabezado personalizado o un token que **no puede** ser enviado mediante una etiqueta `<script>` estándar  
- [ ] Sí — los scripts utilizan prefijos no ejecutables (ej. `)]}'`) que **no pueden** ser omitidos fácilmente  
- [ ] No — los scripts son accesibles directamente mediante peticiones GET utilizando credenciales de ambiente *(Crítico)*  

### ¿Es posible la exfiltración de datos sensibles mediante la sobrescritura de variables globales o prototipos?
- [ ] No — los datos sensibles tienen un alcance (scope) local o están protegidos mediante funciones modernas del navegador  
- [ ] Sí — los datos sensibles se asignan a variables globales y **pueden** ser capturados  
- [ ] Sí — los datos se filtran a través de callbacks de JSONP que **pueden** ser interceptados  

---