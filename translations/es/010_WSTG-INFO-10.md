## WSTG-INFO-10 — Mapeo de la Arquitectura de la Aplicación

El mapeo de la arquitectura de la aplicación implica identificar las tecnologías subyacentes, los componentes de infraestructura y las rutas de flujo de datos que sustentan la aplicación web. Al analizar los encabezados de respuesta HTTP, mensajes de error, formatos de cookies y extensiones de archivo, los evaluadores pueden reconstruir un esquema de los servidores web, servidores de aplicaciones, bases de datos e integraciones de terceros en uso. Este conocimiento es fundamental para personalizar los ataques posteriores, ya que permite la selección de exploits específicos de la plataforma y la identificación de posibles cuellos de botella o dispositivos intermediarios mal configurados, como equilibradores de carga y WAFs. Un atacante aprovecha este mapa estructural para pasar de un escaneo genérico a una explotación dirigida del software stack específico y sus componentes interconectados.

| Campo | Valor |
|---|---|
| **ID de WSTG** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Informativa / Baja* |

> *La severidad se convierte en Media si se revela la topología de la red interna o direcciones IP privadas.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### ¿Se pueden identificar con precisión las tecnologías del servidor web y del servidor de aplicaciones?
- [ ] No — no es posible la identificación debido a un hardening u ofuscación efectivos  
- [ ] Sí — se conoce el tipo de tecnología pero no se pueden determinar las versiones específicas  
- [ ] Sí — la tecnología y las versiones específicas se revelan completamente a través de encabezados o firmas de archivos *(Informativa)*  

### ¿Son detectables los dispositivos intermediarios (WAFs, Equilibradores de Carga, Proxies) en la ruta de comunicación?
- [ ] No — no se detectan intermediarios o son completamente transparentes  
- [ ] Sí — se sospecha su existencia debido al tiempo de respuesta o al comportamiento, pero su identidad **no** está confirmada  
- [ ] Sí — se identifican productos específicos (p. ej., Cloudflare, Nginx, F5) a través de encabezados únicos o comportamiento  

### ¿Filtra la aplicación información sobre su estructura de directorios internos o la topología de la red?
- [ ] No — las rutas internas y las IPs **no** se revelan  
- [ ] Sí — se filtran rutas internas en mensajes de error o stack traces, pero las IPs internas **no** se revelan  
- [ ] Sí — se revelan tanto las estructuras de directorios internos como las direcciones IP privadas  

### ¿Se puede inferir el tipo y la versión de la base de datos del backend a partir del comportamiento de la aplicación?
- [ ] No — no se pueden determinar los detalles de la base de datos  
- [ ] Sí — se infiere el tipo de base de datos (p. ej., PostgreSQL, MSSQL) a través de mensajes de error o comportamiento  
- [ ] Sí — el tipo y la versión de la base de datos se revelan explícitamente en los cuerpos de respuesta o encabezados  

### ¿Está la aplicación alojada en proveedores de servicios en la nube identificables o en CDNs de terceros específicos?
- [ ] No — la infraestructura de alojamiento **no** es identificable  
- [ ] Sí — se identifica el proveedor de la nube (AWS, Azure, GCP) pero los servicios específicos **no**  
- [ ] Sí — los servicios en la nube específicos (p. ej., buckets S3, Lambda, Azure Functions) están mapeados y son accesibles  

---