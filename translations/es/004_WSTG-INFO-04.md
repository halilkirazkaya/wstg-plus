## WSTG-INFO-04 — Enumeración de Aplicaciones en el Servidor Web

La enumeración de aplicaciones es el proceso de identificar todas las aplicaciones web y servicios alojados en un único servidor web o dirección IP. Debido a que los servidores web modernos suelen alojar múltiples Virtual Hosts, entornos de staging heredados o interfaces administrativas en puertos y rutas no estándar, el no identificar estos activos ocultos deja una parte significativa de la superficie de ataque sin evaluar. Los atacantes utilizan técnicas como DNS zone transfers, Virtual Host Brute-forcing y Reverse IP lookups para descubrir estos puntos de entrada olvidados u oscurecidos, que con frecuencia son menos seguros que la aplicación de producción principal.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Informativa / Baja |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Herramientas:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### ¿Existen múltiples direcciones IP o alias de DNS asociados con la infraestructura objetivo?
- [ ] No — solo existen una única IP y un registro A  
- [ ] Sí — se identifican múltiples direcciones IP o alias, ampliando el alcance  

### ¿Aloja el servidor web múltiples Virtual Hosts (vhosts) en la misma IP?
- [ ] No — el servidor solo sirve el dominio principal  
- [ ] Sí — se identifican Virtual Hosts adicionales pero el acceso **no es posible** (ej. 403 Forbidden)  
- [ ] Sí — se identifican y son accesibles Virtual Hosts ocultos, de desarrollo o de staging  

### ¿Hay aplicaciones alojadas en puertos no estándar?
- [ ] No — solo están abiertos los puertos estándar (80/443)  
- [ ] Sí — hay puertos no estándar abiertos pero **no** alojan aplicaciones web  
- [ ] Sí — se identifican aplicaciones web adicionales en puertos no estándar (ej. 8080, 8443, 9000)  

### ¿Existen aplicaciones distintas mapeadas a rutas URI específicas?
- [ ] No — una única aplicación sirve todo el directorio raíz  
- [ ] Sí — se descubren múltiples aplicaciones a través de enumeración de directorios (ej. `/api`, `/portal`, `/backup`)  

### ¿Los servicios de reconocimiento de terceros revelan aplicaciones históricas o secundarias?
- [ ] No — no hay hallazgos significativos en Shodan, Censys o Wayback Machine  
- [ ] Sí — las capturas históricas o los escaneos de servicios revelan aplicaciones que **están** actualmente activas pero no enlazadas  

---