## WSTG-ATHN-05 — Pruebas de Vulnerabilidades en la Función "Recordar Contraseña"

La funcionalidad de "Recordarme" o inicio de sesión persistente está diseñada para mantener el estado autenticado de un usuario a través de los reinicios del navegador mediante el almacenamiento de un token o credencial en el lado del cliente. Si esta característica está mal implementada —como al almacenar contraseñas en texto claro (plaintext), hashes débiles o tokens de baja entropía en cookies— expone al usuario a un secuestro de cuenta (account takeover) a través de Cross-Site Scripting (XSS), acceso físico o interceptación de red. Los pentesters evalúan la entropía de estos tokens de persistencia, los flags de seguridad aplicados al mecanismo de almacenamiento y el ciclo de vida de la sesión para asegurar que la conveniencia del acceso persistente no eluda controles de seguridad críticos como la Multi-Factor Authentication (MFA) o la invalidación de sesión. La explotación a menudo implica la recolección de estos tokens de larga duración para obtener acceso no autorizado a las cuentas sin requerir las credenciales originales.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta* |

> *La severidad se considera Alta si se almacenan credenciales en texto claro o hashes sin sal (unsalted) en la cookie persistente, o si el token permite evadir la Multi-Factor Authentication (MFA).

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Herramientas:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### ¿Implementa la aplicación una función de "Recordarme" o "Mantener sesión iniciada"?
- [ ] No — la función **no** existe en la aplicación  
- [ ] Sí — la función existe y utiliza tokens criptográficamente fuertes y no predecibles  
- [ ] Sí — la función existe pero utiliza tokens predecibles o de baja entropía *(Media)*  
- [ ] Sí — la función existe y almacena credenciales en texto claro o contraseñas codificadas en base64 *(Alta)*  

### ¿Se aplican correctamente los flags de seguridad a la cookie persistente?
- [ ] Sí — los flags `HttpOnly`, `Secure` y `SameSite` están **aplicados**  
- [ ] Sí — se aplican algunos flags pero **falta** `HttpOnly`  
- [ ] Sí — se aplican algunos flags pero **falta** `Secure` sobre HTTPS  
- [ ] No — no se han **aplicado** flags de seguridad a la cookie persistente  

### ¿El token de "Recordarme" sigue siendo válido después de un cierre de sesión manual?
- [ ] No — el token se invalida en el lado del servidor inmediatamente al cerrar la sesión  
- [ ] Sí — el token permanece válido pero requiere una contraseña para acciones sensibles  
- [ ] Sí — el token permanece válido y permite el acceso completo a la cuenta **sin** re-autenticación *(Media)*  

### ¿Se finaliza la sesión persistente al realizar un cambio de contraseña?
- [ ] Sí — todos los tokens persistentes son revocados cuando el usuario cambia su contraseña  
- [ ] No — el token de "Recordarme" permanece válido después de que **se realiza** un restablecimiento o cambio de contraseña *(Alta)*  

### ¿El token persistente evade la Multi-Factor Authentication (MFA) en visitas posteriores?
- [ ] No — se requiere MFA incluso cuando hay un token de "Recordarme" presente  
- [ ] Sí — se evade la MFA, pero el token está vinculado a una huella digital (fingerprint) de dispositivo específica  
- [ ] Sí — la MFA se **evade completamente** utilizando solo la cookie persistente desde cualquier dispositivo *(Alta)*  

---