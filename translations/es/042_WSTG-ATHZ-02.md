## WSTG-ATHZ-02 — Pruebas de evasión del esquema de autorización

La evasión de esquemas de autorización (Bypassing authorization schemas) ocurre cuando una aplicación no logra aplicar los controles de acceso, permitiendo que un atacante acceda a recursos o funciones fuera de sus permisos previstos. Esta vulnerabilidad se manifiesta típicamente como Escalada de Privilegios Horizontal (Horizontal Privilege Escalation), donde un atacante accede a datos pertenecientes a otro usuario del mismo nivel, o Escalada de Privilegios Vertical (Vertical Privilege Escalation), donde un usuario con bajos privilegios obtiene capacidades administrativas. Los atacantes explotan estos fallos manipulando parámetros de solicitud como los IDs de recursos, modificando tokens de sesión o aprovechando cabeceras HTTP como `X-Original-URL` para eludir las restricciones basadas en rutas. Garantizar una autorización robusta es crítico para mantener la confidencialidad de los datos y prevenir acciones administrativas no autorizadas que podrían comprometer todo el sistema.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Herramientas:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### ¿Se previene la escalada de privilegios horizontal entre usuarios del mismo rol?
- [ ] Sí — los controles de autorización **se aplican** a cada solicitud de recurso y la evasión (**bypass**) no es posible  
- [ ] Sí — los controles de autorización **se aplican** pero pueden ser evadidos mediante IDOR o manipulación de parámetros  
- [ ] No — los usuarios **pueden** acceder a datos pertenecientes a otros usuarios simplemente cambiando un ID de recurso *(Alta)*  

### ¿Se previene la escalada de privilegios vertical para usuarios con bajos privilegios?
- [ ] No — la aplicación tiene un solo nivel de privilegios  
- [ ] Sí — las funciones administrativas **no pueden** ser accedidas por usuarios con bajos privilegios  
- [ ] Sí — las funciones administrativas están ocultas en la interfaz de usuario (UI) pero **pueden** ser accedidas mediante URL directa  
- [ ] No — los usuarios con bajos privilegios **pueden** realizar acciones administrativas manipulando roles o permisos *(Crítica)*  

### ¿Se puede evadir la autorización mediante la manipulación de verbos HTTP?
- [ ] No — los controles de acceso se aplican independientemente del método HTTP utilizado  
- [ ] Sí — cambiar el método (por ejemplo, de `GET` a `POST` o `PUT`) **evade** los controles de autorización  

### ¿Son accesibles los endpoints administrativos o restringidos sin ninguna autenticación?
- [ ] No — todos los endpoints sensibles requieren una sesión válida y permisos adecuados  
- [ ] Sí — algunos endpoints administrativos son accesibles si el atacante conoce la URL directa  
- [ ] Sí — el acceso no autenticado a funciones sensibles **es posible** *(Crítica)*  

### ¿Permiten las cabeceras HTTP la elusión de la autorización basada en rutas?
- [ ] No — la aplicación **no** es susceptible a evasiones basadas en cabeceras  
- [ ] Sí — cabeceras como `X-Original-URL`, `X-Rewrite-URL` o `X-Forwarded-For` **pueden** ser utilizadas para evadir los controles de acceso  

---