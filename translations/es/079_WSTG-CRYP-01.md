## WSTG-CRYP-01 — Pruebas de Seguridad Débil en la Capa de Transporte

Las pruebas de seguridad débil en la capa de transporte implican el análisis de la implementación y configuración de SSL/TLS para garantizar la confidencialidad e integridad de los datos durante el tránsito. Los atacantes aprovechan configuraciones incorrectas como protocolos obsoletos (SSLv2, TLS 1.0), suites de cifrado (cipher suites) débiles (NULL, RC4 o anónimas) y certificados inválidos o con firmas débiles para realizar ataques de Man-in-the-Middle (MitM). Esta prueba se dirige típicamente a los endpoints del servidor web o del balanceador de carga de la aplicación donde se establece la comunicación cifrada. Una explotación exitosa permite a un adversario interceptar datos sensibles, secuestrar sesiones de usuario o degradar las conexiones a estados inseguros mediante el Downgrade de protocolos o el descifrado de tráfico.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Media* |

> *La severidad es Alta si se transmiten datos sensibles (PII, credenciales, tokens de sesión); Media para tráfico de información general.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### ¿Se admiten protocolos TLS/SSL obsoletos o inseguros?
- [ ] No — solo TLS 1.2 y TLS 1.3 están **habilitados**  
- [ ] Sí — TLS 1.0 o TLS 1.1 están **habilitados**  
- [ ] Sí — SSLv2 o SSLv3 están **habilitados** *(Crítico)*  

### ¿El servidor acepta suites de cifrado (cipher suites) débiles o inseguras?
- [ ] No — solo se **soportan** cifrados fuertes y modernos (ej., AES-GCM, ChaCha20)  
- [ ] Sí — se **soportan** cifrados con encriptación débil (ej., 3DES, RC4) o longitud de bits baja  
- [ ] Sí — se **soportan** cifrados NULL, anónimos (ADH) o de exportación *(Crítico)*  

### ¿Es el certificado del servidor válido y confiable?
- [ ] Sí — el certificado es válido, coincide con el dominio y está firmado por una **CA de confianza**  
- [ ] No — el certificado está **expirado** o utiliza un **algoritmo de firma débil** (ej., SHA-1)  
- [ ] No — el certificado es **autofirmado** (self-signed) o existe una **discrepancia** (mismatch) en el nombre de dominio  

### ¿El servidor soporta Renegociación Segura y previene ataques de Downgrade?
- [ ] Sí — la Renegociación Segura (Secure Renegotiation) está **soportada** y TLS Fallback SCSV está **habilitado**  
- [ ] No — la Renegociación Segura **no está soportada**, lo que permite potencialmente una inyección MitM  
- [ ] No — el servidor es vulnerable a ataques **POODLE** o **ROBOT**  

### ¿Está HTTP Strict Transport Security (HSTS) correctamente implementado?

| Cabecera | Presente | Ausente |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Sí — la cabecera HSTS está presente con un `max-age` largo e `includeSubDomains` **habilitado**  
- [ ] Sí — la cabecera HSTS está presente pero falta `includeSubDomains` o tiene un **max-age corto**  
- [ ] No — la cabecera HSTS **no está presente**  

---