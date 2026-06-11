## WSTG-ATHN-02 — Pruebas de Credenciales por Defecto

Las pruebas de credenciales por defecto consisten en identificar interfaces administrativas, servicios o componentes de aplicaciones que todavía utilizan nombres de usuario y contraseñas configurados de fábrica o proporcionados por el proveedor. Esta vulnerabilidad es un objetivo de alta prioridad para los atacantes porque, a menudo, proporciona una ruta inmediata al compromiso total del sistema, acceso administrativo o ejecución remota de comandos (Remote Code Execution) con un esfuerzo mínimo. Ocurre típicamente en software comercial (off-the-shelf), plataformas CMS, herramientas de gestión de bases de datos y hardware integrado como dispositivos IoT o dispositivos de red (network appliances). La explotación se realiza habitualmente cruzando las versiones de software identificadas con bases de datos públicas de contraseñas por defecto o utilizando herramientas automatizadas de Credential Stuffing durante las fases iniciales de reconocimiento y explotación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Crítica / Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Herramientas:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### ¿Están las interfaces administrativas o de gestión expuestas a Internet o a segmentos de red no confiables?
- [ ] No — no hay interfaces de gestión accesibles  
- [ ] Sí — las interfaces están presentes pero restringidas por controles a nivel de red (ej. VPN, IP Allowlist)  
- [ ] Sí — las interfaces **son** públicamente accesibles y requieren autenticación  

### ¿Se han cambiado las credenciales por defecto suministradas por el proveedor para todos los componentes identificados?
- [ ] Sí — todas las credenciales por defecto **se han cambiado** en todos los componentes *(Más seguro)*  
- [ ] Sí — las credenciales **se han cambiado** para la aplicación principal, pero los componentes secundarios (ej. CMS, DB) permanecen con los valores por defecto  
- [ ] No — las credenciales por defecto **están activas** en una o más interfaces identificables *(Crítico)*  

### ¿La aplicación obliga a realizar un cambio de contraseña en el primer inicio de sesión para cuentas nuevas o administrativas?
- [ ] Sí — el cambio de contraseña **es obligatorio** antes de que se pueda realizar cualquier acción administrativa  
- [ ] No — la aplicación **permite** el uso de credenciales por defecto indefinidamente  

### ¿Están activos los mecanismos de bloqueo o controles de Rate Limiting para prevenir pruebas automatizadas de credenciales por defecto?
- [ ] Sí — se aplica un Rate Limiting agresivo o bloqueo de cuentas  
- [ ] Sí — existen controles pero el **Bypass** es posible mediante manipulación de cabeceras o rotación de IP  
- [ ] No — no hay mecanismos de Rate Limiting o de bloqueo **habilitados**  

### ¿Los complementos (plugins) de terceros, middleware o herramientas administrativas (ej. phpMyAdmin, Tomcat Manager) utilizan credenciales únicas?
- [ ] No — no existen componentes de terceros en el entorno  
- [ ] Sí — todos los componentes de terceros utilizan credenciales únicas y no predeterminadas  
- [ ] No — al menos un componente de terceros **es accesible** a través de credenciales por defecto conocidas  

---