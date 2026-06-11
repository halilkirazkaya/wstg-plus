## WSTG-CONF-01 — Prueba de la Configuración de la Infraestructura de Red

Las pruebas de configuración de la infraestructura de red implican identificar debilidades en el servidor subyacente y en los componentes de red que soportan la aplicación web. Los atacantes se dirigen a servicios mal configurados, protocolos obsoletos y puertos abiertos innecesarios para obtener un punto de apoyo, realizar movimientos laterales o llevar a cabo labores de reconocimiento. Los hallazgos comunes incluyen versiones de TLS inseguras, banners de servicio detallados e interfaces administrativas expuestas que deberían estar restringidas a redes internas. Al explotar estos fallos a nivel de infraestructura, un adversario puede comprometer la confidencialidad de los datos en tránsito o ganar acceso no autorizado al sistema operativo del host.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Herramientas:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### ¿Existen puertos abiertos o servicios innecesarios expuestos en el host de la aplicación?
- [ ] No — solo los puertos requeridos (por ejemplo, 443) son **accesibles**  
- [ ] Sí — hay servicios adicionales expuestos pero siguen una configuración **segura**  
- [ ] Sí — servicios no esenciales (por ejemplo, FTP, Telnet, SMB) están **expuestos** públicamente  

### ¿Los banners de servicio o cabeceras filtran información sensible de la versión?
- [ ] No — los banners han sido eliminados o son genéricos  
- [ ] Sí — los banners revelan el software del servidor (por ejemplo, Apache/nginx) pero **no** los números de versión  
- [ ] Sí — los banners detallados revelan versiones específicas de software, lo que facilita la **explotación**  

### ¿Sigue la configuración de TLS las mejores prácticas de seguridad modernas?
- [ ] Sí — solo TLS 1.2/1.3 está **habilitado** con suites de cifrado fuertes  
- [ ] Sí — los protocolos heredados (TLS 1.0/1.1) están **habilitados**  
- [ ] No — cifrados débiles o protocolos inseguros (SSLv2/v3) están **habilitados**  

### ¿Están las interfaces administrativas o de gestión restringidas a redes autorizadas?
- [ ] Sí — las interfaces (por ejemplo, SSH, RDP, paneles de control) **no son accesibles** desde la internet pública  
- [ ] No — las interfaces son accesibles públicamente pero requieren autenticación de múltiples factores (**MFA**)  
- [ ] No — las interfaces son accesibles públicamente con autenticación de un solo factor o credenciales por defecto  

---