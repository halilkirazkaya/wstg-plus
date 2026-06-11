## WSTG-CONF-09 — Prueba de Permisos de Archivos

Las pruebas de permisos de archivos consisten en auditar los niveles de control de acceso asignados a los archivos y directorios en el servidor web para garantizar que los recursos sensibles no estén sobreexpuestos a usuarios o procesos no autorizados. Los permisos configurados incorrectamente pueden permitir que los atacantes lean archivos de configuración sensibles que contienen credenciales de bases de datos, visualicen código fuente propietario o modifiquen scripts del lado del servidor para lograr una ejecución remota de comandos (Remote Code Execution). Esta vulnerabilidad ocurre típicamente durante la fase de despliegue cuando se dejan habilitados los indicadores de "lectura global" (world-readable) o "escritura global" (world-writable) en directorios críticos de la aplicación, como rutas de configuración, carpetas de registros o repositorios de carga de archivos. Desde la perspectiva de un atacante, el descubrimiento de un archivo asegurado incorrectamente suele servir como el catalizador principal para el escalamiento de privilegios (Privilege Escalation) o el compromiso total del sistema.

| Campo | Valor |
|---|---|
| **ID WSTG** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta* |

> *La severidad se vuelve Alta si los archivos de configuración sensibles (p. ej., `.env`, `web.config`) son legibles o si el acceso de escritura a la raíz web (web root) es posible.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Herramientas:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### ¿Están protegidos los archivos de configuración sensibles de la aplicación contra el acceso no autorizado?
- [ ] Sí — los archivos de configuración (p. ej., `.env`, `config.php`) **no son accesibles** mediante peticiones web  
- [ ] Sí — los archivos están presentes pero el acceso está **restringido** solo a usuarios locales autorizados  
- [ ] No — los archivos de configuración sensibles **son accesibles** y exponen credenciales o secretos  

### ¿Puede un atacante modificar archivos o directorios dentro de la raíz web?
- [ ] No — todos los directorios de la raíz web son de **solo lectura** para el usuario del servidor web  
- [ ] Sí — existen directorios con permisos de escritura pero la ejecución de scripts está **deshabilitada**  
- [ ] Sí — existen directorios con permisos de escritura global (world-writable) que **permiten** la carga y ejecución de scripts *(Crítico)*  

### ¿Están expuestos metadatos de control de versiones o archivos de respaldo debido a permisos laxos?
- [ ] No — `.git`, `.svn` y archivos de respaldo (p. ej., `.bak`, `~`) **no están presentes** o están protegidos  
- [ ] Sí — existen directorios de metadatos pero el listado de directorios (directory listing) está **deshabilitado**  
- [ ] Sí — los metadatos sensibles o respaldos son **completamente accesibles**, lo que permite la recuperación del código fuente  

### ¿Opera el usuario del servidor web bajo el principio de menor privilegio?
- [ ] Sí — el proceso del servidor web se ejecuta como un **usuario dedicado de bajos privilegios**  
- [ ] No — el proceso del servidor web se ejecuta con **privilegios innecesarios** (p. ej., `root` o `Administrator`)  

### ¿Están los archivos de registro (log files) protegidos contra lectura o modificación no autorizada?
- [ ] Sí — los archivos de registro están **restringidos** a usuarios administrativos  
- [ ] Sí — los archivos de registro son legibles pero **no** contienen tokens de sesión o PII (información de identificación personal)  
- [ ] No — los archivos de registro son **públicamente legibles** y exponen datos sensibles de transacciones o información de usuarios  

---