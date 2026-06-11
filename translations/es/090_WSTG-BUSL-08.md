## WSTG-BUSL-08 — Prueba de Carga de Tipos de Archivos No Esperados

La carga de tipos de archivos no esperados permite a los atacantes omitir las restricciones de la lógica de negocio al enviar archivos que la aplicación no tiene la intención de procesar, lo que potencialmente conduce a la ejecución remota de código (Remote Code Execution), Cross-Site Scripting (XSS) o denegación de servicio (DoS). Los atacantes sondean estas vulnerabilidades modificando las extensiones de archivo, los tipos MIME y los magic bytes para engañar a la lógica de validación del lado del servidor y lograr que acepte contenido malicioso. Esto ocurre típicamente en la carga de fotos de perfil, sistemas de gestión documental o archivos adjuntos en tickets de soporte donde existe una validación insuficiente en el back-end. Una explotación exitosa puede comprometer el servidor host si el archivo cargado se ejecuta, o derivar en ataques del lado del cliente si el archivo se sirve a otros usuarios con un Content-Type incorrecto.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### ¿Restringe la aplicación la carga de archivos a un conjunto específico de extensiones?
- [ ] Sí — se aplica una lista blanca (whitelist) estricta de extensiones y el bypass **no es posible**  
- [ ] Sí — la lista blanca de extensiones está presente pero el bypass **es posible** mediante extensiones dobles o null bytes  
- [ ] No — **se puede** cargar cualquier extensión de archivo  

### ¿Se valida el contenido del archivo más allá de la extensión de archivo?
- [ ] Sí — la lógica del lado del servidor verifica los Magic Bytes y realiza una inspección profunda del contenido  
- [ ] Sí — la lógica del lado del servidor verifica el tipo MIME únicamente a través de la cabecera `Content-Type`  
- [ ] No — **no se aplica** ninguna validación de contenido después de la comprobación de la extensión  

### ¿Puede un atacante ejecutar código mediante la carga de una web shell o script?
- [ ] No — los archivos cargados se almacenan en un directorio no ejecutable o en un bucket de almacenamiento (S3/Azure Blob)  
- [ ] Sí — los archivos se almacenan en un directorio accesible vía web pero la ejecución **está deshabilitada** mediante la configuración del servidor  
- [ ] Sí — los scripts maliciosos cargados **pueden** ejecutarse directamente a través de una ruta URL predecible *(Crítica)*  

### ¿Son posibles ataques del lado del cliente como XSS a través de los tipos de archivos cargados?
- [ ] No — los archivos se sirven con `Content-Disposition: attachment` y `X-Content-Type-Options: nosniff`  
- [ ] Sí — los archivos SVG o HTML **pueden** cargarse y renderizarse en el navegador, lo que deriva en XSS  
- [ ] Sí — los metadatos de la imagen (EXIF) **se procesan** y se reflejan en la interfaz de usuario sin sanitización  

---