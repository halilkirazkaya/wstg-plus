## WSTG-CRYP-02 — Pruebas de Padding Oracle

Las vulnerabilidades de Padding Oracle ocurren cuando el proceso de descifrado de una aplicación filtra información sobre si el relleno (padding) de un texto cifrado descifrado es válido o inválido. Al observar estas respuestas de canal lateral (side-channel)—como códigos de estado HTTP distintos, mensajes de error específicos o variaciones en los tiempos de respuesta—un atacante puede descifrar datos sin conocer la clave de cifrado y, potencialmente, volver a cifrar texto en claro (plaintext) arbitrario. Esta vulnerabilidad se manifiesta típicamente en cookies de sesión cifradas, tokens de identidad o parámetros de URL que utilizan cifradores de bloque en modo Cipher Block Chaining (CBC) con relleno PKCS#7. La explotación permite la pérdida completa de confidencialidad y puede conducir al compromiso de la integridad mediante la construcción de bloques cifrados falsificados.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Crítica* |

> *La severidad es Crítica si la vulnerabilidad reside en la gestión de sesiones, tokens de identidad o cifrado de PII.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### ¿Se utiliza un cifrador de bloque en modo CBC para datos sensibles?
- [ ] No — la aplicación utiliza cifrado autenticado (AES-GCM/ChaCha20-Poly1305)  
- [ ] Sí — la aplicación utiliza cifradores de bloque pero **no** se requiere relleno (padding) (ej. cifradores de flujo o modo CTR)  
- [ ] Sí — la aplicación utiliza el modo CBC y se **aplica** relleno (padding)  

### ¿Revela el servidor la validez del relleno a través de canales laterales?
- [ ] No — el servidor devuelve mensajes de error uniformes y tiempos de respuesta consistentes  
- [ ] Sí — el servidor devuelve diferentes códigos de estado HTTP (ej. 200 vs 500) para errores de relleno  
- [ ] Sí — el servidor devuelve mensajes de error específicos (ej. "Invalid Padding", "Decryption failed")  
- [ ] Sí — el servidor presenta diferencias de tiempo medibles durante el fallo del descifrado  

### ¿Es posible el descifrado automatizado del texto cifrado?
- [ ] No — los canales laterales **no** son lo suficientemente estables para la explotación  
- [ ] Sí — el texto cifrado **puede** ser descifrado parcialmente (últimos bloques)  
- [ ] Sí — la recuperación completa del plaintext **es posible** utilizando herramientas como `PadBuster`  

### ¿Puede el atacante realizar bit-flipping o falsificar textos cifrados válidos?
- [ ] No — las verificaciones de integridad (HMAC) se comprueban **antes** del descifrado  
- [ ] Sí — no hay verificación de integridad, pero la construcción del Payload **no** es factible  
- [ ] Sí — la construcción de texto cifrado arbitrario **es posible**, permitiendo la escalada de privilegios o la inyección de datos  

---