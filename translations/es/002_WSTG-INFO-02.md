## WSTG-INFO-02 — Fingerprint Web Server

El fingerprinting de servidores web es el proceso de identificar el tipo de software específico, la versión y el sistema operativo subyacente de un servidor web objetivo. Los atacantes realizan este reconocimiento para identificar vulnerabilidades conocidas, como CVEs no parcheados o debilidades de configuración, que son específicos de ciertas versiones de Apache, Nginx, IIS u otras tecnologías de servidor. Esto se logra típicamente mediante el análisis de las cabeceras de respuesta HTTP (ej. `Server`, `X-Powered-By`), examinando comportamientos únicos del protocolo u observando información filtrada a través de páginas de error y archivos por defecto. Un fingerprinting exitoso permite a un atacante adaptar su estrategia de explotación, pasando de un escaneo genérico a ataques de alta probabilidad contra fallos arquitectónicos conocidos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No Realizado |
| **Severidad** | Informativo / Bajo* |

> *La severidad pasa a ser Alta si el fingerprinting revela una versión de servidor desactualizada o al final de su vida útil (EOL) con vulnerabilidades críticas conocidas.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Herramientas:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### ¿Existen cadenas de identificación presentes en las cabeceras de respuesta HTTP?
- [ ] No — las cabeceras `Server` y `X-Powered-By` **no están presentes** o contienen valores genéricos  
- [ ] Sí — las cabeceras revelan el software del servidor pero **no** los números de versión específicos  
- [ ] Sí — las cabeceras revelan el software específico del servidor y los números de versión **exactos**  

### ¿Las páginas de error por defecto o las respuestas generadas por el sistema filtran detalles del servidor?
- [ ] No — se utilizan páginas de error personalizadas y **no** revelan información del servidor  
- [ ] Sí — las páginas de error por defecto filtran el nombre y/o la versión del software del servidor  
- [ ] Sí — las páginas de error filtran detalles del servidor, la versión y el **sistema operativo subyacente**  

### ¿Se revela la versión del servidor a través de archivos por defecto o estructuras de directorios?
- [ ] No — los archivos de instalación por defecto, manuales y scripts de prueba han sido **eliminados**  
- [ ] Sí — los archivos por defecto (ej. `info.php`, `manual/`, `test.html`) están **presentes** y revelan datos de la versión  

### ¿Puede inferirse con precisión el tipo de servidor mediante el análisis de comportamientos únicos?
- [ ] No — las respuestas del servidor están normalizadas y el fingerprinting basado en comportamiento **no es posible**  
- [ ] Sí — los comportamientos únicos (ej. orden de las cabeceras, códigos de error específicos o particularidades de la pila TCP/IP) **permiten** la identificación del servidor  

### ¿La versión del servidor identificada cuenta actualmente con soporte y parches?
- [ ] Sí — la versión identificada es actual y cuenta con **soporte** por parte del fabricante  
- [ ] No — la versión identificada está **desactualizada** o se encuentra al final de su vida útil (EOL) con vulnerabilidades conocidas  

---