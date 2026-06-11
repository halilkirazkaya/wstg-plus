## WSTG-INFO-08 — Fingerprint Web Application Framework

El fingerprinting (identificación de huellas) de un framework de aplicaciones web implica identificar el stack tecnológico específico y las versiones de software utilizadas para construir y servir la aplicación. Los atacantes analizan las cabeceras (headers) de respuesta HTTP, las cookies específicas del framework, las estructuras de archivos predecibles y los artefactos de JavaScript únicos para determinar si el objetivo está ejecutando plataformas como React, Angular, Django o Spring. Esta fase de reconocimiento es vital porque permite al evaluador reducir la superficie de ataque a vulnerabilidades conocidas (CVE) y debilidades de configuración comunes específicas de ese framework. Al comprender la arquitectura subyacente, un atacante puede pasar de pruebas genéricas a estrategias de explotación (exploit) altamente dirigidas.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Informativa |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### ¿Las cabeceras (headers) de respuesta HTTP exponen el nombre o la versión del framework?
- [ ] No — las cabeceras como `X-Powered-By` o `Server` **no** están presentes o han sido saneadas  
- [ ] Sí — el nombre del framework está presente pero la versión **está oculta**  
- [ ] Sí — tanto el nombre del framework como la versión **están expuestos**  

### ¿Son identificables las cookies específicas del framework o las estructuras de directorios?
- [ ] No — los artefactos comunes del framework (por ejemplo, rutas `.js`, nombres de cookies) están ofuscados o han sido renombrados  
- [ ] Sí — las cookies específicas del framework (por ejemplo, `JSESSIONID`, `PHPSESSID`, `csrftoken`) **son** identificables  
- [ ] Sí — las estructuras de directorios (por ejemplo, `/wp-content/`, `_next/static/`) **son** visibles y confirman el framework  

### ¿Revelan los mensajes de error o los comentarios del código fuente detalles del framework?
- [ ] No — el manejo de errores es genérico y los comentarios han sido eliminados  
- [ ] Sí — los stack traces específicos del framework **están habilitados** y son visibles en las respuestas  
- [ ] Sí — el código fuente HTML contiene comentarios o etiquetas de metadatos que identifican el framework  

### ¿Existen vulnerabilidades conocidas asociadas con la versión del framework identificada?
- [ ] No — el framework identificado está actualizado y **no tiene** vulnerabilidades públicas conocidas  
- [ ] Sí — la versión del framework está desactualizada y se **pueden** identificar CVEs específicos  

---