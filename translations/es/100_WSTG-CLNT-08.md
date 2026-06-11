## WSTG-CLNT-08 — Testing for Cross Site Flashing

El Cross-Site Flashing (XSF) es una vulnerabilidad de lado del cliente que ocurre cuando un archivo Flash (SWF) maneja incorrectamente la entrada controlada por el usuario, permitiendo que un atacante ejecute código ActionScript malicioso o establezca un puente hacia el entorno JavaScript del navegador. Al manipular parámetros como `FlashVars` o cadenas de consulta de URL (query strings) pasados a sumideros (sinks) como `ExternalInterface.call`, `getURL` o `loadMovie`, un atacante puede realizar acciones en el contexto del dominio vulnerable, incluyendo el secuestro de sesión (session hijacking) y la exfiltración de datos. Esta vulnerabilidad se encuentra principalmente en aplicaciones empresariales heredadas (legacy) que aún alojan archivos SWF, donde la validación de entrada insuficiente permite que la película Flash sea reutilizada como un vector de Cross-Site Scripting (XSS) o para evadir la Same-Origin Policy (SOP) a través de configuraciones de dominio cruzado permisivas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### ¿Se alojan archivos heredados (legacy) de Adobe Flash (.swf) en la aplicación?
- [ ] No — no se alojan ni se referencian archivos Flash dentro del alcance de la aplicación  
- [ ] Sí — hay archivos SWF presentes pero solo sirven contenido estático  
- [ ] Sí — hay archivos SWF presentes y aceptan parámetros dinámicos a través de `FlashVars` o cadenas de URL  

### ¿Se sanitiza la entrada pasada a los sumideros (sinks) de ActionScript?
- [ ] Sí — todas las entradas son validadas estrictamente y los sumideros de ActionScript **no pueden** ser manipulados  
- [ ] Sí — se aplica validación pero la evasión (bypass) **es posible** mediante técnicas de codificación específicas  
- [ ] No — la entrada se pasa directamente a sumideros sensibles como `ExternalInterface.call` o `getURL` *(Crítico)*  

### ¿Puede el archivo Flash ser utilizado para ejecutar JavaScript arbitrario (XSS)?
- [ ] No — `ExternalInterface` está **deshabilitado** o el parámetro `allowScriptAccess` está establecido en `never`  
- [ ] Sí — `allowScriptAccess` está establecido en `sameDomain` pero el SWF está alojado en el dominio de destino  
- [ ] Sí — `allowScriptAccess` está establecido en `always`, permitiendo XSS desde cualquier dominio  

### ¿Previene la política `crossdomain.xml` las solicitudes de origen cruzado no autorizadas?
- [ ] Sí — `crossdomain.xml` es restrictivo y solo permite orígenes específicos y confiables  
- [ ] No — el archivo `crossdomain.xml` **no existe**  
- [ ] Sí — la política es excesivamente permisiva (ej., `<allow-access-from domain="*" />`)  

### ¿Se puede forzar al archivo SWF a cargar películas externas controladas por un atacante?
- [ ] No — la aplicación **no puede** ser forzada a cargar archivos SWF externos  
- [ ] Sí — las funciones `loadMovie` o `loadMovieNum` aceptan URLs externas no validadas  

---