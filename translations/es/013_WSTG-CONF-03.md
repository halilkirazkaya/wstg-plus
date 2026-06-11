## WSTG-CONF-03 — Prueba del Manejo de Extensiones de Archivos para Información Sensible

Las pruebas de manejo de extensiones de archivos consisten en identificar si el servidor web o el servidor de aplicaciones revela información sensible al servir archivos que deberían estar restringidos o ser ejecutados. Los atacantes buscan frecuentemente archivos de respaldo (backup), archivos de configuración y fragmentos de código fuente (por ejemplo, `.bak`, `.old`, `.env`, `.inc`) que puedan haber quedado en la raíz web y se sirvan como texto plano debido a la falta de mapeos de controladores específicos. Esta vulnerabilidad es relevante porque puede conducir a la exposición de credenciales de bases de datos, API keys y lógica de negocio interna, facilitando una explotación más profunda de la infraestructura. Estas exposiciones suelen ocurrir en directorios públicos, carpetas de subida o a través de configuraciones incorrectas de MIME-type en el servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si los archivos de configuración que contienen credenciales o el código fuente completo son accesibles.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### ¿Son accesibles las extensiones de archivos temporales o de respaldo sensibles (por ejemplo, `.bak`, `.old`, `.swp`, `~`)?
- [ ] No — el servidor devuelve 403 Forbidden o 404 Not Found para las extensiones de respaldo comunes  
- [ ] Sí — el servidor permite la descarga de archivos de respaldo, pero estos **no** contienen datos sensibles  
- [ ] Sí — el servidor permite la descarga de archivos de respaldo que contienen datos sensibles o código fuente *(Alta)*  

### ¿Expone el servidor archivos de configuración del sistema o del entorno (por ejemplo, `.env`, `.config`, `.yml`, `.ini`)?
- [ ] No — los archivos de configuración restringidos **no** pueden ser accedidos y devuelven 403/404  
- [ ] Sí — los archivos de configuración sensibles **son** accesibles y revelan secretos internos o credenciales  

### ¿Se puede recuperar el código fuente añadiendo extensiones alternativas (por ejemplo, `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] No — el código de la aplicación **no** se renderiza como texto plano independientemente de la manipulación de la extensión  
- [ ] Sí — el código fuente es revelado porque el servidor **no** tiene un handler para la extensión añadida  

### ¿Están bloqueados para el acceso público los directorios administrativos o de metadatos (por ejemplo, `.git/`, `.svn/`, `.DS_Store`)?
- [ ] No — estos directorios/archivos **no** existen en la raíz web  
- [ ] Sí — el acceso **no es posible** debido a reglas de rewrite del lado del servidor o permisos  
- [ ] Sí — los directorios de metadatos **en efecto** son accesibles y permiten la reconstrucción completa del código fuente  

### ¿Cómo maneja el servidor las solicitudes de archivos con múltiples extensiones (por ejemplo, `file.php.jpg`)?
- [ ] No — el servidor prioriza correctamente la seguridad de la extensión interna o bloquea la solicitud  
- [ ] Sí — el servidor procesa el archivo como la primera extensión, pero el bypass **no es posible** para la ejecución  
- [ ] Sí — el servidor ignora la extensión final, permitiendo que la ejecución de código o la divulgación de código fuente **sea posible**  

---