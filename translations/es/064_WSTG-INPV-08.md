## WSTG-INPV-08 — Pruebas de Inyección SSI (SSI Injection)

La Inyección de Server-Side Includes (SSI Injection) ocurre cuando una aplicación falla al desinfectar la entrada del usuario antes de incorporarla en archivos HTML procesados por el motor SSI del servidor. Los atacantes explotan esto inyectando directivas SSI, como `<!--#exec cmd="id" -->`, para ejecutar comandos de shell arbitrarios, leer archivos locales o exfiltrar variables de entorno. Esta vulnerabilidad se encuentra típicamente en aplicaciones que utilizan extensiones de archivo heredadas como `.shtml`, `.shtm` o `.stm`, o donde el servidor web está configurado explícitamente para procesar SSI dentro de archivos HTML estándar. Una explotación exitosa otorga al atacante los mismos permisos que el proceso del servidor web, lo que a menudo conduce a un compromiso total del sistema o a la exposición de datos sensibles.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### ¿El servidor web admite y procesa directivas SSI?
- [ ] No — el servidor **no** admite SSI o las extensiones de archivo como `.shtml` están **deshabilitadas**  
- [ ] Sí — SSI **está habilitado** pero la entrada del usuario **no** se refleja en las páginas procesadas  
- [ ] Sí — SSI **está habilitado** y la entrada del usuario **se** refleja en las páginas procesadas  

### ¿Se pueden ejecutar comandos de sistema arbitrarios a través de la directiva `#exec`?
- [ ] No — la directiva `#exec` está **deshabilitada** o restringida por la configuración del servidor  
- [ ] Sí — la ejecución de comandos **es posible** a través de payloads de `#exec cmd` o `#exec cgi`  

### ¿Es posible la exfiltración de archivos sensibles o variables de entorno?
- [ ] No — las directivas como `#include` o `#config` están **deshabilitadas**  
- [ ] Sí — la lectura de archivos locales (p. ej., `/etc/passwd`) **es posible** a través de `#include virtual`  
- [ ] Sí — las variables de entorno del servidor **pueden** ser exfiltradas a través de `#printenv` o `#echo`  

### ¿Son efectivos los controles de validación de entrada o WAF contra los payloads de SSI?
- [ ] Sí — se **aplica** una validación de entrada estricta y el bypass **no es posible**  
- [ ] Sí — se **aplica** validación/WAF pero el bypass **es posible** utilizando codificación de caracteres o una sintaxis SSI diferente  
- [ ] No — no existe validación ni protección WAF para los caracteres de SSI (`<`, `!`, `#`, `-`, `"`)  

---