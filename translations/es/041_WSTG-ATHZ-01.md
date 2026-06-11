## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Las vulnerabilidades de Directory Traversal y File Inclusion surgen cuando una aplicación utiliza entradas controlables por el usuario para construir rutas a archivos o directorios sin una validación o sanitización suficiente. Los atacantes explotan estos fallos inyectando secuencias como `../` para navegar fuera del directorio previsto, accediendo potencialmente a archivos sensibles del sistema, datos de configuración o código fuente de la aplicación. En casos más graves que involucran Local File Inclusion (LFI) o Remote File Inclusion (RFI), un atacante puede lograr la ejecución remota de código (RCE) incluyendo scripts maliciosos o aprovechando el log poisoning y wrappers de PHP. Estas vulnerabilidades suelen residir en parámetros utilizados para la carga dinámica de contenido, motores de plantillas o endpoints de recuperación de imágenes donde la lógica del lado del servidor maneja rutas del sistema de archivos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Herramientas:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### ¿Aceptan los parámetros nombres de archivos o rutas para su procesamiento en el lado del servidor?
- [ ] No — ningún parámetro parece interactuar con el sistema de archivos  
- [ ] Sí — existen parámetros pero utilizan una lista de permitidos (allowlist) estricta de identificadores de archivos  
- [ ] Sí — los parámetros aceptan nombres de archivos o rutas directas  

### ¿Se sanitiza la entrada para prevenir secuencias de directory traversal?
- [ ] Sí — la entrada se valida contra una lista de permitidos estricta y las secuencias de salto de directorio **no son posibles**  
- [ ] Sí — la entrada se sanitiza eliminando secuencias `../`, pero un bypass recursivo **es posible**  
- [ ] No — no **se aplica** ninguna sanitización o validación a la entrada relacionada con rutas  

### ¿Es posible acceder a archivos fuera del directorio restringido mediante secuencias de salto de directorio?
- [ ] No — la aplicación o el SO impiden el acceso a archivos fuera del alcance definido  
- [ ] Sí — el acceso a archivos dentro de la raíz web **es posible**  
- [ ] Sí — el acceso a archivos sensibles del sistema (ej. `/etc/passwd`, `C:\Windows\win.ini`) **es posible** *(Crítica)*  

### ¿Permite el servidor la inclusión de URLs remotas (RFI)?
- [ ] No — la inclusión de archivos remotos está **deshabilitada** a nivel de servidor/aplicación  
- [ ] Sí — los archivos remotos **pueden** ser incluidos pero la ejecución **no es posible**  
- [ ] Sí — los archivos remotos **pueden** ser incluidos y ejecutados en el servidor *(Crítica)*  

### ¿Se pueden omitir (bypass) los filtros utilizando codificación o caracteres especiales?
- [ ] No — los filtros manejan eficazmente varias codificaciones y bytes nulos  
- [ ] Sí — el bypass **es posible** utilizando URL encoding, double URL encoding o Unicode de 16 bits  
- [ ] Sí — el bypass **es posible** utilizando inyección de byte nulo (`%00`) o wrappers del sistema de archivos (ej. `php://filter`)  

---