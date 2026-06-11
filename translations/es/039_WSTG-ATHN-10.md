## WSTG-ATHN-10 — Pruebas de autenticación débil en canales alternativos

Las pruebas de autenticación débil en canales alternativos consisten en identificar y analizar rutas secundarias de acceso a la cuenta —como APIs móviles, flujos de recuperación de contraseñas, interfaces de soporte técnico o subdominios heredados (legacy)— que pueden no aplicar el mismo rigor de seguridad que el portal web principal. Estos canales alternativos suelen carecer de una Multi-factor Authentication (MFA) robusta, de un Rate Limiting estricto o de requisitos de contraseñas complejas, creando un "eslabón más débil" que compromete todo el sistema de autenticación. Los atacantes se dirigen específicamente a estos puntos de entrada ignorados para realizar Credential Stuffing o eludir la MFA aprovechando las discrepancias en cómo las diferentes interfaces gestionan la verificación de identidad. Garantizar la paridad en todas las superficies de autenticación es esencial para prevenir el acceso no autorizado y mantener la integridad de la sesión y los datos del usuario.

| Campo | Valor |
|---|---|
| **ID de WSTG** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### ¿Existen y se han identificado canales de autenticación alternativos?
- [ ] No — solo existe un único canal de autenticación  
- [ ] Sí — se identificaron múltiples canales (p. ej., API de aplicación móvil, cliente de escritorio, portal heredado, servicios SOAP)  

### ¿Los canales alternativos imponen una paridad de seguridad con el canal principal?
- [ ] Sí — todos los canales aplican requisitos **idénticos** de complejidad de contraseña y MFA  
- [ ] Sí — los canales aplican complejidad de contraseña, pero la MFA **no es obligatoria** o **puede** ser eludida  
- [ ] No — los canales alternativos tienen requisitos de autenticación **significativamente más débiles**  

### ¿Son consistentes el Rate Limiting y el bloqueo de cuentas en todos los canales?
- [ ] Sí — el Rate Limiting y el bloqueo se **aplican de manera consistente** en todos los endpoints  
- [ ] Sí — existen controles, pero la elusión (bypass) **es posible** en canales específicos (p. ej., API móvil)  
- [ ] No — el Rate Limiting o el bloqueo **no se aplican** a las rutas de autenticación secundarias  

### ¿Se puede eludir la MFA cambiando a un canal alternativo?
- [ ] No — la MFA es obligatoria y **no puede** ser eludida independientemente del punto de entrada  
- [ ] Sí — la MFA es requerida en el portal web pero **no se aplica** en la API móvil o heredada  

### ¿Son los flujos de recuperación de contraseña o de "olvidé mi contraseña" más débiles que el inicio de sesión estándar?
- [ ] No — los flujos de recuperación utilizan una verificación sólida fuera de banda (out-of-band) y no filtran información  
- [ ] Sí — los flujos de recuperación utilizan preguntas de seguridad débiles que **pueden** ser adivinadas o investigadas fácilmente  
- [ ] Sí — los flujos de recuperación son vulnerables a la enumeración o **no requieren** el mismo nivel de prueba que un inicio de sesión estándar  

---