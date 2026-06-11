## WSTG-INPV-12 — Inyección de Comandos (Command Injection)

La inyección de comandos (Command Injection) ocurre cuando una aplicación pasa datos no seguros suministrados por el usuario, como entradas de formularios, encabezados HTTP o cookies, a un shell del sistema. Esta vulnerabilidad permite a un atacante ejecutar comandos arbitrarios del sistema operativo en el servidor, generalmente con los privilegios del proceso de la aplicación web. Desde la perspectiva de un atacante, esto se explota mediante el uso de metacaracteres de shell como puntos y coma, tuberías o comillas invertidas para encadenar comandos maliciosos al flujo de ejecución previsto. El impacto es frecuentemente catastrófico, lo que lleva al compromiso total del sistema, la filtración de datos no autorizada o el movimiento lateral dentro de la red interna.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Herramientas:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### ¿La aplicación invoca comandos a nivel de sistema o ejecutables de shell?
- [ ] No — la aplicación **no** interactúa con el shell del sistema operativo  
- [ ] Sí — la aplicación utiliza APIs internas o librerías de alto nivel para tareas del sistema  
- [ ] Sí — la aplicación invoca directamente comandos de shell a través de llamadas al sistema como `system()`, `exec()` o `popen()`  

### ¿Se valida o sanea la entrada del usuario antes de ser pasada a las llamadas al sistema?
- [ ] Sí — **se aplica** una validación de entrada y listas de permitidos (allow-listing) estrictas  
- [ ] Sí — se utiliza el saneamiento o el escape (escaping) pero el bypass **es posible**  
- [ ] No — la entrada del usuario se pasa directamente al shell **sin** validación  

### ¿Se pueden utilizar metacaracteres de shell para manipular el flujo de ejecución de comandos?
- [ ] No — los metacaracteres se filtran o escapan correctamente  
- [ ] Sí — la ejecución in-band, donde la salida del comando se refleja en la respuesta, **es posible**  
- [ ] Sí — la ejecución ciega (blind), donde no se refleja ninguna salida, **es posible**  

### ¿Es posible la exfiltración fuera de banda (OOB) desde el host?
- [ ] No — el servidor no tiene salida (egress) o las señales OOB (DNS/HTTP) fallan  
- [ ] Sí — la exfiltración de datos a través de señales OOB basadas en DNS o HTTP **es posible**  

### ¿El proceso de la aplicación se ejecuta con privilegios de sistema elevados?
- [ ] No — la aplicación se ejecuta con una cuenta de servicio dedicada de bajos privilegios  
- [ ] Sí — la aplicación se ejecuta como `root`, `Administrator` o un usuario de altos privilegios *(Crítica)*  

---