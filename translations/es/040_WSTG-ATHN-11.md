## WSTG-ATHN-11 — Pruebas de Autenticación de Múltiple Factor (MFA)

Las pruebas de autenticación de múltiple factor (MFA) evalúan la robustez de la capa de seguridad secundaria diseñada para prevenir el acceso no autorizado incluso cuando las credenciales primarias están comprometidas. Los atacantes intentan omitir el MFA con frecuencia identificando puntos finales (endpoints) donde no se aplica de manera consistente, como versiones heredadas (legacy) de API, backends móviles o flujos de trabajo de restablecimiento de contraseña. La explotación a menudo implica la manipulación de respuestas del servidor, el ataque de fuerza bruta (Brute Force) a códigos de corta duración (OTP) o la explotación de condiciones de carrera (race conditions) y fallos en la gestión de sesiones que permiten al usuario pasar a un estado autenticado sin proporcionar el segundo factor.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### ¿Se aplica el MFA de manera consistente en todos los portales de autenticación?
- [ ] Sí — El MFA es obligatorio para todos los intentos de inicio de sesión basados en web, móviles y API  
- [ ] Sí — Pero el MFA **no se aplica** en puntos finales específicos (ej. `/api/v1/login` heredado o portales específicos para móviles)  
- [ ] No — El MFA **no está implementado** en la aplicación *(Crítico)*  

### ¿Se puede omitir el paso de verificación de MFA mediante la navegación directa por URL o la manipulación de respuestas?
- [ ] No — La navegación directa o la manipulación de parámetros **no pueden** omitir el desafío  
- [ ] Sí — Navegar directamente a las URL del panel de control (dashboard) interno **es posible** sin completar el desafío de MFA  
- [ ] Sí — Manipular la respuesta del servidor (ej. cambiar un `401 Unauthorized` a `200 OK`) **es posible** para obtener acceso  

### ¿Está el proceso de verificación del código MFA protegido contra ataques de fuerza bruta (Brute Force)?
- [ ] Sí — Se aplica una limitación de tasa (Rate Limiting) estricta o bloqueo de cuenta tras múltiples intentos fallidos de OTP  
- [ ] Sí — Existe una limitación de tasa (Rate Limiting), pero **es posible** omitirla mediante rotación de IP o manipulación de cabeceras (headers)  
- [ ] No — No se aplica ninguna limitación de tasa (Rate Limiting) y el ataque automatizado de fuerza bruta (Brute Force) de códigos **es posible**  

### ¿Mantiene la aplicación un estado de sesión seguro durante la transición al MFA?
- [ ] Sí — Los tokens de sesión de altos privilegios se emiten **solo después** de completar con éxito el MFA  
- [ ] No — Se emite una cookie de sesión totalmente funcional **antes** de completar el MFA, permitiendo el acceso a algunas funciones  
- [ ] No — La aplicación utiliza un identificador estático o una transición de sesión predecible que **puede** ser secuestrada (hijacked)  

### ¿Son vulnerables a la explotación los factores de MFA alternativos (SMS, Email, códigos de respaldo)?
- [ ] No — Todos los factores utilizan valores seguros, no predecibles y limitados en el tiempo  
- [ ] Sí — Los códigos de respaldo son predecibles o **pueden** ser enumerados  
- [ ] Sí — La funcionalidad "Reenviar código" puede ser abusada para realizar inundaciones (flooding) de SMS/Email o revelar detalles de contacto parciales  

---