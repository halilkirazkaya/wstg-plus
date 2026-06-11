## WSTG-APIT-01 — API Reconnaissance

El reconocimiento de API es el proceso sistemático de identificar y mapear la superficie de ataque de la API para descubrir endpoints, métodos soportados y estructuras de datos subyacentes. Los atacantes realizan esta fase para descubrir APIs no documentadas (shadow APIs), versiones obsoletas con vulnerabilidades heredadas (legacy vulnerabilities) y archivos de documentación expuestos, como definiciones de Swagger o OpenAPI, que revelan la lógica de negocio interna. Mediante el análisis de patrones de URL predecibles, archivos JavaScript del lado del cliente y resultados de fuerza bruta de directorios (directory brute-force), un adversario puede construir un mapa completo de la API para identificar objetivos para ataques de Broken Object Level Authorization (BOLA) o Mass Assignment. Este reconocimiento suele dirigirse a rutas comunes como `/api/v1/`, `/swagger.json` o `/graphql` para obtener una hoja de ruta de la funcionalidad del backend de la aplicación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Informativa / Baja |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Herramientas:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### ¿Son accesibles públicamente los archivos de documentación de la API (Swagger, OpenAPI, WSDL)?
- [ ] No — La documentación de la API **no** es accesible o está restringida por autenticación  
- [ ] Sí — La documentación es accesible pero requiere credenciales válidas  
- [ ] Sí — La documentación de la API (p. ej., `/swagger-ui.html`) es **accesible públicamente** sin autenticación  

### ¿Se pueden descubrir endpoints de API no documentados o "shadow" mediante fuzzing?
- [ ] No — El fuzzing de directorios y endpoints no arrojó rutas no documentadas  
- [ ] Sí — Se encontraron endpoints no documentados, pero **no** se puede acceder a ellos sin autorización  
- [ ] Sí — Se encontraron endpoints no documentados y **se puede** acceder a ellos sin una autorización válida  

### ¿Revela la aplicación múltiples versiones de la API (p. ej., /v1/, /v2/, /beta/)?
- [ ] No — Solo es accesible la versión actual y robustecida (hardened) de la API  
- [ ] Sí — Existen versiones heredadas (legacy) pero implementan los **mismos** controles de seguridad que la versión actual  
- [ ] Sí — Las versiones heredadas (p. ej., `/v1/`) son accesibles y **carecen** de los controles de seguridad de las versiones más recientes  

### ¿Los recursos del lado del cliente (JavaScript/Apps móviles) filtran estructuras de endpoints o claves de API?
- [ ] No — El código del lado del cliente **no** contiene rutas de API embebidas (hardcoded) ni claves sensibles  
- [ ] Sí — El código del lado del cliente contiene mapas de endpoints de API pero **no** claves sensibles  
- [ ] Sí — Las claves de API, secretos o URLs de endpoints de uso interno **están** embebidos (hardcoded) en los recursos del lado del cliente  

### ¿Se pueden descubrir parámetros o encabezados ocultos en endpoints conocidos?
- [ ] No — El fuzzing de parámetros (p. ej., mediante `Arjun`) **no** reveló entradas ocultas  
- [ ] Sí — Se descubrieron parámetros ocultos (p. ej., `debug=true`, `admin=1`) pero **no** son funcionales  
- [ ] Sí — Se descubrieron parámetros ocultos y **pueden** utilizarse para alterar el comportamiento de la aplicación  

---