## WSTG-CONF-14 — Prueba de otras configuraciones incorrectas de cabeceras de seguridad HTTP

La prueba de configuraciones incorrectas de cabeceras de seguridad HTTP consiste en verificar la presencia e implementación correcta de cabeceras defensivas que proporcionan una protección esencial contra vectores de ataque comunes basados en la web. Aunque estas cabeceras no corrigen vulnerabilidades de código subyacentes, su ausencia aumenta significativamente el riesgo de ataques man-in-the-middle (MITM), explotación de MIME-sniffing y fugas no intencionadas de datos cross-origin a través de referrers. Desde la perspectiva de un atacante, la falta de cabeceras de seguridad es un indicador visible de un entorno poco robustecido, lo que a menudo facilita cadenas de explotación más complejas como el secuestro de sesión (session hijacking) o la exfiltración de información. Estas configuraciones se evalúan normalmente a nivel de servidor y deben aplicarse de forma consistente en todos los tipos de respuesta, incluidos los endpoints de API y los recursos estáticos.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Auditoría de presencia de cabeceras de seguridad
| Cabecera | Presente | Ausente | Configuración recomendada |
|----------|:--------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` o `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Específica de funciones, ej., `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### ¿Cómo se aplican las cabeceras de seguridad en el alcance de la aplicación?
- [ ] Sí — las cabeceras se aplican globalmente a través de un balanceador de carga central o un proxy inverso y **no pueden** ser sobrescritas  
- [ ] Sí — las cabeceras se aplican a las respuestas de los documentos principales, pero **faltan** en las respuestas de la API o en los recursos estáticos  
- [ ] No — las cabeceras se aplican de forma inconsistente o **faltan** en todo el dominio de la aplicación  

### ¿Está la cabecera Strict-Transport-Security (HSTS) configurada correctamente?
- [ ] Sí — `max-age` está configurado con una duración larga (ej., 2 años) e `includeSubDomains` está **habilitado**  
- [ ] Sí — `max-age` está configurado, pero `includeSubDomains` está **deshabilitado**, dejando los subdominios vulnerables  
- [ ] No — la cabecera HSTS **no está presente** o `max-age` está configurado en 0, lo que permite ataques de degradación de protocolo  

### ¿Evita la Referrer-Policy la fuga de parámetros sensibles en la URL?
- [ ] Sí — la política está establecida en `no-referrer` o `same-origin` *(Más seguro)*  
- [ ] Sí — la política está establecida en `strict-origin-when-cross-origin`  
- [ ] No — la política **no está establecida** o está en `unsafe-url`, lo que podría filtrar tokens o información de identificación personal (PII) sensible en las cabeceras referer  

### ¿La cabecera X-Content-Type-Options evita el MIME-sniffing?
- [ ] Sí — la directiva `nosniff` está **presente** y correctamente implementada  
- [ ] No — la cabecera **falta**, lo que podría permitir que un navegador ejecute archivos no ejecutables (ej., imágenes) como scripts  

---