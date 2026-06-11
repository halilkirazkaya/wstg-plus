## WSTG-INPV-11 — Code Injection

La inyección de código (Code Injection) ocurre cuando una aplicación evalúa fragmentos de código suministrados por el usuario dentro de su entorno de ejecución, típicamente a través de funciones inseguras específicas del lenguaje como `eval()`, `exec()` o `system()`. Esta vulnerabilidad se diferencia de la inyección de comandos (Command Injection) porque se dirige al contexto de ejecución del lenguaje de programación (p. ej., PHP, Python, Node.js) en lugar de a la shell del sistema operativo subyacente. Los atacantes explotan estos puntos para ejecutar lógica arbitraria, exfiltrar variables de entorno o lograr una ejecución remota de código (RCE) completa en el servidor. Los Pentesters deben investigar funcionalidades que procesen expresiones matemáticas, plantillas del lado del servidor (Server-Side Templates) o parámetros de configuración dinámica donde la entrada pueda ser interpretada como código ejecutable.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### ¿Existen funcionalidades de la aplicación que evalúen entradas dinámicas a través de funciones de evaluación específicas del lenguaje?
- [ ] No — no existen funciones de evaluación dinámica presentes en el código base  
- [ ] Sí — se utilizan funciones como `eval()`, `exec()` o `include()`, pero **no** aceptan entradas del usuario  
- [ ] Sí — se utilizan funciones y estas procesan entradas suministradas por el usuario  

### ¿Se aplican mecanismos de validación o sanitización a las entradas antes de su evaluación?
- [ ] Sí — la entrada se valida estrictamente contra una **lista blanca (whitelist)** estática  
- [ ] Sí — la entrada se sanitiza mediante listas negras (blacklisting), pero es posible realizar un **bypass**  
- [ ] No — la entrada se pasa directamente a la función de evaluación y **no se aplican controles**  

### ¿El entorno de ejecución está en un sandbox o restringido para evitar el acceso a nivel de sistema?
- [ ] Sí — la ejecución ocurre en un sandbox altamente restringido donde **no** se pueden realizar llamadas al sistema (system calls)  
- [ ] Sí — existen algunas restricciones, pero es posible realizar un **escape del sandbox**  
- [ ] No — el entorno permite acceso total a las APIs del lenguaje subyacente y a las funciones del SO *(Crítica)*  

### ¿Puede utilizarse la inyección de código para exfiltrar información sensible?
- [ ] No — la ejecución es ciega (blind) y **no** es posible la comunicación fuera de banda (out-of-band)  
- [ ] Sí — las variables de entorno o los archivos locales **pueden** ser exfiltrados a través de peticiones HTTP/DNS  
- [ ] Sí — los resultados del código ejecutado se **reflejan directamente** en la respuesta de la aplicación  

### ¿Permite la aplicación la inclusión remota de archivos o la carga dinámica de librerías?
- [ ] No — solo se pueden ejecutar rutas de código locales y predefinidas  
- [ ] Sí — la aplicación **puede** ser forzada a cargar y ejecutar código desde una fuente remota o controlada por el atacante  

---