## WSTG-INPV-09 — Pruebas de XPath Injection

XPath Injection ocurre cuando una aplicación utiliza información proporcionada por el usuario para construir una consulta XPath para datos XML, lo que permite a un atacante interferir con la lógica de la consulta. Al inyectar sintaxis específica como comillas simples, corchetes y operadores lógicos, un atacante puede navegar por la estructura del documento XML, eludir la autenticación o exfiltrar nodos de datos sensibles. Esta vulnerabilidad se encuentra comúnmente en servicios web, interfaces de gestión de configuración y sistemas heredados que dependen de almacenes de datos basados en XML en lugar de bases de datos SQL tradicionales. Desde la perspectiva de un atacante, esto se explota a menudo utilizando técnicas basadas en booleanos o en errores para mapear sistemáticamente el árbol XML y recuperar valores ocultos del backend.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Herramientas:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### ¿Se sanean o parametrizan adecuadamente las entradas del usuario utilizadas para construir consultas XPath?
- [ ] Sí — las entradas **no** se utilizan en consultas XPath o están estrictamente parametrizadas  
- [ ] Sí — **se aplica** una validación/codificación de entrada robusta y el bypass **no es posible**  
- [ ] Sí — **se aplica** algo de validación pero el bypass **es posible** utilizando caracteres codificados  
- [ ] No — la entrada del usuario se concatena directamente en las consultas XPath *(Crítico)*  

### ¿Revela la aplicación detalles estructurales de XML/XPath a través de mensajes de error?
- [ ] No — se muestran páginas de error genéricas y **no** se filtran detalles de XML  
- [ ] Sí — los mensajes de error revelan la presencia de procesamiento XML pero **no** detalles de la ruta  
- [ ] Sí — los mensajes de error detallados revelan la sintaxis de XPath, nombres de nodos o rutas de archivos  

### ¿Es posible eludir la lógica de autenticación o autorización utilizando lógica de XPath?
- [ ] No — la lógica de autenticación **no** depende de XML/XPath o está implementada de forma segura  
- [ ] Sí — los controles de inicio de sesión o de permisos pueden eludirse utilizando payloads basados en lógica como `' or 1=1 or '`  

### ¿Se puede enumerar la estructura del documento XML o los datos a través de inyección ciega?
- [ ] No — la aplicación **no** es susceptible a inferencias basadas en booleanos o en tiempo  
- [ ] Sí — los nombres de los nodos y los datos **pueden** ser exfiltrados utilizando inferencia basada en booleanos  
- [ ] Sí — el recorrido completo del árbol XML y la extracción de datos **es posible**  

---