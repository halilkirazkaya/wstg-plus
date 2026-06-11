## WSTG-ATHN-07 — Pruebas de Política de Contraseñas Débiles

Las pruebas de políticas de contraseñas débiles consisten en evaluar los requisitos de complejidad, longitud y entropía aplicados por una aplicación durante la creación de cuentas y las actualizaciones de credenciales. Las políticas insuficientes comprometen directamente la confidencialidad de las cuentas de usuario al hacerlas susceptibles a ataques de diccionario automatizados, intentos de Brute Force y Credential Stuffing. Estas vulnerabilidades suelen residir en los endpoints de registro, flujos de trabajo de restablecimiento de contraseña y paneles administrativos de gestión de usuarios. Desde la perspectiva de un atacante, una política permisiva reduce significativamente el coste computacional y el tiempo necesarios para comprometer credenciales con éxito utilizando wordlists comunes o cracking basado en patrones.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Baja* |

> *La severidad pasa a ser Alta si la aplicación carece de mecanismos de bloqueo de cuenta o Rate Limiting para prevenir el guessing automatizado.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Herramientas:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### ¿Aplica la aplicación una longitud mínima de contraseña?
- [ ] Sí — la longitud mínima es de 12+ caracteres *(Más seguro)*  
- [ ] Sí — la longitud mínima es de entre 8 y 11 caracteres  
- [ ] Sí — la longitud mínima es **menor de** 8 caracteres  
- [ ] No — no se aplica ninguna longitud mínima  

### ¿Se exigen requisitos de complejidad (mayúsculas, minúsculas, dígitos, símbolos)?
- [ ] Sí — el uso de múltiples clases de caracteres es obligatorio y el **bypass** no es posible  
- [ ] Sí — se sugieren clases de caracteres pero **no** se exigen  
- [ ] No — no se aplican requisitos de complejidad  

### ¿Bloquea la aplicación contraseñas comunes/débiles o contraseñas que contienen datos específicos del usuario?
- [ ] Sí — las contraseñas débiles conocidas (ej. "password123") y las basadas en el nombre de usuario **no** pueden utilizarse  
- [ ] Sí — se bloquean las contraseñas comunes, pero se **pueden** utilizar datos específicos del usuario (ej. nombre de usuario, correo electrónico)  
- [ ] No — se acepta cualquier cadena que cumpla con los requisitos de longitud/complejidad  

### ¿Pueden omitirse los requisitos de complejidad o longitud mediante una modificación en el lado del cliente (Client-Side)?
- [ ] No — las políticas se aplican estrictamente en el **lado del servidor (Server-Side)**  
- [ ] Sí — las políticas se aplican mediante JavaScript y **pueden** omitirse interceptando la solicitud  

### ¿Se comunica claramente la política de contraseñas al usuario sin filtrar detalles de implementación?
- [ ] Sí — la política es visible durante la entrada de datos y proporciona mensajes de error genéricos  
- [ ] Sí — la política es visible, pero los mensajes de error revelan regex específicos o brechas en la lógica  
- [ ] No — la política **no** es visible, lo que obliga al descubrimiento mediante ensayo y error  

---