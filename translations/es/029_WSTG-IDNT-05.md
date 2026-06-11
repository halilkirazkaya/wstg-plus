## WSTG-IDNT-05 — Prueba de políticas de nombres de usuario débiles o no aplicadas

Las pruebas de políticas de nombres de usuario débiles o no aplicadas se centran en las reglas que rigen la creación de identificadores de cuenta y su susceptibilidad a la enumeración o suplantación (spoofing). Las políticas débiles a menudo permiten secuencias predecibles, palabras comunes de diccionarios o identificadores que reflejan los ID internos de los empleados, lo que reduce significativamente el espacio de búsqueda para ataques de Brute Force y credential stuffing. Desde la perspectiva de un atacante, identificar estos patrones es el primer paso en el descubrimiento de cuentas a gran escala, lo cual se logra analizando mensajes de error de registro, el comportamiento del restablecimiento de contraseñas o metadatos en perfiles públicos. Garantizar una política robusta evita que los atacantes mapeen sistemáticamente la base de usuarios y facilita la defensa contra ataques dirigidos de phishing e intentos automatizados de bypass de autenticación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### ¿Se basan los nombres de usuario en patrones altamente predecibles o secuenciales?
- [ ] No — los nombres de usuario son aleatorios, definidos por el usuario o cadenas de alta entropía.
- [ ] Sí — los nombres de usuario siguen un formato predecible (p. ej., `user1001`, `user1002`), pero la autenticación es robusta.
- [ ] Sí — los nombres de usuario son **estrictamente secuenciales** o siguen un formato corporativo conocido, lo que facilita la enumeración.

### ¿Aplica la aplicación requisitos mínimos de longitud y complejidad para los nombres de usuario?
- [ ] Sí — se **aplican** políticas estrictas de longitud y juego de caracteres durante el registro.
- [ ] Sí — existen políticas, pero permiten nombres de usuario extremadamente cortos (p. ej., 1-2 caracteres) o excesivamente simples.
- [ ] No — **no se aplica ninguna política** con respecto a la longitud del nombre de usuario o los tipos de caracteres.

### ¿Se pueden enumerar nombres de usuario válidos a través de las respuestas de la aplicación?
- [ ] No — la aplicación devuelve mensajes de error genéricos y muestra tiempos de respuesta consistentes para todos los intentos.
- [ ] Sí — la aplicación devuelve mensajes de error distintos (p. ej., "El nombre de usuario ya está en uso"), pero se aplica Rate Limiting.
- [ ] Sí — la aplicación **filtra** nombres de usuario válidos a través de errores de registro, restablecimientos de contraseña o diferencias de tiempo.

### ¿Impide la política el registro de nombres de usuario comunes o administrativos reservados?
- [ ] Sí — la aplicación bloquea nombres comunes (p. ej., `admin`, `root`, `support`) y términos reservados del sistema.
- [ ] No — los usuarios **pueden** registrar cuentas con nombres sensibles o administrativos.

### ¿Es la política de nombres de usuario consistente en todos los puntos de entrada (API, Móvil, Web)?
- [ ] Sí — las políticas **se aplican** de manera consistente en todas las interfaces y versiones.
- [ ] No — los endpoints heredados o versiones específicas de la API **no aplican** las mismas restricciones de nombre de usuario.

---