## WSTG-SESS-07 — Testing Session Timeout

Las pruebas de tiempo de espera de la sesión (Session Timeout) evalúan si una aplicación termina de manera efectiva la sesión de un usuario tras un periodo predefinido de inactividad o una duración total. Este control es vital para mitigar el riesgo de secuestro de sesión (Session Hijacking), particularmente en estaciones de trabajo compartidas o en escenarios donde un atacante captura un identificador de sesión a través de la interceptación de red o XSS. Los Pentesters analizan tanto los tiempos de espera por inactividad (Idle Timeouts), que se activan tras un periodo de inactividad, como los tiempos de espera absolutos (Absolute Timeouts), que limitan la vida útil total de una sesión independientemente de la actividad. Desde la perspectiva de un atacante, las sesiones indefinidas o excesivamente largas proporcionan una ventana extendida de oportunidad para mantener el acceso no autorizado y evadir la necesidad de re-autenticación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### ¿Implementa la aplicación un tiempo de espera de sesión por inactividad (Idle Session Timeout)?
- [ ] Sí — la sesión expira tras un periodo corto (ej. 15-30 minutos) de inactividad  
- [ ] Sí — la sesión expira pero la duración del tiempo de espera es **excesivamente larga** (ej. > 24 horas)  
- [ ] No — la sesión **permanece activa** indefinidamente durante la inactividad  

### ¿Se aplica el tiempo de espera de la sesión en el lado del servidor?
- [ ] Sí — el servidor rechaza peticiones con tokens expirados independientemente del estado en el lado del cliente  
- [ ] No — el tiempo de espera **solo** se aplica a través de JavaScript en el lado del cliente o meta-refresh  

### ¿Implementa la aplicación un tiempo de espera de sesión absoluto (Absolute Session Timeout)?
- [ ] Sí — la sesión se termina tras una duración máxima fija independientemente de la actividad  
- [ ] No — la sesión **puede** extenderse indefinidamente mientras haya actividad continua  

### ¿Se invalida el identificador de sesión en el servidor al agotarse el tiempo de espera?
- [ ] Sí — el token de sesión **no puede** ser reutilizado una vez alcanzado el umbral de tiempo de espera  
- [ ] No — el token de sesión **sigue siendo válido** en el servidor incluso después de que el cliente es redirigido a la página de inicio de sesión  

### ¿Puede mantenerse la sesión activa indefinidamente mediante peticiones automatizadas de tipo heartbeat?
- [ ] No — el tiempo de espera absoluto o controles secundarios **previenen** la extensión infinita de la sesión  
- [ ] Sí — las peticiones periódicas en segundo plano (ej. AJAX) **pueden** mantener la sesión indefinidamente  

---