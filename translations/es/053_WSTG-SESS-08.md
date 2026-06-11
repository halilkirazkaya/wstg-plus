## WSTG-SESS-08 — Session Puzzling

El Session Puzzling, también conocido como Session Variable Overloading (Sobrecarga de Variables de Sesión), ocurre cuando una aplicación utiliza la misma variable de sesión para múltiples propósitos en diferentes módulos o no limpia adecuadamente los estados de sesión durante las transiciones de flujo de trabajo. Los atacantes explotan este comportamiento poblando variables de sesión en un contexto —como una vista previa de perfil no autenticada o un registro de varios pasos— y luego navegando hacia un área protegida donde la aplicación confía incorrectamente en esas variables preexistentes. Esto puede derivar en impactos críticos, incluyendo el Authentication Bypass, Privilege Escalation o el acceso no autorizado a datos sensibles al engañar a la aplicación para que crea que una sesión se encuentra en un estado de mayor privilegio del que realmente tiene. La vulnerabilidad es más frecuente en aplicaciones complejas con estado (stateful) donde la lógica de gestión de sesiones se aplica de forma inconsistente entre los diferentes componentes funcionales.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### ¿Utiliza la aplicación los mismos nombres de variables de sesión para diferentes propósitos en módulos distintos?
- [ ] No — los nombres de las variables son únicos o los alcances (scopes) están aislados  
- [ ] Sí — existen nombres de variables compartidos pero el estado se limpia entre módulos  
- [ ] Sí — existen nombres de variables compartidos y el estado **no se limpia**  

### ¿Pueden las acciones no autenticadas influir en las variables de sesión utilizadas en contextos autenticados?
- [ ] No — las variables de sesión se inicializan solo **después** de una autenticación exitosa  
- [ ] Sí — las variables de sesión pueden establecerse antes del login pero **no** afectan al estado post-login  
- [ ] Sí — las variables de sesión establecidas durante la pre-autenticación **son** confiables post-autenticación *(Crítico)*  

### ¿La aplicación reinicializa el identificador de sesión y sus variables asociadas ante un cambio en el nivel de privilegios?
- [ ] Sí — la sesión se restablece por completo y **se emite** un nuevo ID  
- [ ] Parcial — se emite un nuevo ID pero algunas variables de sesión **persisten**  
- [ ] No — el ID de sesión y todas las variables **permanecen** inalterados  

### ¿Se pueden obtener privilegios administrativos mediante la manipulación de variables de sesión a través de una cuenta sin privilegios?
- [ ] No — las variables de privilegio **no pueden** ser modificadas por los usuarios  
- [ ] Sí — las variables pueden ser modificadas pero la aplicación las **valida** contra la base de datos del backend  
- [ ] Sí — las variables pueden ser modificadas y la aplicación **confía** en el estado de la sesión implícitamente *(Crítico)*  

### ¿Se limpia el estado de la sesión inmediatamente después de completar un flujo de trabajo específico (ej. restablecimiento de contraseña, checkout)?
- [ ] Sí — las variables de sesión del flujo de trabajo se destruyen al finalizar  
- [ ] No — las variables del flujo de trabajo **persisten** y pueden ser reutilizadas en otras áreas de la aplicación  

---