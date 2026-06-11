## WSTG-IDNT-04 — Testing for Account Enumeration and Guessable User Account

La **Account Enumeration** ocurre cuando una aplicación revela la existencia o inexistencia de una cuenta de usuario a través de variaciones en las respuestas HTTP, mensajes de error o diferencias de **Timing** medibles. Los atacantes explotan este comportamiento enviando sistemáticamente listas de nombres de usuario para identificar cuentas válidas, las cuales son posteriormente blanco de **Credential Stuffing**, ataques de **Brute Force** o ingeniería social. Esta vulnerabilidad se manifiesta comúnmente en portales de inicio de sesión, funciones de restablecimiento de contraseña, formularios de registro y **API** endpoints que manejan identificadores específicos del usuario. Desde la perspectiva de un atacante, una enumeración exitosa reduce significativamente la superficie de ataque al transformar un intento de **Brute Force** a ciegas en una explotación dirigida de identidades conocidas y válidas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Bajo / Medio |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### ¿Son los mensajes de error de autenticación consistentes en todos los escenarios?
- [ ] Sí — se utilizan mensajes de error genéricos tanto para nombres de usuario inválidos como para contraseñas inválidas  
- [ ] Sí — los mensajes son similares, pero **están presentes** ligeras variaciones en la puntuación o en el uso de mayúsculas/minúsculas  
- [ ] No — los mensajes de error indican explícitamente "User not found" o "Incorrect password" *(Vulnerable)*  

### ¿El flujo de restablecimiento de contraseña o "olvidé mi contraseña" filtra la existencia de cuentas?
- [ ] No — la aplicación devuelve un mensaje genérico independientemente de si el correo/nombre de usuario existe  
- [ ] Sí — existen controles, pero el **Bypass** es posible mediante la longitud de la respuesta o los códigos de estado  
- [ ] Sí — la aplicación confirma explícitamente si se envió un correo de restablecimiento o si la cuenta **no existe**  

### ¿Está el proceso de registro protegido contra la Account Enumeration?
- [ ] No — el registro no filtra información o utiliza CAPTCHA/**Rate Limiting** para prevenir verificaciones masivas  
- [ ] Sí — existen controles, pero el **Bypass** es posible mediante diferencias de **Timing** o en la respuesta de la **API**  
- [ ] Sí — la aplicación notifica inmediatamente al usuario si el nombre de usuario o correo electrónico **ya está en uso**  

### ¿Son medibles las diferencias de Timing al comparar nombres de usuario válidos vs. inválidos?
- [ ] No — los tiempos de respuesta son consistentes independientemente de la validez de la cuenta debido a comparaciones de tiempo constante  
- [ ] Sí — existen diferencias de **Timing** pero son demasiado pequeñas para ser medidas de forma fiable a través de la red  
- [ ] Sí — **están presentes** discrepancias de **Timing** medibles (por ejemplo, debido a la ejecución del hashing de contraseña solo para usuarios válidos)  

### ¿Utiliza la aplicación convenciones de nombres predecibles o adivinables para las cuentas?
- [ ] No — los identificadores de cuenta son aleatorios (UUIDs) o definidos por el usuario y complejos  
- [ ] Sí — los identificadores de cuenta siguen un patrón (ej. `emp001`, `emp002`) y la enumeración **es posible**  
- [ ] Sí — los identificadores son enteros secuenciales, lo que hace **posible** la enumeración completa de la base de datos  

---