## WSTG-IDNT-03 — Test Account Provisioning Process

El proceso de aprovisionamiento de cuentas rige cómo se crean, verifican y asignan privilegios a las nuevas identidades dentro del entorno de una aplicación. Los atacantes se dirigen a este flujo de trabajo para realizar registros masivos automatizados para spam o denegación de servicio (DoS), evadir los pasos de verificación de identidad para crear cuentas fraudulentas, o manipular parámetros para otorgarse permisos elevados durante la fase de registro (signup). Las vulnerabilidades suelen manifestarse en formularios de autoregistro, sistemas basados solo en invitaciones o consolas de aprovisionamiento administrativo donde los fallos de lógica de negocio permiten la creación de cuentas no autorizadas o la escalada de privilegios (Privilege Escalation). Desde la perspectiva de un atacante, comprometer el flujo de aprovisionamiento proporciona un punto de apoyo legítimo que puede utilizarse como base para una explotación más profunda, la filtración de datos (Data Exfiltration) o ataques de ingeniería social.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta* |

> *La severidad se vuelve Crítica si un atacante puede aprovisionar cuentas administrativas o evadir las restricciones globales de registro.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### ¿El autoregistro está estrictamente controlado o limitado a usuarios autorizados?
- [ ] Sí — el registro está **deshabilitado** o requiere aprobación administrativa manual  
- [ ] Sí — el registro está abierto pero restringido a dominios de correo electrónico **autorizados** o tokens de invitación  
- [ ] No — el registro está **abierto** para cualquier usuario sin restricciones de dominio o tokens  

### ¿Se requiere un mecanismo robusto de verificación de identidad (p. ej., email/SMS) antes de la activación de la cuenta?
- [ ] Sí — la verificación es **obligatoria** y **no puede** ser evadida  
- [ ] Sí — la verificación es **obligatoria** pero **puede** ser evadida mediante manipulación de parámetros (Parameter Tampering) o tokens predecibles  
- [ ] No — las cuentas se activan **inmediatamente** tras el envío sin ninguna verificación  

### ¿Puede el usuario manipular los parámetros de rol o permisos durante la solicitud de registro?
- [ ] No — los roles se **asignan en el lado del servidor** (Server-side) y no existen parámetros en el lado del cliente  
- [ ] Sí — existen parámetros de rol pero su manipulación **no es posible** debido a la validación en el lado del servidor  
- [ ] Sí — los parámetros de rol/permisos (p. ej., `role=admin`, `is_staff=true`) **pueden** ser modificados para obtener acceso elevado *(Crítica)*  

### ¿Se han implementado controles de límite de tasa (Rate Limiting) o medidas anti-automatización para prevenir la creación masiva de cuentas?
- [ ] Sí — los límites de tasa y CAPTCHAs están **activos** y son efectivos  
- [ ] Sí — existen controles pero la evasión **es posible** mediante suplantación de cabeceras (Header Spoofing) o servicios de resolución de CAPTCHA  
- [ ] No — no se **aplican** límites de tasa ni controles anti-automatización  

### ¿El proceso de aprovisionamiento filtra información sobre cuentas existentes (Enumeración de Usuarios / User Enumeration)?
- [ ] No — los mensajes de respuesta son **idénticos** para usuarios existentes y no existentes  
- [ ] Sí — diferentes respuestas o variaciones de tiempo **permiten** la enumeración de usuarios existentes  

---