## WSTG-INFO-03 — Revisión de Metaarchivos del Servidor Web para Fuga de Información (Information Leakage)

Los metaarchivos del servidor web como `robots.txt`, `sitemap.xml` y `security.txt` están destinados a guiar a los rastreadores (crawlers) y proporcionar metadatos administrativos, pero a menudo revelan inadvertidamente estructuras de directorios sensibles o endpoints ocultos. Los atacantes analizan estos archivos para enumerar la superficie de ataque (attack surface) de la aplicación, identificando directivas "Disallow" que frecuentemente apuntan a paneles administrativos no enlazados, directorios de respaldo (backup) o entornos de desarrollo. Más allá de las instrucciones para motores de búsqueda, los archivos en el directorio `.well-known/` o archivos como `humans.txt` pueden divulgar stacks tecnológicos, información de los desarrolladores o contactos de seguridad de la organización. La revisión sistemática de estos archivos permite a un tester obtener información estructural y técnica sin necesidad de realizar un descubrimiento agresivo por fuerza bruta (Brute Force).


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No realizada |
| **Severidad** | Baja / Informativa* |

> *La severidad aumenta a Media si los metaarchivos revelan interfaces administrativas no autenticadas o respaldos de configuración sensibles.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### ¿Existe el archivo `robots.txt` y expone directorios sensibles?
- [ ] No — `robots.txt` **no** existe  
- [ ] Sí — `robots.txt` existe pero solo contiene rutas públicas estándar  
- [ ] Sí — `robots.txt` revela rutas de directorios sensibles o interfaces administrativas  
- [ ] Sí — `robots.txt` revela credenciales o versiones específicas de software en los comentarios  

### ¿Está presente el archivo `sitemap.xml` y revela la estructura oculta de la aplicación?
- [ ] No — `sitemap.xml` **no** existe  
- [ ] Sí — `sitemap.xml` está presente pero solo enumera URLs de acceso público  
- [ ] Sí — `sitemap.xml` enumera endpoints no enlazados o recursos internos que **pueden** ser accedidos  

### ¿Exponen detalles técnicos los metaarchivos de seguridad y organizacionales?
- [ ] No — no se encontraron archivos de metadatos en la raíz ni en `.well-known/`  
- [ ] Sí — `security.txt` o `humans.txt` están presentes pero contienen información estándar  
- [ ] Sí — los metaarchivos divulgan hostnames internos, identidades de desarrolladores o detalles del stack tecnológico que **pueden** ayudar en ataques de ingeniería social o posteriores ataques  

### ¿Existen metaarchivos legados o específicos de algún framework?
- [ ] No — no se encontraron archivos específicos de frameworks (ej. `info.php`, `composer.json`, `package.json`)  
- [ ] Sí — archivos como `README.md`, `CHANGELOG.txt` o `.DS_Store` están presentes pero saneados  
- [ ] Sí — archivos específicos de framework o versión están presentes y **permiten** un fingerprinting tecnológico preciso  

---