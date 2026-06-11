## WSTG-INFO-01 — Realización de descubrimiento y reconocimiento mediante motores de búsqueda para la fuga de información

El descubrimiento mediante motores de búsqueda implica el aprovechamiento de motores de búsqueda públicos, páginas en caché y servicios de indexación para identificar información que la organización objetivo ha expuesto de forma involuntaria. Los atacantes utilizan operadores de búsqueda avanzada (`Google Dorks`) y servicios de indexación de terceros para descubrir archivos sensibles, directorios, portales de inicio de sesión, mensajes de error y metadatos que no deberían ser accesibles públicamente. Esta fase de reconocimiento es crítica porque no requiere interacción directa con el objetivo, lo que la hace prácticamente indetectable, mientras que potencialmente revela puntos de entrada de alto valor como paneles de administración expuestos, archivos de configuración, volcados de bases de datos (*database dumps*) y documentación interna.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Herramientas:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### ¿Están indexados archivos o directorios sensibles por los motores de búsqueda?
- [ ] No — no se encontró contenido sensible en los resultados de los motores de búsqueda  
- [ ] Sí — archivos de configuración, respaldos o documentos internos **están** indexados *(Crítico)*  

### ¿Revela el `Google Dorking` interfaces administrativas o de inicio de sesión expuestas?
- [ ] No — los paneles de administración y las páginas de inicio de sesión **no** están indexados  
- [ ] Sí — los paneles de administración **están** indexados pero requieren una autenticación de múltiples factores robusta  
- [ ] Sí — los paneles de administración **están** indexados y son accesibles públicamente **sin** controles suficientes  

### ¿Las versiones en caché de las páginas exponen datos sensibles que ya han sido eliminados?
- [ ] No — no se encontraron datos sensibles en las capturas de caché  
- [ ] Sí — las páginas en caché contienen credenciales, direcciones IP internas o información sensible  

### ¿El archivo `robots.txt` está divulgando involuntariamente rutas sensibles?
- [ ] No — `robots.txt` **no** revela estructuras de directorios sensibles  
- [ ] Sí — `robots.txt` enumera rutas sensibles que ayudan al reconocimiento del atacante  
- [ ] No existe el archivo `robots.txt`  

### ¿Los servicios de terceros (`Shodan`, `Censys`, `Wayback Machine`) están revelando una exposición histórica o actual?
- [ ] No — no hay hallazgos significativos en los servicios de indexación de terceros  
- [ ] Sí — las capturas históricas o los escaneos de servicios revelan metadatos sensibles o *banners* de servicios  

---