## WSTG-INPV-04 — Testing for HTTP Parameter Pollution

El HTTP Parameter Pollution (HPP) ocurre cuando una aplicación recibe múltiples parámetros HTTP con el mismo nombre y los procesa de manera inconsistente o insegura. Al suministrar parámetros duplicados, un atacante puede manipular la lógica interna de la aplicación, eludiendo potencialmente Web Application Firewalls (WAF), filtros de validación de entrada o mecanismos de control de acceso. Esta vulnerabilidad depende en gran medida del servidor web subyacente o del framework de la aplicación, que puede elegir el primero, el último o una versión concatenada de los parámetros repetidos. La explotación generalmente se dirige a endpoints que pasan parámetros a APIs de back-end, consultas de bases de datos o mecanismos de redirección de URL, lo que genera impactos que van desde Cross-Site Scripting (XSS) hasta la exfiltración de datos no autorizada.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### ¿Cómo maneja la aplicación múltiples parámetros HTTP con el mismo nombre?
- [ ] No — los parámetros duplicados son **rechazados** o ignorados por el servidor  
- [ ] Sí — el servidor selecciona consistentemente solo la **primera** o la **última** ocurrencia  
- [ ] Sí — el servidor **concatena** los valores, permitiendo potencialmente una inyección  

### ¿Se puede aprovechar el HPP para eludir controles de seguridad como un WAF o un filtro de entrada?
- [ ] No — los controles de seguridad inspeccionan correctamente **todas** las ocurrencias de un parámetro  
- [ ] Sí — un WAF o filtro solo inspecciona la **primera** ocurrencia, permitiendo que una **segunda** ocurrencia maliciosa pase  
- [ ] Sí — la concatenación permite a un atacante **dividir** un Payload a través de múltiples parámetros para evadir la detección  

### ¿La aplicación refleja parámetros contaminados en la respuesta sin una sanitización adecuada?
- [ ] No — los parámetros se sanitizan o codifican antes de reflejarse en la interfaz de usuario (UI)  
- [ ] Sí — la contaminación resulta en contenido reflejado, pero **se aplica** sanitización  
- [ ] Sí — la contaminación resulta en contenido reflejado y **no se aplica** sanitización, lo que deriva en XSS o errores de lógica  

### ¿Son vulnerables a la contaminación los parámetros pasados a llamadas de API internas o sistemas de back-end?
- [ ] No — las peticiones de back-end utilizan métodos de construcción seguros y no dinámicos  
- [ ] Sí — el HPP permite a un atacante **inyectar** o **sobrescribir** parámetros en la estructura de la petición del back-end  

### ¿Exhibe la aplicación un comportamiento diferente cuando los parámetros se contaminan en peticiones GET frente a POST?
- [ ] No — el comportamiento es consistente en todos los métodos HTTP  
- [ ] Sí — solo los parámetros GET son vulnerables a la contaminación  
- [ ] Sí — solo los parámetros POST (body) son vulnerables a la contaminación  

---