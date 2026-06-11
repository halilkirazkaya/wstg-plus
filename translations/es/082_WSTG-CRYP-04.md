## WSTG-CRYP-04 — Testing for Weak Cryptographic Primitives

Las primitivas criptográficas débiles implican el uso de algoritmos obsoletos o inseguros, tales como MD5, SHA-1, DES o RC4, los cuales son susceptibles a ataques de colisión, análisis de frecuencias o descifrado por Brute Force. Estas debilidades ocurren típicamente en el hashing de contraseñas, el cifrado de datos en reposo (encryption at rest), la generación de tokens de sesión o la verificación de firmas digitales dentro del backend de la aplicación. Desde la perspectiva de un atacante, la identificación de estas primitivas permite comprometer la integridad y confidencialidad de los datos, como realizar el craqueo de hashes interceptados para recuperar contraseñas en texto claro o la falsificación de tokens de autenticación. Las aplicaciones modernas deben, en su lugar, utilizar primitivas que sean estándares de la industria, resistentes a colisiones y de alta entropía, como Argon2, Bcrypt o AES-GCM, para garantizar una protección robusta contra el criptoanálisis.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### ¿Se utilizan algoritmos de hashing obsoletos como MD5 o SHA-1 para el almacenamiento de datos sensibles?
- [ ] No — la aplicación utiliza algoritmos modernos resistentes a colisiones como Argon2 o Bcrypt  
- [ ] Sí — se utilizan algoritmos obsoletos pero **solo** para verificaciones de integridad que no son sensibles para la seguridad  
- [ ] Sí — **se utilizan** algoritmos obsoletos para contraseñas o tokens sensibles *(Alta)*  

### ¿Se emplean actualmente cifrados simétricos débiles como DES, 3DES o RC4?
- [ ] No — se utiliza AES (de 128 bits o superior) en un modo seguro como GCM o CBC  
- [ ] Sí — los cifrados legados están **habilitados** por compatibilidad con versiones anteriores, pero **no** son los predeterminados  
- [ ] Sí — los cifrados legados son el método principal para proteger los datos en reposo o en tránsito  

### ¿La aplicación implementa lógica criptográfica personalizada ("roll-your-own") o no estándar?
- [ ] No — la aplicación se adhiere estrictamente a librerías criptográficas estándar y revisadas por pares  
- [ ] Sí — existen wrappers personalizados, pero las primitivas subyacentes **son** estándares de la industria  
- [ ] Sí — **se aplican** algoritmos criptográficos propietarios o lógica personalizada a datos sensibles *(Crítica)*  

### ¿Se gestionan las sales criptográficas (salts) y los vectores de inicialización (IVs) según las mejores prácticas?
- [ ] Sí — los salts son únicos por usuario y los IVs son impredecibles para cada operación  
- [ ] Sí — se utilizan salts, pero **no** son únicos para todos los registros  
- [ ] No — los salts son estáticos, están hardcoded o ausentes, y los IVs **se reutilizan** a través de múltiples sesiones  

### ¿Es la longitud de bits de las claves asimétricas (RSA, Diffie-Hellman) suficiente para los estándares modernos?
- [ ] Sí — las claves RSA/DH son de 2048 bits o superiores, o se utiliza Criptografía de Curva Elíptica (ECC)  
- [ ] No — las claves son inferiores a 2048 bits, pero el bypass **no** es trivial  
- [ ] No — las claves son de 1024 bits o menores, lo que las hace **vulnerables** a la factorización o ataques computacionales  

---