## WSTG-CONF-13 — Path Confusion

Path Confusion surge de las discrepancias en cómo los diferentes componentes web, como proxies inversos, balanceadores de carga y servidores de aplicaciones backend, analizan e interpretan las rutas URL. Los atacantes explotan estas inconsistencias inyectando caracteres específicos como puntos y coma, barras codificadas o segmentos de puntos para engañar a los filtros de seguridad y acceder a endpoints restringidos o recursos estáticos. Esta vulnerabilidad puede derivar en fallos de seguridad críticos, incluyendo la omisión de autenticación (Authentication Bypass), el acceso no autorizado a datos y el envenenamiento de caché web (Web Cache Poisoning), ya que el front-end puede aplicar reglas de seguridad a una ruta que el back-end interpreta de manera diferente. La explotación exitosa suele ocurrir en el límite arquitectónico donde la lógica de enrutamiento y normalización de solicitudes diverge a lo largo del stack tecnológico.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si la confusión de rutas resulta en una omisión de autenticación o Broken Access Control para endpoints administrativos sensibles.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### ¿Interpretan las diferentes capas arquitectónicas los delimitadores de ruta (ej., `;`, `#`, `?`) de manera consistente?
- [ ] Sí — todas las capas normalizan e interpretan los delimitadores de ruta de manera idéntica  
- [ ] No — existen inconsistencias pero **no** pueden utilizarse para acceder a recursos restringidos  
- [ ] No — las discrepancias permiten que un atacante omita los filtros del front-end y alcance la lógica del back-end  

### ¿Se pueden omitir los controles de acceso utilizando secuencias de Path Traversal o caracteres codificados?
- [ ] No — la normalización se aplica de manera consistente antes de las comprobaciones de seguridad  
- [ ] Sí — la omisión es **posible** mediante secuencias de segmentos de puntos (ej., `/admin/..;/`)  
- [ ] Sí — la omisión es **posible** mediante caracteres codificados en URL (ej., `%2f`, `%2e%2e%2f`)  

### ¿Es la aplicación susceptible a Web Cache Poisoning a través de Path Confusion?
- [ ] No — la lógica de almacenamiento en caché no se ve afectada por la ambigüedad de rutas o información de ruta adicional  
- [ ] Sí — el contenido malicioso **puede** almacenarse en caché para rutas legítimas debido a discrepancias en el mapeo  
- [ ] Sí — la información sensible **puede** almacenarse en caché en directorios públicos a través de técnicas de `RCD` (Relative Path Overwrite)  

### ¿Maneja el servidor la "información de ruta adicional" (PathInfo) de forma segura?
- [ ] No — la función está **deshabilitada** o no existe  
- [ ] Sí — `PathInfo` se maneja y **no** interfiere con los filtros de seguridad  
- [ ] No — `PathInfo` permite a un atacante añadir datos que confunden la lógica de enrutamiento de la aplicación  

---