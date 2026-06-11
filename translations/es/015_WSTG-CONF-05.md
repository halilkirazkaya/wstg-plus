## WSTG-CONF-05 — Enumeración de interfaces de administración de la infraestructura y la aplicación

Las interfaces administrativas representan el plano de control de gestión de una aplicación y su infraestructura subyacente, otorgando a menudo acceso privilegiado a las configuraciones del sistema, datos de usuario y operaciones del lado del servidor. Los atacantes priorizan la enumeración de estas interfaces a través de ataques de fuerza bruta de directorios (directory brute-forcing), el análisis de `robots.txt` o la observación de patrones de URL predecibles para encontrar puntos de entrada que puedan carecer de una autenticación robusta o que dependan de credenciales por defecto. El descubrimiento de un panel de administración expuesto aumenta significativamente el riesgo de un compromiso total del sistema, la modificación no autorizada de datos o la interrupción del servicio. Estas interfaces se encuentran frecuentemente en puertos no estándar o subdominios, lo que las convierte en objetivos principales para los escáneres automatizados y el reconocimiento manual.


| Campo | Valor |
|---|---|
| **ID WSTG** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si la interfaz permite cambios de configuración en todo el sistema, gestión de usuarios o exfiltración de datos sin autenticación multifactor.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### ¿Son las interfaces administrativas detectables mediante el fuzzeo de directorios o el reconocimiento?
- [ ] No — no hay interfaces administrativas expuestas o detectables  
- [ ] Sí — las interfaces son detectables pero el acceso **está restringido** a rangos de IP internos  
- [ ] Sí — las interfaces **son** detectables y accesibles públicamente  

### ¿Se aplica la autenticación en los portales administrativos descubiertos?
- [ ] Sí — se **exige** autenticación multifactor (MFA)  
- [ ] Sí — se **exige** autenticación de factor único  
- [ ] No — no se requiere autenticación para acceder a la interfaz *(Crítico)*  

### ¿Están las rutas administrativas ocultas o son no estándar para evitar su descubrimiento?
- [ ] Sí — las rutas utilizan nombres no estándar, aleatorios o no predecibles  
- [ ] No — las rutas utilizan convenciones de nomenclatura comunes (por ejemplo, `/admin`, `/manager`, `/console`) y **pueden** ser adivinadas fácilmente  

### ¿Proporciona la interfaz acceso a funcionalidades sensibles del sistema o de la aplicación?
- [ ] No — la interfaz es un panel de estado de "solo lectura" con un impacto mínimo  
- [ ] Sí — la interfaz **puede** modificar la configuración de la aplicación o los datos de los usuarios  
- [ ] Sí — la interfaz **puede** ejecutar comandos a nivel de sistema o realizar la gestión de bases de datos  

---