## WSTG-ERRH-01 — Pruebas de Manejo Inadecuado de Errores

El manejo inadecuado de errores ocurre cuando una aplicación web revela detalles técnicos sensibles —como stack traces, información del esquema de la base de datos o rutas de archivos internos— a través de sus respuestas de error. Los atacantes provocan estos errores enviando malformed input, accediendo a recursos inexistentes o induciendo server-side exceptions para mapear la arquitectura subyacente de la aplicación e identificar posibles vectores para una explotación posterior. Esta filtración de información a menudo sirve como precursor de ataques más graves, incluidos SQL Injection o Path Traversal, al proporcionar al atacante especificaciones precisas del entorno. Una implementación segura debe proporcionar mensajes genéricos y amigables para el usuario, capturando diagnósticos detallados únicamente en registros (logs) seguros del lado del servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### ¿Utiliza la aplicación un manejador de errores genérico y global para las excepciones no controladas?
- [ ] Sí — las páginas de error genéricas están **habilitadas** y **no** revelan detalles técnicos  
- [ ] Sí — se utilizan páginas de error personalizadas pero la filtración **es posible** a través de cabeceras (headers) específicas  
- [ ] No — las páginas de error por defecto del servidor (p. ej., Tomcat, IIS, Nginx) son **visibles**  

### ¿Se puede enumerar información técnica a través de malformed input o casos de borde (boundary cases)?
- [ ] No — los errores se manejan de manera segura con IDs de referencia únicos para el registro (logging)  
- [ ] Sí — los stack traces o las consultas a la base de datos **se revelan** en las respuestas  
- [ ] Sí — las rutas de archivos internos, variables de entorno o cadenas de versión del servidor **se revelan**  

### ¿Están las respuestas de la API filtrando objetos de error detallados (verbose) o información de depuración (debug)?
- [ ] No — las APIs devuelven códigos de error estandarizados y mensajes JSON saneados  
- [ ] Sí — las APIs devuelven objetos de depuración detallados o detalles completos de la excepción en el cuerpo de la respuesta  
- [ ] Sí — los stack traces se incluyen en los campos `details` o `exception` de la respuesta JSON  

### ¿Se comporta la aplicación de manera diferente (Time-based o Content-based) cuando ocurren errores?
- [ ] No — las firmas de respuesta (signatures) y los tiempos son consistentes independientemente del tipo de error  
- [ ] Sí — las respuestas diferenciales **pueden** usarse para enumerar estados válidos frente a inválidos (p. ej., User Enumeration)  

---