## WSTG-BUSL-09 — Prueba de subida de archivos maliciosos

La funcionalidad de subida de archivos permite a los usuarios enviar datos al servidor, proporcionando un vector directo para introducir contenido malicioso en el entorno de la aplicación. Los atacantes explotan lógicas de validación débiles para subir web shells, malware o archivos especialmente diseñados —como SVG o HTML— para lograr la ejecución remota de comandos (Remote Code Execution (RCE)) o Cross-Site Scripting (XSS). Esta vulnerabilidad se encuentra típicamente en funciones como la configuración de perfiles, sistemas de gestión documental y gestores de archivos adjuntos, donde el servidor no verifica correctamente los tipos de archivo, el contenido y los permisos de ejecución. Una explotación exitosa a menudo conduce al compromiso total del sistema, la exfiltración de datos o el uso del servidor como punto de pivote para ataques a la red interna.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### ¿Proporciona la aplicación funcionalidad para la subida de archivos?
- [ ] No — no existe funcionalidad de subida de archivos  
- [ ] Sí — existe funcionalidad para usuarios autenticados  
- [ ] Sí — la funcionalidad está disponible para usuarios **no autenticados** *(Crítico)*  

### ¿Se aplica la validación de extensiones de archivo en el lado del servidor?
- [ ] Sí — se **aplica** una lista blanca (allowlist) estricta de extensiones y el bypass **no es posible**  
- [ ] Sí — se utiliza una allowlist pero el bypass **es posible** mediante distinción entre mayúsculas y minúsculas o extensiones dobles  
- [ ] Sí — se utiliza una lista negra (blocklist), la cual **puede** ser evadida con extensiones alternativas (ej. `.phtml`, `.asa`)  
- [ ] No — no se **aplica** ninguna validación de extensión en el lado del servidor  

### ¿Se valida el contenido del archivo más allá de la extensión o el tipo MIME?
- [ ] Sí — la aplicación verifica los magic bytes y realiza una inspección profunda del contenido  
- [ ] Sí — la aplicación solo comprueba la cabecera `Content-Type`, la cual **es** fácilmente falsificada (spoofed)  
- [ ] No — la aplicación confía totalmente en la extensión del archivo para la validación  

### ¿Se almacenan los archivos subidos en un directorio con permisos de ejecución?
- [ ] No — los archivos se almacenan en un servicio de almacenamiento dedicado (ej. S3) o en un directorio no ejecutable  
- [ ] Sí — los archivos se almacenan en el servidor web pero la ejecución está **deshabilitada** mediante configuración  
- [ ] Sí — los archivos se almacenan en un directorio accesible vía web y la ejecución **está habilitada** *(Crítico)*  

### ¿Se pueden evadir los filtros de seguridad mediante la manipulación del nombre del archivo?
- [ ] No — los nombres de archivo son saneados o regenerados aleatoriamente por el servidor  
- [ ] Sí — los filtros **pueden** ser evadidos utilizando bytes nulos (null bytes) (ej. `shell.php%00.jpg`)  
- [ ] Sí — se **pueden** utilizar caracteres de path traversal (ej. `../../`) para subir archivos a directorios arbitrarios  

---