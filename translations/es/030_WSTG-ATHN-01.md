## WSTG-ATHN-01 — Pruebas de transporte de credenciales a través de un canal cifrado

Las pruebas de transporte de credenciales a través de un canal cifrado garantizan que los datos de autenticación sensibles, como nombres de usuario, contraseñas y tokens de sesión, estén protegidos contra la interceptación durante el tránsito. Los atacantes posicionados en la red —por ejemplo, mediante ataques Man-in-the-Middle (MitM) en redes Wi-Fi públicas o redes internas comprometidas— pueden utilizar herramientas de sniffing de paquetes para capturar credenciales en texto claro si no se aplica correctamente TLS/SSL. Esta vulnerabilidad suele ocurrir en los endpoints de inicio de sesión, formularios de restablecimiento de contraseña y cabeceras de autenticación de API donde la aplicación no exige HTTPS o realiza redirecciones incorrectas desde HTTP. Una explotación exitosa otorga al atacante acceso total a las cuentas de usuario, lo que conlleva al compromiso total de los datos del usuario y a un posible movimiento lateral dentro de la aplicación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Herramientas:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### ¿Se exige HTTPS durante todo el proceso de autenticación?
- [ ] Sí — HSTS está **habilitado** y todo el tráfico se fuerza estrictamente a través de HTTPS  
- [ ] Sí — se produce la redirección a HTTPS, pero HSTS **no está habilitado**  
- [ ] No — las credenciales **pueden** enviarse a través de HTTP no cifrado  

### ¿Se sirven las páginas y formularios de inicio de sesión a través de un canal cifrado?
- [ ] Sí — la página que contiene el formulario de inicio de sesión se entrega vía HTTPS  
- [ ] No — la página de inicio de sesión se sirve a través de HTTP, permitiendo el secuestro de la acción del formulario (form-action hijacking) o el sniffing de credenciales  

### ¿Se aplica el flag `Secure` a todas las cookies de sesión sensibles?
- [ ] Sí — el flag `Secure` está **aplicado** a todas las cookies de autenticación y de sesión  
- [ ] Sí — el flag `Secure` está **aplicado** solo a algunas cookies  
- [ ] No — el flag `Secure` **no está aplicado**, lo que permite que las cookies sean exfiltradas a través de solicitudes no cifradas  

### ¿Soporta el servidor versiones de TLS débiles o suites de cifrado inseguras?
- [ ] No — solo TLS moderno (1.2+) y cifrados fuertes están **habilitados**  
- [ ] Sí — se **soportan** versiones legadas (TLS 1.0/1.1) o cifrados débiles (RC4, DES, CBC)  

### ¿Evita la aplicación la transmisión de credenciales a dominios de terceros a través de canales no cifrados?
- [ ] Sí — no se envían datos sensibles a dominios externos o todas las llamadas externas se fuerzan a través de HTTPS  
- [ ] No — las cabeceras de autenticación o credenciales se envían a endpoints de terceros (p. ej., analíticas o SSO) a través de HTTP  

---