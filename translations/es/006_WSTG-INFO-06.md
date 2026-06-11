## WSTG-INFO-06 — Identificación de puntos de entrada de la aplicación

La identificación de los puntos de entrada de la aplicación consiste en mapear toda la superficie de ataque enumerando todas las formas posibles en que un usuario o un sistema externo puede interactuar con la aplicación. Este proceso incluye el descubrimiento de todas las URLs, endpoints de API, parámetros (GET, POST, basados en cookies), encabezados HTTP y protocolos especializados como WebSockets o gRPC. Para un pentester, esta etapa es vital porque las vulnerabilidades se encuentran a menudo en APIs "en la sombra" (shadow APIs) no documentadas, endpoints legados o estructuras de parámetros complejas que se pasaron por alto durante el desarrollo. Una enumeración exhaustiva garantiza que ningún componente de la aplicación permanezca como una caja negra sin probar, evitando así que los atacantes encuentren rutas no monitoreadas hacia el sistema.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Informativa |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### ¿Se han enumerado todos los parámetros GET y POST en toda la aplicación?
- [ ] Sí — se ha completado la enumeración exhaustiva mediante crawling manual y automatizado
- [ ] Sí — solo se han identificado parámetros comunes mediante crawling estándar
- [ ] No — los parámetros están mayoritariamente **sin identificar** o sin probar

### ¿Se buscan parámetros no documentados u ocultos (p. ej., debug, admin) utilizando fuzzing?
- [ ] No — el descubrimiento de parámetros ocultos **no es necesario** para este alcance específico
- [ ] Sí — se ha realizado fuzzing de parámetros (p. ej., usando `Arjun`) **sin hallazgos**
- [ ] Sí — se **descubrieron** parámetros ocultos y se mapearon para pruebas posteriores
- [ ] No — **no se realizó** el descubrimiento de parámetros ocultos

### ¿Se han identificado todos los métodos HTTP soportados (PUT, DELETE, PATCH, etc.) para cada endpoint?
- [ ] Sí — el descubrimiento de métodos **se aplica** a todos los endpoints descubiertos
- [ ] Sí — el descubrimiento de métodos **se aplica** solo a endpoints de alto interés o sensibles
- [ ] No — solo **se identifican** los métodos estándar GET y POST

### ¿Se han mapeado los puntos de entrada no estándar como WebSockets, gRPC o encabezados personalizados?
- [ ] No — la aplicación **no utiliza** protocolos no estándar ni encabezados personalizados
- [ ] Sí — se **identifican** todos los puntos de entrada específicos de protocolos y encabezados personalizados
- [ ] No — **existen** puntos de entrada no estándar pero no se han mapeado

### ¿Se ha mapeado completamente la superficie de la API (incluyendo endpoints con versiones como /v1/ o /v2/)?
- [ ] No — la aplicación **no expone** una API
- [ ] Sí — todas las versiones y endpoints de la API **están identificados** y documentados
- [ ] Sí — solo **se identifica** la versión actual de la API en producción
- [ ] No — los endpoints de la API **permanecen sin mapear** o solo se han descubierto parcialmente

---