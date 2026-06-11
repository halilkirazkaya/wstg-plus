## WSTG-CONF-04 — Revisión de archivos de respaldo antiguos y no referenciados en busca de información sensible

La revisión de archivos de respaldo antiguos y archivos no referenciados consiste en identificar archivos olvidados, temporales o ocultos en un servidor web que no están destinados al acceso público. Estos archivos suelen incluir copias de seguridad del código fuente (`.zip`, `.bak`), archivos de intercambio de editores de texto (`.swp`, `~`) o metadatos de control de versiones (`.git`, `.svn`) que pueden filtrar información sensible. Los atacantes utilizan wordlists automatizadas y herramientas de descubrimiento para realizar Brute Force sobre convenciones de nomenclatura comunes, con el objetivo de exfiltrar credenciales de bases de datos, API keys hardcoded o lógica que facilite un Exploit posterior. Esta vulnerabilidad suele originarse por un mantenimiento manual del servidor o por pipelines de CI/CD defectuosos que no logran sanear el web root de producción.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Media / Alta* |

> *La severidad se vuelve Alta si se descubren archivos de configuración, archivos de código fuente o credenciales.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### ¿Están habilitados los listados de directorios en el servidor web?
- [ ] No — el listado de directorios está **deshabilitado** y devuelve un 403 Forbidden o un error personalizado  
- [ ] Sí — el listado de directorios está **habilitado** en directorios no sensibles  
- [ ] Sí — el listado de directorios está **habilitado** en directorios que contienen código fuente o archivos sensibles *(Crítico)*  

### ¿Son descubribles los archivos de respaldo con extensiones comunes (ej., .bak, .old, .save)?
- [ ] No — no se **encuentran** extensiones de respaldo comunes o están bloqueadas por la configuración del servidor  
- [ ] Sí — existen archivos de respaldo pero **no** contienen información sensible  
- [ ] Sí — los archivos de respaldo de configuración o código fuente **son** accesibles *(Alta)*  

### ¿Están expuestos los directorios de control de versiones (ej., .git, .svn) o sus metadatos?
- [ ] No — los metadatos de control de versiones **no están presentes** o están correctamente bloqueados  
- [ ] Sí — existen metadatos pero el repositorio completo **no puede** ser reconstruido  
- [ ] Sí — el repositorio de código fuente completo **puede** ser exfiltrado a través de los metadatos expuestos  

### ¿Existen archivos comprimidos (ej., .zip, .tar.gz, .rar) de la aplicación en el web root?
- [ ] No — no se descubrieron archivos comprimidos sensibles mediante Brute Force o enumeración  
- [ ] Sí — se encontraron archivos comprimidos pero están protegidos por contraseña o contienen activos públicos  
- [ ] Sí — los archivos comprimidos sin cifrar que contienen el código fuente o datos de la aplicación **son** accesibles públicamente  

### ¿La aplicación filtra archivos temporales creados por editores de texto o IDEs?
- [ ] No — los archivos temporales como `.swp`, `~` o `.DS_Store` **no están presentes**  
- [ ] Sí — archivos temporales no sensibles están **presentes**  
- [ ] Sí — los archivos temporales revelan fragmentos de código fuente o rutas internas  

---