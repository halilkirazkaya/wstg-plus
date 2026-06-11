## WSTG-IDNT-02 — Prueba del Proceso de Registro de Usuarios

La prueba del proceso de registro de usuarios implica evaluar el flujo de trabajo mediante el cual se crean nuevas identidades para garantizar que no pueda ser abusado para el acceso no autorizado o la explotación automatizada. Las vulnerabilidades en este proceso a menudo se manifiestan como la falta de verificación de identidad (email/SMS), susceptibilidad a la creación masiva de cuentas mediante bots, o divulgación de información a través de mensajes de error detallados que revelan nombres de usuario existentes. Los atacantes explotan estos fallos para realizar enumeración de usuarios (User Enumeration), llevar a cabo campañas de spam a gran escala, o manipular los parámetros de registro para obtener privilegios elevados durante la fase de creación de la cuenta. Esta prueba es vital para proteger la integridad de la aplicación y evitar que los atacantes pueblen el sistema con cuentas fraudulentas o maliciosas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### ¿Se requiere verificación de identidad antes de que una cuenta nueva se active?
- [ ] Sí — el registro requiere un token único y limitado en el tiempo enviado por correo electrónico o SMS  
- [ ] Sí — se requiere verificación, pero los tokens son **débiles** o **predecibles**  
- [ ] No — las cuentas se activan **inmediatamente** sin ningún paso de verificación  

### ¿El proceso de registro evita la enumeración de usuarios (User Enumeration)?
- [ ] Sí — la aplicación devuelve respuestas **idénticas** tanto para correos/nombres de usuario nuevos como existentes  
- [ ] No — los mensajes de error (p. ej., "Email ya está en uso") **permiten** a un atacante mapear usuarios registrados  
- [ ] No — las diferencias de tiempo en las respuestas del servidor **revelan** si un usuario existe  

### ¿Se han implementado controles contra la automatización para evitar el registro masivo?
- [ ] Sí — los mecanismos de CAPTCHA o prueba de trabajo (Proof-of-Work) están **presentes** y son **efectivos**  
- [ ] Sí — los controles existen, pero el bypass **es posible** a través de endpoints de API o manipulación de encabezados (Headers)  
- [ ] No — no se aplica Rate Limiting o CAPTCHA al endpoint de registro  

### ¿Pueden manipularse los parámetros de registro para escalar privilegios?
- [ ] No — los roles de usuario se asignan **estrictamente** mediante la lógica del lado del servidor  
- [ ] Sí — parámetros como `role`, `admin`, o `group_id` **pueden** ser modificados en la solicitud para obtener un acceso superior  

### ¿Se aplica la política de contraseñas durante la fase de registro?
- [ ] Sí — se **imponen** requisitos de contraseñas robustas en el lado del servidor  
- [ ] No — el sistema **acepta** contraseñas débiles (p. ej., "password123")  

---