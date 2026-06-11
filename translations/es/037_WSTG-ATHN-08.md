## WSTG-ATHN-08 — Pruebas de Respuestas a Preguntas de Seguridad Débiles

Las pruebas de respuestas a preguntas de seguridad débiles consisten en evaluar los mecanismos de autenticación basada en el conocimiento (KBA) utilizados para verificar la identidad de un usuario, generalmente durante el restablecimiento de contraseñas o en flujos de autenticación de múltiples factores. Esto es relevante porque las preguntas de seguridad a menudo dependen de información estática y no secreta que puede ser descubierta fácilmente mediante inteligencia de fuentes abiertas (OSINT) o ingeniería social, lo que conduce a una toma de control de cuentas (Account Takeover) no autorizada. Estas vulnerabilidades suelen ocurrir en los módulos de "Olvidé mi contraseña" o "Desbloqueo de cuenta", donde los usuarios proporcionan respuestas a preguntas preestablecidas. Desde la perspectiva de un atacante, este es un objetivo principal para realizar Brute Force automatizado o investigación dirigida, ya que muchos usuarios proporcionan respuestas predecibles o públicamente verificables a preguntas comunes como "¿Cuál es el apellido de soltera de su madre?" o "¿A qué escuela secundaria asistió?".

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Estado de la Prueba** | No Realizado |
| **Severidad** | Media / Alta* |

> *La severidad se considera Alta si la pregunta de seguridad es el único requisito para un restablecimiento completo de contraseña y la toma de control de la cuenta sin verificaciones adicionales.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Herramientas:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### ¿Implementa la aplicación preguntas de seguridad para la verificación de identidad o la recuperación de contraseñas?
- [ ] No — las preguntas de seguridad **no se utilizan** para la autenticación o recuperación  
- [ ] Sí — las preguntas de seguridad **se utilizan** como un factor secundario opcional  
- [ ] Sí — las preguntas de seguridad **se utilizan** como el mecanismo de recuperación principal o único *(Riesgo Alto)*  

### ¿Las preguntas de seguridad se seleccionan de una lista fija o son definidas por el usuario?
- [ ] No — las preguntas son definidas enteramente por el usuario y requieren una alta entropía  
- [ ] Sí — las preguntas son fijas pero incluyen opciones que no son descubribles mediante OSINT  
- [ ] Sí — las preguntas provienen de una lista fija de temas comunes descubribles mediante OSINT *(Vulnerable)*  

### ¿Se aplica Rate Limiting o bloqueo de cuenta a los intentos de respuesta a las preguntas de seguridad?
- [ ] Sí — se aplican políticas estrictas de **Rate Limiting** y bloqueo de cuenta para prevenir el Brute Force  
- [ ] Sí — se aplica **Rate Limiting** pero es posible realizar un **bypass** mediante rotación de IP o manipulación de cabeceras  
- [ ] No — **no se aplica Rate Limiting** en los intentos de respuesta  

### ¿La aplicación impone complejidad o validación en el contenido de la respuesta?
- [ ] Sí — la validación impide el uso de palabras comunes y garantiza la complejidad o unicidad de la respuesta  
- [ ] Sí — existe una validación básica pero **puede** ser omitida con listas de palabras comunes o ataques de diccionario  
- [ ] No — **no se realiza validación**; se permiten respuestas simples o de un solo carácter  

### ¿Puede omitirse el desafío de la pregunta de seguridad mediante fallos lógicos?
- [ ] No — el flujo de recuperación requiere estrictamente una respuesta válida para continuar  
- [ ] Sí — el flujo de recuperación **puede** ser vulnerado mediante un **bypass** manipulando códigos de respuesta HTTP o parámetros de sesión  
- [ ] Sí — la respuesta se refleja en el código fuente o en campos ocultos de la página  

---