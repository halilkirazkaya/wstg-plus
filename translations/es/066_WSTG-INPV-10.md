## WSTG-INPV-10 — Inyección IMAP SMTP

Las vulnerabilidades de inyección IMAP y SMTP surgen cuando una aplicación web filtra incorrectamente los datos proporcionados por el usuario antes de incorporarlos en los comandos enviados a un servidor de correo. Al inyectar caracteres de Retorno de Carro y Salto de Línea (CRLF), un atacante puede finalizar el comando previsto y añadir instrucciones de correo arbitrarias, como agregar destinatarios adicionales (CC/BCC) o modificar el cuerpo del mensaje. Estos fallos se encuentran con mayor frecuencia en pasarelas de web-a-correo, formularios de contacto y clientes de correo electrónico que se comunican directamente con los servidores de correo a través de protocolos como IMAP4 o SMTP. La explotación exitosa permite el reenvío no autorizado de spam, la exfiltración de contenidos de correo electrónico sensibles y, en algunos casos, el compromiso total de la integridad del servidor de correo.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### ¿Procesa la aplicación la entrada proporcionada por el usuario para construir comandos IMAP o SMTP?
- [ ] No — la aplicación **no** interactúa con servidores de correo mediante la entrada del usuario  
- [ ] Sí — la aplicación interactúa con servidores de correo pero utiliza una API o librería segura  
- [ ] Sí — la aplicación construye comandos de correo en bruto utilizando parámetros controlados por el usuario  

### ¿Se valida la entrada para prevenir la inyección de secuencias de Retorno de Carro (`\r`) y Salto de Línea (`\n`)?
- [ ] Sí — los caracteres CRLF son filtrados, codificados o rechazados **estrictamente**  
- [ ] Sí — la entrada se valida contra una lista de permitidos (allowlist) estricta de caracteres  
- [ ] No — los caracteres CRLF **no** se filtran, lo que permite la terminación e inyección de comandos  

### ¿Puede un atacante inyectar cabeceras de correo adicionales como `Bcc:` o `Cc:`?
- [ ] No — la modificación de cabeceras **no es posible** debido a restricciones estrictas de entrada  
- [ ] Sí — la inyección de cabeceras adicionales **es posible** mediante secuencias CRLF  
- [ ] Sí — el reenvío de correo (mail relaying) a dominios externos **está habilitado** y es explotable *(Crítico)*  

### ¿Permite la aplicación la manipulación de comandos IMAP para acceder a buzones de correo no autorizados?
- [ ] No — la aplicación **no** utiliza IMAP o limita estrictamente el acceso al buzón al usuario autenticado  
- [ ] Sí — la inyección de comandos IMAP **es posible** pero se limita a la enumeración de metadatos  
- [ ] Sí — la inyección de comandos IMAP permite la lectura o eliminación de correos electrónicos de otros usuarios  

### ¿Es el servidor de correo backend vulnerable a command pipelining?
- [ ] No — el servidor de correo rechaza comandos por lotes o múltiples comandos en una sola sesión  
- [ ] Sí — el servidor de correo procesa múltiples comandos inyectados a través de un único campo de entrada  

---