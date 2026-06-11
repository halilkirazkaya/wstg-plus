## WSTG-ATHN-03 — Pruebas de mecanismos de bloqueo de cuenta débiles

Un mecanismo de bloqueo de cuenta es un control crítico de defensa en profundidad diseñado para mitigar ataques de Brute Force y Credential Stuffing mediante la desactivación de una cuenta tras un número específico de intentos de autenticación fallidos. Sin una política de bloqueo robusta, los atacantes pueden utilizar herramientas automatizadas para adivinar contraseñas sistemáticamente en múltiples cuentas (Password Spraying) o dirigirse a una sola cuenta de alto valor hasta tener éxito. Esta vulnerabilidad se manifiesta típicamente en portales de inicio de sesión, formularios de restablecimiento de contraseña y endpoints de API que carecen de Rate Limiting o protección a nivel de cuenta. Desde la perspectiva de un atacante, un mecanismo de bloqueo débil o ausente permite intentos de contraseña infinitos, mientras que uno excesivamente agresivo puede ser explotado para causar un Denial of Service (DoS) distribuido contra usuarios legítimos al bloquear intencionadamente sus cuentas.

| Campo | Valor |
|---|---|
| **ID WSTG** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad aumenta a Alta si la aplicación maneja PII sensible, datos financieros, o si las cuentas administrativas son susceptibles a Brute Force sin ningún control secundario.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Herramientas:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### ¿Se aplica un mecanismo de bloqueo de cuenta tras un número determinado de intentos fallidos?
- [ ] Sí — la cuenta se bloquea tras un umbral definido y razonable (ej. 5-10 intentos)  
- [ ] Sí — la cuenta se bloquea pero solo tras un número excesivamente alto de intentos  
- [ ] No — la cuenta **no** puede ser bloqueada y permite Brute Force indefinido  

### ¿Se puede omitir (bypass) el mecanismo de bloqueo mediante técnicas comunes de evasión?
- [ ] No — el bloqueo se aplica en el lado del servidor (server-side) y **no** se ve afectado por cambios en el lado del cliente  
- [ ] Sí — **es posible** omitir el bloqueo mediante la rotación de direcciones IP o el uso de un pool de proxies  
- [ ] Sí — **es posible** omitir el bloqueo manipulando cabeceras HTTP como `X-Forwarded-For` o `User-Agent`  
- [ ] Sí — **es posible** omitir el bloqueo borrando cookies o iniciando nuevas sesiones  

### ¿Proporciona el mecanismo de bloqueo una vía para la recuperación o expiración de la cuenta?
- [ ] Sí — la cuenta se desbloquea automáticamente tras una duración establecida o mediante verificación por correo electrónico  
- [ ] Sí — la cuenta requiere la intervención administrativa para ser desbloqueada  
- [ ] No — el bloqueo es permanente sin una vía de recuperación clara, lo que aumenta el riesgo de DoS  

### ¿Diferencia la aplicación entre un nombre de usuario válido e inválido durante el bloqueo?
- [ ] No — la respuesta de la aplicación es idéntica tanto para usuarios existentes como para los no existentes  
- [ ] Sí — la aplicación revela si una cuenta está bloqueada, lo que permite la **enumeración de nombres de usuario** (username enumeration)  

### ¿Es el mecanismo de bloqueo susceptible a un ataque de Denegación de Servicio (DoS)?
- [ ] No — controles como CAPTCHA o Rate Limiting basado en IP previenen el bloqueo masivo de cuentas  
- [ ] Sí — un atacante **puede** bloquear sistemáticamente a todos los usuarios conocidos proporcionando contraseñas incorrectas  

---