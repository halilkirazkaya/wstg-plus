## WSTG-SESS-09 — Pruebas de secuestro de sesión (Session Hijacking)

El secuestro de sesión (Session Hijacking) ocurre cuando un atacante captura, predice o fija un identificador de sesión (SID) válido para obtener acceso no autorizado a la sesión activa de un usuario. Esta vulnerabilidad se manifiesta típicamente a través de una seguridad insuficiente en la capa de transporte, la falta de banderas de seguridad en las cookies o algoritmos de generación de SID predecibles que permiten a un atacante omitir la autenticación por completo. Desde la perspectiva de un atacante, la explotación exitosa permite la suplantación total de la víctima sin requerir sus credenciales, lo cual se logra a menudo mediante el rastreo de red (sniffing), Cross-Site Scripting (XSS) o ataques de fijación de sesión (session fixation).


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Herramientas:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### ¿Están los identificadores de sesión protegidos durante el tránsito?
- [ ] Sí — La bandera `Secure` está **habilitada** y HSTS se aplica estrictamente  
- [ ] Sí — La bandera `Secure` está **habilitada** pero HSTS está **deshabilitado**  
- [ ] No — La bandera `Secure` **no se aplica**, permitiendo la interceptación del SID a través de canales no cifrados *(Alta)*  

### ¿Está el identificador de sesión protegido contra el acceso por scripts del lado del cliente?
- [ ] Sí — La bandera `HttpOnly` se **aplica** a todas las cookies relacionadas con la sesión  
- [ ] No — La bandera `HttpOnly` **no se aplica**, permitiendo la exfiltración del SID vía XSS *(Crítica)*  

### ¿Implementa la aplicación el amarre de sesión (session binding) a atributos del cliente?
- [ ] Sí — La sesión está vinculada a atributos del cliente (IP/User-Agent) y su reutilización **no es posible**  
- [ ] Sí — El amarre de sesión está presente pero su omisión **es posible** mediante la suplantación de cabeceras (header spoofing)  
- [ ] No — No existe amarre de sesión; el SID **es válido** cuando se utiliza desde cualquier origen  

### ¿Es el identificador de sesión lo suficientemente aleatorio para prevenir la predicción?
- [ ] Sí — El SID utiliza un generador de números pseudoaleatorios criptográficamente seguro (CSPRNG)  
- [ ] No — La longitud del SID es insuficiente o sigue una secuencia/patrón **predecible**  

### ¿Se gestionan las sesiones concurrentes para limitar la ventana de ataque?
- [ ] No — La aplicación impone estrictamente una única sesión activa por usuario  
- [ ] Sí — Las sesiones concurrentes múltiples están **habilitadas** pero son monitoreadas  
- [ ] Sí — Las sesiones concurrentes ilimitadas **son posibles** a través de diferentes dispositivos/ubicaciones  

---