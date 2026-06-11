## WSTG-ATHZ-03 — Pruebas de escalamiento de privilegios

El escalamiento de privilegios ocurre cuando un atacante explota vulnerabilidades para obtener acceso a recursos o funcionalidades reservadas para usuarios con niveles de autorización superiores o identidades diferentes. En la escalada vertical, un usuario estándar intenta acceder a funciones administrativas, mientras que la escalada horizontal implica acceder a datos que pertenecen a otro usuario con el mismo nivel de privilegios. Estas fallas se manifiestan típicamente en listas de control de acceso (ACL) mal implementadas, referencias directas inseguras a objetos (IDOR) o manipulación de parámetros dentro de tokens de sesión o identidad. Desde la perspectiva de un atacante, esto se logra a menudo manipulando IDs numéricos, modificando parámetros basados en roles en cookies o JWTs, o llamando directamente a endpoints de API ocultos que carecen de controles de autorización en el lado del servidor.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Herramientas:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### ¿La aplicación implementa múltiples niveles de privilegio o multitenencia?
- [ ] No — la aplicación es un sistema de usuario único o de rol único  
- [ ] Sí — se **han definido** múltiples roles o inquilinos y requieren pruebas de autorización  

### ¿Es posible el escalamiento horizontal de privilegios (acceso al mismo nivel)?
- [ ] No — se **aplican** controles de autorización a todos los identificadores de recursos y propietarios de sesiones  
- [ ] Sí — el acceso a los datos de otros usuarios **es posible** a través de IDOR o manipulación de parámetros  
- [ ] Sí — la filtración de datos **es posible**, pero la modificación de los datos de otros usuarios **no es posible**  

### ¿Es posible el escalamiento vertical de privilegios (de bajo a alto)?
- [ ] No — los endpoints administrativos aplican estrictamente controles de acceso basados en roles (RBAC)  
- [ ] Sí — los usuarios con bajos privilegios **pueden** acceder a las funciones administrativas mediante el acceso directo a la URL  
- [ ] Sí — las funciones administrativas **pueden** ser accedidas manipulando encabezados relacionados con roles, cookies o claims de JWT  

### ¿Se pueden modificar los roles o permisos de usuario a través de Mass Assignment o Parameter Pollution?
- [ ] No — los campos basados en roles son estrictamente de solo lectura y **no pueden** ser modificados por los usuarios  
- [ ] Sí — los permisos de usuario **pueden** elevarse incluyendo parámetros ocultos (por ejemplo, `role=admin`) en las actualizaciones de perfil o en el registro  

### ¿Se aplican los controles de autorización de manera consistente en todas las versiones de la API y métodos HTTP?
- [ ] Sí — los controles **se aplican** consistentemente en todas las versiones y métodos  
- [ ] No — las versiones heredadas de la API (por ejemplo, `/v1/`) o métodos HTTP específicos (por ejemplo, `PUT`, `DELETE`, `PATCH`) **evaden** la autorización  
- [ ] No — las funciones administrativas están ocultas en la interfaz de usuario pero están **habilitadas** a nivel de API para todos los usuarios  

---