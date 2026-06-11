## WSTG-IDNT-01 — Test Role Definitions

La prueba de definiciones de roles consiste en identificar y documentar los diversos roles de usuario y niveles de permiso definidos dentro de la aplicación para asegurar que estén separados lógicamente y cumplan con el principio de **least privilege** (mínimo privilegio). Un atacante analiza la aplicación para mapear los roles de alto privilegio frente a los roles de usuario estándar, buscando cualquier ambigüedad en los límites de los permisos o funciones administrativas no documentadas. La identificación de estos roles es el primer paso para descubrir rutas de **Privilege Escalation** (escalada de privilegios), ya que las inconsistencias en las definiciones de roles a menudo conducen al acceso no autorizado a datos sensibles o funciones administrativas. Esta fase ocurre típicamente durante el reconocimiento inicial y el mapeo de la lógica de negocio, centrándose en los perfiles de usuario, dashboards administrativos y estructuras de permisos de **API**.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### ¿Están los roles claramente definidos y documentados en la aplicación o en su documentación?
- [ ] Sí — los roles están completamente documentados y siguen el principio de **least privilege**  
- [ ] Sí — los roles están documentados pero contienen **permisos superpuestos**  
- [ ] No — no existe documentación formal ni definiciones de roles  

### ¿Existe una separación clara entre las funciones administrativas y las de usuario estándar?
- [ ] Sí — las funciones administrativas están **completamente aisladas** de las vistas de usuario estándar  
- [ ] Sí — existe separación, pero los **endpoints** administrativos son **predecibles o adivinables**  
- [ ] No — las funciones administrativas y estándar están **mezcladas** dentro de las mismas interfaces  

### ¿Utiliza la aplicación un mecanismo centralizado para el Control de Acceso Basado en Roles (RBAC)?
- [ ] Sí — se ha **implementado** un **RBAC** o **ABAC** centralizado y se aplica de manera consistente  
- [ ] Sí — existen controles descentralizados pero se aplican de forma **inconsistente** en los módulos  
- [ ] No — la lógica de control de acceso está **hardcoded** en páginas o componentes individuales  

### ¿Existen roles no documentados u ocultos presentes en el sistema?
- [ ] No — todos los roles descubiertos coinciden con la funcionalidad documentada  
- [ ] Sí — **existen** roles ocultos o "en la sombra" (por ejemplo, `super_admin`, `support`, `test`)  

### ¿Puede un usuario con bajos privilegios enumerar los permisos o roles de otros usuarios?
- [ ] No — los usuarios **no pueden** ver ni enumerar los roles/permisos de otras entidades  
- [ ] Sí — los usuarios **pueden** enumerar roles a través de páginas de perfil, metadatos o respuestas de la **API** *(Media)*  

---