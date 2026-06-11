## WSTG-SESS-11 — Testing for Concurrent Sessions

Las pruebas de sesiones concurrentes (Concurrent Sessions) evalúan si una aplicación permite que una única cuenta de usuario mantenga múltiples sesiones activas simultáneas en diferentes navegadores, dispositivos o direcciones IP. La falta de restricción de las sesiones concurrentes aumenta la ventana de oportunidad para que los atacantes utilicen tokens de sesión secuestrados (Session Hijacking) o credenciales comprometidas sin alertar al usuario legítimo ni finalizar las sesiones existentes. Este comportamiento se evalúa normalmente autenticándose desde múltiples fuentes y monitoreando la estabilidad de la sesión, lo que a menudo revela debilidades en la lógica de Session Management o una falta de reglas de negocio centradas en la seguridad. Desde la perspectiva de un atacante, la falta de control de concurrencia permite una persistencia sigilosa tras un compromiso inicial y facilita ataques paralelos automatizados.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### ¿Están limitadas las sesiones concurrentes por la política de la aplicación?
- [ ] Sí — solo **se permite** una sesión por usuario; los nuevos inicios de sesión invalidan los anteriores *(Más seguro)*  
- [ ] Sí — **se impone** un número fijo de sesiones concurrentes y no se puede exceder  
- [ ] No — **es posible** un número ilimitado de sesiones concurrentes  

### ¿Cómo responde la aplicación cuando se alcanza el límite de sesiones?
- [ ] La sesión activa más antigua **se finaliza automáticamente** (mitigación de session fixation/hijacking)  
- [ ] **Se solicita** al usuario que finalice las sesiones existentes antes de que se autorice la nueva sesión  
- [ ] El nuevo intento de inicio de sesión **se bloquea** hasta que se produzca un logout manual desde otro dispositivo  
- [ ] No se toma ninguna medida — múltiples sesiones permanecen **habilitadas** simultáneamente  

### ¿Proporciona la aplicación una interfaz de gestión de sesiones para el usuario?
- [ ] Sí — los usuarios **pueden** ver las sesiones activas (IP, dispositivo, hora) y finalizarlas de forma remota  
- [ ] Sí — los usuarios **pueden** ver las sesiones activas pero **no pueden** finalizarlas de forma remota  
- [ ] No — los usuarios **no pueden** ver ni gestionar otras sesiones activas asociadas a su cuenta  

### ¿Se notifica al usuario cuando se detecta actividad de inicio de sesión concurrente?
- [ ] Sí — **se activa** una alerta (correo electrónico, SMS o in-app) para inicios de sesión desde nuevos dispositivos o ubicaciones  
- [ ] No — **no se proporciona** ninguna notificación cuando se establecen sesiones concurrentes  

---