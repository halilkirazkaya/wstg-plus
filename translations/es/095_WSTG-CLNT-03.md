## WSTG-CLNT-03 — HTML Injection

La Inyección HTML (HTML Injection) ocurre cuando una aplicación no logra sanitizar las entradas proporcionadas por el usuario antes de renderizarlas en el navegador, permitiendo que un atacante inyecte etiquetas HTML arbitrarias en el Document Object Model (DOM) de la página web. Aunque es similar al Cross-Site Scripting (XSS), el objetivo principal de la Inyección HTML es manipular la presentación visual de la página o facilitar ataques de Phishing mediante la inyección de formularios maliciosos y contenido engañoso. Los atacantes apuntan a parámetros que se reflejan en la interfaz de usuario (UI), como términos de búsqueda, campos de perfil de usuario o mensajes de error, para engañar a las víctimas y lograr que revelen credenciales sensibles o interactúen con enlaces de terceros no autorizados. Esta vulnerabilidad representa un riesgo significativo para la integridad de la interfaz de usuario de la aplicación y puede ser un precursor de ataques más complejos de ingeniería social o relacionados con la sesión.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### ¿Se reflejan las entradas controladas por el usuario en el DOM sin una codificación HTML adecuada?
- [ ] No — toda la entrada reflejada es estrictamente codificada en HTML antes de ser renderizada
- [ ] Sí — algunos caracteres **están** codificados, pero **es posible** realizar bypasses para ciertas etiquetas
- [ ] Sí — la entrada se refleja en bruto y la Inyección HTML **es posible**

### ¿Se pueden inyectar elementos HTML estructurales para alterar el diseño de la página?
- [ ] No — la aplicación filtra o elimina las etiquetas estructurales como `<div>`, `<table>` o `<iframe>`
- [ ] Sí — los elementos estructurales **pueden** ser inyectados, pero **no** impactan significativamente en la UI
- [ ] Sí — **es posible** una alteración visual (defacement) significativa de la UI mediante etiquetas inyectadas

### ¿Es posible inyectar elementos funcionales de phishing como formularios o enlaces maliciosos?
- [ ] No — las etiquetas `<form>`, `<input>` y `<a>` son bloqueadas o sanitizadas eficazmente
- [ ] Sí — se **pueden** inyectar enlaces maliciosos para redirigir a los usuarios a sitios externos
- [ ] Sí — se **pueden** inyectar elementos `<form>` funcionales para la recolección de credenciales *(Media)*

### ¿Mitigan el impacto de la inyección las cabeceras de seguridad o las Content Security Policies (CSP)?
- [ ] Sí — existe una CSP restrictiva y se **aplica** `form-action` o `frame-src`
- [ ] No — las cabeceras de seguridad están presentes, pero la CSP **está deshabilitada** o contiene unsafe-inline/comodines
- [ ] No — no hay CSP ni cabeceras de seguridad pertinentes **habilitadas**

---