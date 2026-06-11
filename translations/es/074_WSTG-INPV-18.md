## WSTG-INPV-18 — Testing for Server-Side Template Injection (SSTI)

La Server-Side Template Injection (SSTI) ocurre cuando una aplicación incrusta entrada controlada por el usuario en un motor de plantillas del lado del servidor sin el saneamiento o escape de caracteres suficiente. Esto permite a un atacante inyectar directivas de plantilla maliciosas, que luego se ejecutan en el servidor, lo que potencialmente conduce al compromiso total del sistema a través de Remote Code Execution (RCE). La SSTI típicamente se manifiesta en funcionalidades como la generación dinámica de correos electrónicos, páginas de perfil y sistemas de notificación que utilizan motores como Jinja2, Twig o FreeMarker. Desde la perspectiva de un atacante, esta vulnerabilidad a menudo se explota para filtrar variables de entorno sensibles, leer archivos internos o ejecutar comandos del sistema aprovechando los objetos y métodos nativos del motor de plantillas.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Estado de la Prueba** | No realizada |
| **Severidad** | Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Herramientas:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### ¿Utiliza la aplicación un motor de plantillas del lado del servidor para procesar la entrada del usuario?
- [ ] No — la aplicación **no** utiliza plantillas del lado del servidor para contenido dinámico  
- [ ] Sí — se utilizan plantillas, pero la entrada del usuario **no** está incrustada en las directivas de la plantilla  
- [ ] Sí — la entrada del usuario está incrustada en las plantillas **sin** el saneamiento adecuado  

### ¿Se evalúan expresiones aritméticas básicas cuando se inyectan en los campos de entrada?
- [ ] No — los payloads como `{{7*7}}` o `${7*7}` se reflejan como cadenas literales  
- [ ] Sí — las expresiones se evalúan (p. ej., devolviendo `49`), lo que confirma que el motor de plantillas **es vulnerable**  

### ¿Se puede acceder a los objetos o métodos integrados del motor de plantillas?
- [ ] No — el acceso a objetos, métodos o configuraciones peligrosas está **restringido** o bajo un sandbox  
- [ ] Sí — se **puede** acceder a los objetos integrados (p. ej., `self`, `config`, `__class__`) para filtrar datos sensibles  
- [ ] Sí — el Remote Code Execution (RCE) mediante llamadas al sistema o librerías del SO **es posible**  

### ¿Existe un mecanismo de sandbox o lista de permitidos (allowlist) que impida la ejecución de directivas peligrosas?
- [ ] Sí — se ha implementado una lista de permitidos estricta o un sandbox seguro y el bypass **no es posible**  
- [ ] Sí — existe un sandbox, pero el bypass **es posible** mediante payloads específicos dependientes del motor  
- [ ] No — **no se aplica** ningún sandbox ni lista de permitidos al motor de plantillas  

---