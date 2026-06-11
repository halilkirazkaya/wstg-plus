## WSTG-CLNT-06 — Pruebas de Manipulación de Recursos del Lado del Cliente

La manipulación de recursos del lado del cliente ocurre cuando una aplicación permite que la entrada controlada por el usuario influya en la fuente o ruta de los recursos cargados por el navegador, tales como scripts, hojas de estilo o iframes. Al manipular fragmentos de URI, parámetros de consulta u otras fuentes del DOM, un atacante puede obligar a la aplicación a cargar contenido externo malicioso o ejecutar código no autorizado. Esta vulnerabilidad se encuentra típicamente en la lógica de JavaScript que puebla dinámicamente los atributos `src` o `href` de los elementos HTML sin una validación suficiente contra una lista de permitidos (allow-list) confiable. Desde la perspectiva de un atacante, esto se explota mediante la construcción de una URL que redirecciona la carga de recursos a un servidor controlado por el atacante, lo que potencialmente resulta en Cross-Site Scripting (XSS) basado en DOM, exfiltración de datos sensibles o UI redressing.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Media / Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Herramientas:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### ¿Utiliza la aplicación sinks del lado del cliente para cargar o incrustar recursos dinámicamente?
- [ ] No — los recursos se cargan únicamente a través de HTML estático o lógica del lado del servidor  
- [ ] Sí — el JavaScript del lado del cliente puebla dinámicamente atributos de recursos como `src`, `href` o `data`  

### ¿Se han implementado controles de validación o listas de permitidos para las fuentes URI de los recursos?
- [ ] Sí — **se aplican** listas de permitidos estrictas y validación de origen a todos los recursos dinámicos  
- [ ] Sí — la validación está presente pero depende de listas negras (blacklists) débiles o expresiones regulares fáciles de evadir  
- [ ] No — no se aplican controles de validación a las rutas de recursos controladas por el usuario  

### ¿Es posible evadir la validación de URI utilizando protocolos alternativos o codificación?
- [ ] No — **no es posible** evadir los filtros de recursos  
- [ ] Sí — la evasión **es posible** utilizando diferentes protocolos como `data:`, `vbscript:` o `javascript:`  
- [ ] Sí — la evasión **es posible** utilizando caracteres codificados o bytes nulos para eludir coincidencias de cadenas simples  

### ¿Puede un atacante redireccionar la carga de recursos a un dominio externo no confiable?
- [ ] No — las rutas de los recursos están restringidas al mismo origen o a un subdirectorio fijo  
- [ ] Sí — la aplicación **puede** ser forzada a cargar scripts o estilos desde un dominio externo arbitrario  

### ¿Cuál es el impacto máximo de la manipulación de recursos?
- [ ] Bajo — la manipulación solo afecta a elementos no ejecutables como imágenes o texto no sensible  
- [ ] Medio — la manipulación permite la inyección de CSS o un UI redressing significativo  
- [ ] Alto — la manipulación conduce a XSS basado en DOM o a la ejecución de JavaScript arbitrario *(Crítico)*  

---