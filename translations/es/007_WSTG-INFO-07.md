## WSTG-INFO-07 — Mapeo de rutas de ejecución a través de la aplicación

El mapeo de rutas de ejecución consiste en la identificación sistemática de todos los endpoints accesibles, flujos funcionales y ramas de toma de decisiones dentro de una aplicación web. Este proceso es vital para garantizar que ninguna funcionalidad "oscura" —como código heredado, interfaces de depuración o rutas de API no documentadas— permanezca oculta al escrutinio de seguridad, ya que estas suelen carecer de controles de seguridad modernos. Los atacantes utilizan una combinación de spidering automatizado y exploración manual para visualizar la estructura de la aplicación, descubriendo a menudo vulnerabilidades como el acceso administrativo no autorizado o fallos en la lógica de negocio durante el proceso. Al correlacionar las entradas con respuestas específicas del servidor, los evaluadores pueden identificar con precisión la lógica de procesamiento sensible que requiere pruebas de análisis profundo dirigidas para garantizar una cobertura robusta.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Informativa |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### ¿Se ha indexado completamente la aplicación utilizando crawling automático y manual?
- [ ] Sí — se ha completado un mapa exhaustivo de todos los recursos visibles y enlazados  
- [ ] Sí — se ha completado el crawling automático, pero la exploración manual está pendiente  
- [ ] No — la aplicación no ha sido indexada  

### ¿Se han identificado endpoints ocultos o no documentados mediante análisis de JS o Brute Force de directorios?
- [ ] No — no se han descubierto rutas no documentadas a través de análisis de canal lateral  
- [ ] Sí — se han descubierto rutas ocultas, pero no se puede acceder a ellas sin autorización  
- [ ] Sí — los endpoints no documentados son accesibles y proporcionan funcionalidad sensible *(Crítica / Alta)*  

### ¿Pueden alterarse las rutas de ejecución mediante parámetros o cabeceras no estándar?
- [ ] No — parámetros como `debug`, `test` o `admin` no afectan al flujo de ejecución  
- [ ] Sí — los parámetros cambian la salida pero no eluden los controles de seguridad  
- [ ] Sí — se aplican parámetros que alteran la ruta y permiten eludir la lógica prevista  

### ¿Está completamente mapeado el flujo de la lógica de negocio de varios pasos (p. ej., autenticación multifactor, checkout)?
- [ ] Sí — se han identificado todos los estados y transiciones en los procesos de varios pasos  
- [ ] No — las transiciones complejas de la máquina de estados no pueden mapearse por completo  

### ¿Están expuestos los archivos de documentación de API (Swagger/OpenAPI) o los mapas del lado del cliente (Source Maps)?
- [ ] No — no hay mapas arquitectónicos ni archivos de documentación expuestos públicamente  
- [ ] Sí — la documentación está presente pero está protegida por autenticación  
- [ ] Sí — Swagger UI, OpenAPI JSON o JS Source Maps están habilitados y son accesibles públicamente  

---