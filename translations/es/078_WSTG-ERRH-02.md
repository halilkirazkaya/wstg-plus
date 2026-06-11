## WSTG-ERRH-02 — Pruebas de Stack Traces

Las stack traces se generan cuando una aplicación no logra manejar una excepción de manera controlada, lo que resulta en la divulgación del estado de ejecución interno y la jerarquía de llamadas al usuario final. Para un consultor de pruebas de penetración (penetration tester), estas trazas son invaluables, ya que revelan el framework de la aplicación, versiones de librerías, rutas internas del sistema de archivos y el flujo lógico, lo que reduce significativamente el esfuerzo requerido para el reconocimiento (reconnaissance). La explotación implica provocar errores de manera intencionada mediante entradas malformadas, tipos de datos inesperados o violaciones de condiciones de contorno para forzar a la aplicación a un estado inestable y exfiltrar información sobre el stack tecnológico subyacente. Esta filtración a menudo proporciona el contexto necesario para identificar componentes de terceros vulnerables o para diseñar exploits específicos para fallos de lógica descubiertos dentro de las rutas de código divulgadas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Estado de la Prueba** | No realizada |
| **Severidad** | Baja / Media* |

> *La severidad aumenta a Media si la stack trace revela detalles arquitectónicos sensibles, consultas a bases de datos o parámetros de configuración interna.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### ¿Se devuelven stack traces completas al cliente ante errores de la aplicación?
- [ ] No — la aplicación devuelve páginas de error genéricas o mensajes de error personalizados  
- [ ] Sí — las stack traces están **habilitadas** pero solo para módulos específicos no sensibles  
- [ ] Sí — las stack traces completas están **habilitadas** y son visibles en el cuerpo de la respuesta HTTP  

### ¿Los mensajes de error divulgan información sensible del entorno?
- [ ] No — los mensajes de error no contienen detalles técnicos ni del entorno  
- [ ] Sí — los mensajes revelan **rutas de archivos internas** o **estructuras de directorios** del lado del servidor  
- [ ] Sí — los mensajes revelan **esquemas de base de datos**, **SQL queries** o **versiones de librerías de terceros**  

### ¿Se pueden provocar stack traces mediante la manipulación de parámetros de entrada?
- [ ] No — la entrada malformada se maneja de forma controlada o es rechazada por la validación  
- [ ] Sí — las trazas pueden provocarse mediante **tipos de datos inesperados** (p. ej., enviando un array donde se espera un string)  
- [ ] Sí — las trazas se provocan mediante **null bytes**, **caracteres especiales** o **valores límite** (boundary values)  

### ¿Se aplica de manera consistente un manejador de errores global en toda la aplicación?
- [ ] Sí — se aplica consistentemente un manejador de excepciones global en todos los endpoints probados  
- [ ] Sí — existe un manejador pero **no se aplica** a endpoints legados, API o recién añadidos  
- [ ] No — no se detecta ningún mecanismo global de manejo de errores, lo que resulta en errores por defecto del servidor  

---