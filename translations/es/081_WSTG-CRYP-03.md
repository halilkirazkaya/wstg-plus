## WSTG-CRYP-03 — Pruebas de información sensible enviada a través de canales no cifrados

La transmisión de datos sensibles a través de canales no cifrados, como HTTP en texto claro, expone la información a la interceptación y modificación mediante ataques Man-in-the-Middle (MITM). Esta vulnerabilidad suele ocurrir cuando las aplicaciones web no logran imponer Transport Layer Security (TLS) en todos los endpoints, particularmente en componentes legados, formularios de inicio de sesión o durante la transición inicial de HTTP a HTTPS. Desde la perspectiva de un atacante, esto permite el sniffing pasivo de credenciales de autenticación, tokens de sesión e información de identificación personal (PII) en segmentos de red comprometidos o no confiables, como redes Wi-Fi públicas. Garantizar que todos los datos sensibles estén encapsulados dentro de un túnel TLS robusto es fundamental para mantener la confidencialidad e integridad de las interacciones del usuario y prevenir el secuestro de sesión (session hijacking).


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Media |

> *La severidad pasa a ser Alta si las credenciales de autenticación, los identificadores de sesión o la PII se transmiten en texto claro.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### ¿Permite la aplicación la comunicación a través de HTTP no cifrado para endpoints sensibles?
- [ ] No — todo el tráfico de la aplicación se fuerza estrictamente a través de HTTPS *(Más seguro)*  
- [ ] Sí — las páginas no sensibles son accesibles a través de HTTP, pero los endpoints sensibles **no lo son**  
- [ ] Sí — los endpoints sensibles (login, checkout, perfil) **son** accesibles a través de HTTP no cifrado *(Riesgo alto)*  

### ¿Existe una redirección global de HTTP a HTTPS?
- [ ] Sí — la aplicación realiza una redirección 301/302 a HTTPS para todas las peticiones HTTP  
- [ ] Sí — la redirección solo se realiza para endpoints sensibles específicos  
- [ ] No — **no se aplica** ninguna redirección de HTTP a HTTPS  

### ¿Está implementado HTTP Strict Transport Security (HSTS)?
- [ ] Sí — HSTS está **habilitado** con un `max-age` prolongado e incluye las directivas `includeSubDomains` y `preload`  
- [ ] Sí — HSTS está **habilitado** pero carece de las directivas `includeSubDomains` o `preload`  
- [ ] No — HSTS **no está habilitado** en el servidor de aplicaciones  

### ¿Están las cookies sensibles protegidas contra la transmisión en texto claro?

| Nombre de la cookie | Flag Secure presente | Flag Secure ausente |
|---------------------|:--------------------:|:-------------------:|
| `SessionID`         |                      |                     |
| `AuthToken`         |                      |                     |
| `CSRF-Token`        |                      |                     |

### ¿Transmite la aplicación datos sensibles a APIs de terceros a través de canales no cifrados?
- [ ] No — todas las llamadas a APIs externas se realizan a través de túneles cifrados (HTTPS)  
- [ ] Sí — se envía cierta telemetría no sensible a través de HTTP  
- [ ] Sí — **se envían** claves de API, PII o credenciales a terceros a través de HTTP no cifrado  

---