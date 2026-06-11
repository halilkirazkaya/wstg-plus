## WSTG-SESS-10 — Testing JSON Web Tokens

Los JSON Web Tokens (JWT) se utilizan comúnmente para la gestión de sesiones sin estado (stateless) y la propagación de identidad, pero su seguridad depende enteramente de la implementación correcta de los algoritmos de firma y de una verificación estricta de los claims. Los atacantes intentan frecuentemente eludir la autenticación explotando fallos del algoritmo "none", realizando fuerza bruta (Brute Force) sobre claves secretas HS256 débiles, o llevando a cabo ataques de intercambio de algoritmos (algorithm-switching) donde una clave pública asimétrica se utiliza como un secreto HMAC simétrico. Estas vulnerabilidades suelen manifestarse en las cookies de sesión o en las cabeceras de Authorization, permitiendo potencialmente que un atacante realice una escalada de privilegios o suplante a cualquier usuario modificando claims como `role` o `user_id` sin invalidar la integridad criptográfica del token.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Estado de la Prueba** | No realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Herramientas:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### ¿Acepta el servidor el algoritmo "none"?
- [ ] No — el servidor **rechaza** tokens que utilicen `alg: none` en la cabecera (header)  
- [ ] Sí — el servidor **acepta** tokens con `alg: none` y una firma vacía *(Crítico)*  

### ¿Cómo se verifica la firma del JWT y cómo se protege contra fuerza bruta (Brute Force)?
- [ ] Sí — se utilizan claves asimétricas fuertes (RS256/ES256) o secretos HMAC de alta entropía  
- [ ] Sí — se utiliza HS256 pero el secreto **puede** ser descifrado por Brute Force debido a una baja entropía o a valores por defecto  
- [ ] No — la verificación de la firma **no se aplica** o **puede** eludirse mediante el intercambio de algoritmos (de RS256 a HS256)  

### ¿El backend impone estrictamente los claims estándar (exp, nbf, iat)?
- [ ] Sí — el claim `exp` (expiración) está presente y **no puede** eludirse  
- [ ] Sí — `exp` está presente pero el servidor **no** lo impone durante la validación  
- [ ] No — los claims de expiración **faltan** o se ignoran, lo que permite un Replay Attack de sesión indefinido  

### ¿Evita la implementación la inyección de claves basada en cabeceras (kid, jku, x5u)?
- [ ] Sí — las cabeceras se sanean y solo se **permiten** fuentes o claves internas de confianza  
- [ ] No — la cabecera `kid` (Key ID) es vulnerable a Directory Traversal o SQL Injection para apuntar a un archivo conocido  
- [ ] No — las cabeceras `jku` o `x5u` **pueden** apuntar a URLs controladas por el atacante para cargar claves maliciosas  

---