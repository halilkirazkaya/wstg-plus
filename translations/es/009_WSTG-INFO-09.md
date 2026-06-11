## WSTG-INFO-09 — Fingerprint Web Application

El Fingerprinting de una aplicación web es el proceso de identificar el framework específico, el Sistema de Gestión de Contenidos (CMS) o el stack tecnológico subyacente y sus versiones asociadas. Esta fase de reconocimiento es vital porque permite a un atacante mapear los componentes identificados con vulnerabilidades conocidas (CVEs) y exploits públicos, reduciendo significativamente la superficie de ataque. La información se recolecta típicamente de las cabeceras de respuesta HTTP, rutas de archivos únicas, nombres de cookies, metadatos del código fuente HTML y hashes criptográficos de recursos estáticos. Al determinar con precisión el stack tecnológico, un atacante puede pasar de un escaneo automatizado genérico a una explotación altamente dirigida de debilidades específicas de la versión.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Baja / Informativa |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### ¿Revelan las cabeceras de respuesta HTTP el stack tecnológico o los números de versión?
- [ ] No — las cabeceras sensibles han sido eliminadas o contienen valores genéricos  
- [ ] Sí — cabeceras por defecto como `X-Powered-By`, `Server` o `X-AspNet-Version` **están** presentes  
- [ ] Sí — las cabeceras revelan versiones específicas y desactualizadas del servidor web o del framework de la aplicación  

### ¿Es identificable el framework de la aplicación o el CMS a través de rutas de archivos únicas o convenciones de nombres?
- [ ] No — las rutas de archivos están ofuscadas y **no** revelan la tecnología subyacente  
- [ ] Sí — estructuras de directorios comunes (ej. `/wp-content/`, `/_next/`, `/node_modules/`) **son** visibles  
- [ ] Sí — archivos únicos como `robots.txt`, `humans.txt` o `sitemap.xml` contienen metadatos específicos de la tecnología  

### ¿Se exponen números de versión específicos dentro del código fuente HTML o en los recursos estáticos?
- [ ] No — no se encuentra información de versiones en comentarios o parámetros de recursos  
- [ ] Sí — existen comentarios HTML o etiquetas meta (ej. `<meta name="generator" content="...">`)  
- [ ] Sí — se añaden cadenas de versión a los nombres de archivos CSS/JS (ej. `jquery.js?v=1.12.4`) y **no** pueden desactivarse  

### ¿Revelan las páginas de error por defecto o las interfaces de gestión el entorno de software?
- [ ] No — la aplicación utiliza páginas de error personalizadas y genéricas para todos los códigos de estado  
- [ ] Sí — las páginas de error por defecto del servidor web o framework **están** habilitadas y filtran datos de versión  
- [ ] Sí — los portales de inicio de sesión administrativo (ej. `/wp-admin`, `/phpmyadmin`) **son** accesibles e identificables  

### ¿Revelan las cookies el framework de gestión de sesiones?
- [ ] No — las cookies de sesión utilizan nombres genéricos o personalizados  
- [ ] Sí — se utilizan nombres de cookies específicos de la tecnología (ej. `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`)  

---