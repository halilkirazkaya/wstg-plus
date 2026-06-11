## WSTG-ATHN-09 — Pruebas de Funcionalidades de Cambio o Restablecimiento de Contraseña Débiles

Los mecanismos de cambio y restablecimiento de contraseña son componentes críticos de la gestión de la autenticación que, si se implementan incorrectamente, permiten a los atacantes tomar el control de las cuentas de usuario. Estas vulnerabilidades suelen implicar tokens de restablecimiento predecibles, falta de bloqueos de cuenta durante intentos de Brute Force, entrega insegura de tokens o la posibilidad de cambiar contraseñas sin verificar la contraseña actual. Los atacantes explotan estos fallos interceptando o adivinando enlaces de recuperación, realizando Host Header Injection para redirigir correos electrónicos de restablecimiento o utilizando Session Fixation para mantener el acceso tras un cambio de contraseña. Garantizar que estos procesos requieran una verificación sólida y utilicen aleatoriedad criptográfica es esencial para mantener la integridad de la cuenta y evitar el acceso no autorizado.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### ¿Se requiere la contraseña actual para establecer una nueva contraseña durante un cambio estándar?
- [ ] Sí — la contraseña actual **es requerida** y verificada  
- [ ] Sí — la contraseña actual **es requerida** pero puede omitirse mediante Parameter Pollution o manipulación  
- [ ] No — la contraseña actual **no es** solicitada, lo que permite el Account Takeover mediante Session Hijacking *(Alta)*  

### ¿Son los tokens de restablecimiento de contraseña criptográficamente seguros e impredecibles?
- [ ] Sí — los tokens tienen alta entropía, son aleatorios y **no son predecibles**  
- [ ] Sí — se generan tokens pero muestran baja entropía o patrones predecibles (p. ej., basados en marcas de tiempo)  
- [ ] No — los tokens son **predecibles** o se basan en datos estáticos del usuario como email/ID codificados en Base64 *(Crítica)*  

### ¿Se puede redirigir el enlace de restablecimiento de contraseña mediante Host Header Injection?
- [ ] No — la aplicación ignora o valida el encabezado `Host` para la generación del enlace  
- [ ] Sí — la aplicación utiliza el encabezado `Host` o `X-Forwarded-Host` para construir los enlaces, pero la validación **está presente**  
- [ ] Sí — los enlaces **pueden** ser redirigidos a un dominio controlado por el atacante mediante la manipulación de encabezados *(Alta)*  

### ¿Tiene el token de restablecimiento un ciclo de vida limitado e invalidación inmediata?
- [ ] Sí — los tokens expiran rápidamente y se invalidan **inmediatamente** después de su uso o del cambio de contraseña  
- [ ] Sí — los tokens expiran tras una duración prolongada (p. ej., más de 24 horas) o permiten **múltiples usos**  
- [ ] No — los tokens **nunca expiran** o permanecen válidos después de que la contraseña ha sido cambiada con éxito  

### ¿Existen límites de tasa (Rate Limiting) o bloqueos en los endpoints de restablecimiento y cambio de contraseña?
- [ ] Sí — **se aplican** Rate Limiting estricto y CAPTCHAs para evitar la enumeración y el Brute Force  
- [ ] Sí — existen controles limitados pero su elusión (bypass) **es posible** mediante rotación de IP o spoofing de encabezados  
- [ ] No — no **se aplica** Rate Limiting, lo que permite la enumeración masiva de cuentas o el Brute Force de tokens  

---