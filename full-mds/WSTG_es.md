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

## WSTG-INFO-05 — Revisión del contenido de la página web para detectar fugas de información

La revisión del contenido de la página web implica la inspección manual y automatizada de las respuestas de la aplicación, incluyendo archivos HTML, JavaScript y CSS, para identificar información sensible expuesta involuntariamente a los usuarios. Los atacantes analizan minuciosamente el código fuente del lado del cliente en busca de comentarios de desarrolladores, campos de formulario ocultos, rutas internas del servidor, claves API (API keys) integradas en el código e información de depuración (debugging) que pueda ayudar en fases posteriores de explotación. Esta fuga ocurre a menudo cuando los desarrolladores olvidan eliminar metadatos o código de depuración antes de pasar a producción, revelando potencialmente fallos de lógica o detalles de la infraestructura del backend. Desde la perspectiva de un atacante, esto proporciona un método de bajo esfuerzo para mapear la estructura interna de la aplicación y descubrir puntos de entrada ignorados sin activar alertas de seguridad ni interactuar con la lógica del backend del servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Baja / Media* |

> *La severidad se vuelve Alta si se descubren credenciales integradas en el código (hardcoded), claves privadas o variables de entorno internas.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Herramientas:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### ¿Existen comentarios de desarrolladores que contengan información sensible en el código fuente?
- [ ] No — no se encontraron comentarios o solo se hallaron comentarios genéricos no sensibles  
- [ ] Sí — existen comentarios pero **no** contienen lógica ni datos sensibles  
- [ ] Sí — los comentarios **revelan** rutas internas, consultas SQL o instrucciones administrativas  
- [ ] Sí — los comentarios **revelan** credenciales o notas sensibles de los desarrolladores *(Alta)*  

### ¿Los campos de entrada HTML ocultos contienen datos sensibles o información de estado?
- [ ] No — no se encontraron campos ocultos o solo contienen tokens no sensibles  
- [ ] Sí — los campos ocultos se utilizan para el estado pero están **cifrados** o **firmados**  
- [ ] Sí — los campos ocultos contienen contraseñas en **texto plano**, roles de usuario o IDs internos  

### ¿Hay secretos integrados (hardcoded), claves API o direcciones IP internas en los archivos JavaScript?
- [ ] No — no se encontraron cadenas sensibles ni secretos en los archivos JS  
- [ ] Sí — los archivos JS contienen claves API públicas con las restricciones **adecuadas**  
- [ ] Sí — los archivos JS contienen claves API **no protegidas**, IPs internas o endpoints privados  
- [ ] Sí — los archivos JS contienen credenciales administrativas o claves privadas **hardcoded** *(Crítica)*  

### ¿La aplicación revela versiones del framework o rutas de archivos internas en el código fuente?
- [ ] No — la información del framework y las rutas de archivos están **minificadas** u **ofuscadas**  
- [ ] Sí — los nombres y versiones del framework son **visibles** pero están parcheados  
- [ ] Sí — las rutas de archivos internas o rutas absolutas del servidor están **expuestas** en el código fuente  

### ¿Es el código fuente propenso a fugas debido a la falta de minificación u ofuscación?
- [ ] No — el código de producción está **completamente minificado** y se han eliminado los comentarios  
- [ ] Sí — el código está minificado pero los comentarios **permanecen** en el fuente  
- [ ] Sí — los mapas de fuente (archivos `.map`) son **públicamente accesibles**, lo que permite la reconstrucción completa del código fuente

---

## WSTG-INFO-06 — Identificación de puntos de entrada de la aplicación

La identificación de los puntos de entrada de la aplicación consiste en mapear toda la superficie de ataque enumerando todas las formas posibles en que un usuario o un sistema externo puede interactuar con la aplicación. Este proceso incluye el descubrimiento de todas las URLs, endpoints de API, parámetros (GET, POST, basados en cookies), encabezados HTTP y protocolos especializados como WebSockets o gRPC. Para un pentester, esta etapa es vital porque las vulnerabilidades se encuentran a menudo en APIs "en la sombra" (shadow APIs) no documentadas, endpoints legados o estructuras de parámetros complejas que se pasaron por alto durante el desarrollo. Una enumeración exhaustiva garantiza que ningún componente de la aplicación permanezca como una caja negra sin probar, evitando así que los atacantes encuentren rutas no monitoreadas hacia el sistema.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Informativa |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### ¿Se han enumerado todos los parámetros GET y POST en toda la aplicación?
- [ ] Sí — se ha completado la enumeración exhaustiva mediante crawling manual y automatizado
- [ ] Sí — solo se han identificado parámetros comunes mediante crawling estándar
- [ ] No — los parámetros están mayoritariamente **sin identificar** o sin probar

### ¿Se buscan parámetros no documentados u ocultos (p. ej., debug, admin) utilizando fuzzing?
- [ ] No — el descubrimiento de parámetros ocultos **no es necesario** para este alcance específico
- [ ] Sí — se ha realizado fuzzing de parámetros (p. ej., usando `Arjun`) **sin hallazgos**
- [ ] Sí — se **descubrieron** parámetros ocultos y se mapearon para pruebas posteriores
- [ ] No — **no se realizó** el descubrimiento de parámetros ocultos

### ¿Se han identificado todos los métodos HTTP soportados (PUT, DELETE, PATCH, etc.) para cada endpoint?
- [ ] Sí — el descubrimiento de métodos **se aplica** a todos los endpoints descubiertos
- [ ] Sí — el descubrimiento de métodos **se aplica** solo a endpoints de alto interés o sensibles
- [ ] No — solo **se identifican** los métodos estándar GET y POST

### ¿Se han mapeado los puntos de entrada no estándar como WebSockets, gRPC o encabezados personalizados?
- [ ] No — la aplicación **no utiliza** protocolos no estándar ni encabezados personalizados
- [ ] Sí — se **identifican** todos los puntos de entrada específicos de protocolos y encabezados personalizados
- [ ] No — **existen** puntos de entrada no estándar pero no se han mapeado

### ¿Se ha mapeado completamente la superficie de la API (incluyendo endpoints con versiones como /v1/ o /v2/)?
- [ ] No — la aplicación **no expone** una API
- [ ] Sí — todas las versiones y endpoints de la API **están identificados** y documentados
- [ ] Sí — solo **se identifica** la versión actual de la API en producción
- [ ] No — los endpoints de la API **permanecen sin mapear** o solo se han descubierto parcialmente

---

## WSTG-INFO-07 — Mapeo de rutas de ejecución a través de la aplicación

El mapeo de rutas de ejecución consiste en la identificación sistemática de todos los endpoints accesibles, flujos funcionales y ramas de toma de decisiones dentro de una aplicación web. Este proceso es vital para garantizar que ninguna funcionalidad "oscura" —como código heredado, interfaces de depuración o rutas de API no documentadas— permanezca oculta al escrutinio de seguridad, ya que estas suelen carecer de controles de seguridad modernos. Los atacantes utilizan una combinación de spidering automatizado y exploración manual para visualizar la estructura de la aplicación, descubriendo a menudo vulnerabilidades como el acceso administrativo no autorizado o fallos en la lógica de negocio durante el proceso. Al correlacionar las entradas con respuestas específicas del servidor, los evaluadores pueden identificar con precisión la lógica de procesamiento sensible que requiere pruebas de análisis profundo dirigidas para garantizar una cobertura robusta.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Informativa |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### ¿Se ha indexado completamente la aplicación utilizando crawling automático y manual?
- [ ] Sí — se ha completado un mapa exhaustivo de todos los recursos visibles y enlazados  
- [ ] Sí — se ha completado el crawling automático, pero la exploración manual está pendiente  
- [ ] No — la aplicación no ha sido indexada  

### ¿Se han identificado endpoints ocultos o no documentados mediante análisis de JS o Brute Force de directorios?
- [ ] No — no se han descubierto rutas no documentadas a través de análisis de canal lateral  
- [ ] Sí — se han descubierto rutas ocultas, pero no se puede acceder a ellas sin autorización  
- [ ] Sí — los endpoints no documentados son accesibles y proporcionan funcionalidad sensible *(Crítica / Alta)*  

### ¿Pueden alterarse las rutas de ejecución mediante parámetros o cabeceras no estándar?
- [ ] No — parámetros como `debug`, `test` o `admin` no afectan al flujo de ejecución  
- [ ] Sí — los parámetros cambian la salida pero no eluden los controles de seguridad  
- [ ] Sí — se aplican parámetros que alteran la ruta y permiten eludir la lógica prevista  

### ¿Está completamente mapeado el flujo de la lógica de negocio de varios pasos (p. ej., autenticación multifactor, checkout)?
- [ ] Sí — se han identificado todos los estados y transiciones en los procesos de varios pasos  
- [ ] No — las transiciones complejas de la máquina de estados no pueden mapearse por completo  

### ¿Están expuestos los archivos de documentación de API (Swagger/OpenAPI) o los mapas del lado del cliente (Source Maps)?
- [ ] No — no hay mapas arquitectónicos ni archivos de documentación expuestos públicamente  
- [ ] Sí — la documentación está presente pero está protegida por autenticación  
- [ ] Sí — Swagger UI, OpenAPI JSON o JS Source Maps están habilitados y son accesibles públicamente

---

## WSTG-INFO-08 — Fingerprint Web Application Framework

El fingerprinting (identificación de huellas) de un framework de aplicaciones web implica identificar el stack tecnológico específico y las versiones de software utilizadas para construir y servir la aplicación. Los atacantes analizan las cabeceras (headers) de respuesta HTTP, las cookies específicas del framework, las estructuras de archivos predecibles y los artefactos de JavaScript únicos para determinar si el objetivo está ejecutando plataformas como React, Angular, Django o Spring. Esta fase de reconocimiento es vital porque permite al evaluador reducir la superficie de ataque a vulnerabilidades conocidas (CVE) y debilidades de configuración comunes específicas de ese framework. Al comprender la arquitectura subyacente, un atacante puede pasar de pruebas genéricas a estrategias de explotación (exploit) altamente dirigidas.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Informativa |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### ¿Las cabeceras (headers) de respuesta HTTP exponen el nombre o la versión del framework?
- [ ] No — las cabeceras como `X-Powered-By` o `Server` **no** están presentes o han sido saneadas  
- [ ] Sí — el nombre del framework está presente pero la versión **está oculta**  
- [ ] Sí — tanto el nombre del framework como la versión **están expuestos**  

### ¿Son identificables las cookies específicas del framework o las estructuras de directorios?
- [ ] No — los artefactos comunes del framework (por ejemplo, rutas `.js`, nombres de cookies) están ofuscados o han sido renombrados  
- [ ] Sí — las cookies específicas del framework (por ejemplo, `JSESSIONID`, `PHPSESSID`, `csrftoken`) **son** identificables  
- [ ] Sí — las estructuras de directorios (por ejemplo, `/wp-content/`, `_next/static/`) **son** visibles y confirman el framework  

### ¿Revelan los mensajes de error o los comentarios del código fuente detalles del framework?
- [ ] No — el manejo de errores es genérico y los comentarios han sido eliminados  
- [ ] Sí — los stack traces específicos del framework **están habilitados** y son visibles en las respuestas  
- [ ] Sí — el código fuente HTML contiene comentarios o etiquetas de metadatos que identifican el framework  

### ¿Existen vulnerabilidades conocidas asociadas con la versión del framework identificada?
- [ ] No — el framework identificado está actualizado y **no tiene** vulnerabilidades públicas conocidas  
- [ ] Sí — la versión del framework está desactualizada y se **pueden** identificar CVEs específicos

---

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

## WSTG-CONF-02 — Test Application Platform Configuration

Las pruebas de configuración de la plataforma de la aplicación implican la auditoría del servidor web subyacente, el servidor de aplicaciones y los ajustes del framework para asegurar que estén robustecidos (hardened) contra exploits comunes. Los atacantes buscan activamente credenciales por defecto (default credentials), vulnerabilidades de la plataforma sin parches e interfaces administrativas expuestas para obtener acceso no autorizado o ejecutar código. Las configuraciones erróneas a menudo resultan en la divulgación de variables de entorno sensibles, rutas internas del sistema o la presencia de aplicaciones de ejemplo innecesarias que expanden la superficie de ataque (attack surface). Desde una perspectiva profesional, una plataforma mal configurada sirve como un punto de entrada de alta probabilidad para lograr el acceso inicial al entorno objetivo.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si las interfaces administrativas son accesibles con credenciales por defecto o si los scripts de ejemplo permiten la ejecución remota de código (Remote Code Execution).

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### ¿Están activas las credenciales o cuentas por defecto en la plataforma?
- [ ] No — todas las cuentas por defecto están **deshabilitadas** o se han cambiado las contraseñas  
- [ ] Sí — existen cuentas por defecto pero **no son accesibles** desde la red externa  
- [ ] Sí — las cuentas por defecto están **activas** y son accesibles a través de internet *(Crítico)*  

### ¿Está la plataforma divulgando información sensible de la versión a través de cabeceras o páginas de error?
- [ ] No — las cadenas de versión están **ocultas** o son genéricas  
- [ ] Sí — la información detallada de la versión **se divulga** en las cabeceras HTTP (p. ej., `Server`, `X-Powered-By`)  
- [ ] Sí — se exponen trazas de la pila (stack traces) completas y rutas internas del sistema en mensajes de error detallados  

### ¿Están las interfaces administrativas o de gestión debidamente restringidas?
- [ ] No — las interfaces administrativas **no están expuestas**  
- [ ] Sí — existen interfaces pero están **restringidas** mediante listas de permitidos (allowlisting) de IP o autenticación de múltiples factores (Multi-Factor Authentication)  
- [ ] Sí — las interfaces (p. ej., `/admin`, `/manager`, `/console`) son **públicamente accesibles** con autenticación de un solo factor  

### ¿Existen archivos de ejemplo, scripts o documentación en el servidor de producción?
- [ ] No — todos los archivos no esenciales han sido **eliminados**  
- [ ] Sí — los archivos de ejemplo o la documentación **están presentes** pero no filtran información sensible  
- [ ] Sí — los scripts de ejemplo (p. ej., `info.php`, `examples/`) **están presentes** y proporcionan un vector de ataque directo  

### ¿Está el servidor configurado para admitir métodos HTTP inseguros?
- [ ] No — solo `GET` y `POST` están **habilitados**  
- [ ] Sí — métodos potencialmente arriesgados como `PUT`, `DELETE` o `TRACE` están **habilitados** pero restringidos  
- [ ] Sí — los métodos arriesgados están **habilitados** y permiten la modificación no autorizada de archivos o el robo de credenciales

---

## WSTG-CONF-03 — Prueba del Manejo de Extensiones de Archivos para Información Sensible

Las pruebas de manejo de extensiones de archivos consisten en identificar si el servidor web o el servidor de aplicaciones revela información sensible al servir archivos que deberían estar restringidos o ser ejecutados. Los atacantes buscan frecuentemente archivos de respaldo (backup), archivos de configuración y fragmentos de código fuente (por ejemplo, `.bak`, `.old`, `.env`, `.inc`) que puedan haber quedado en la raíz web y se sirvan como texto plano debido a la falta de mapeos de controladores específicos. Esta vulnerabilidad es relevante porque puede conducir a la exposición de credenciales de bases de datos, API keys y lógica de negocio interna, facilitando una explotación más profunda de la infraestructura. Estas exposiciones suelen ocurrir en directorios públicos, carpetas de subida o a través de configuraciones incorrectas de MIME-type en el servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si los archivos de configuración que contienen credenciales o el código fuente completo son accesibles.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### ¿Son accesibles las extensiones de archivos temporales o de respaldo sensibles (por ejemplo, `.bak`, `.old`, `.swp`, `~`)?
- [ ] No — el servidor devuelve 403 Forbidden o 404 Not Found para las extensiones de respaldo comunes  
- [ ] Sí — el servidor permite la descarga de archivos de respaldo, pero estos **no** contienen datos sensibles  
- [ ] Sí — el servidor permite la descarga de archivos de respaldo que contienen datos sensibles o código fuente *(Alta)*  

### ¿Expone el servidor archivos de configuración del sistema o del entorno (por ejemplo, `.env`, `.config`, `.yml`, `.ini`)?
- [ ] No — los archivos de configuración restringidos **no** pueden ser accedidos y devuelven 403/404  
- [ ] Sí — los archivos de configuración sensibles **son** accesibles y revelan secretos internos o credenciales  

### ¿Se puede recuperar el código fuente añadiendo extensiones alternativas (por ejemplo, `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] No — el código de la aplicación **no** se renderiza como texto plano independientemente de la manipulación de la extensión  
- [ ] Sí — el código fuente es revelado porque el servidor **no** tiene un handler para la extensión añadida  

### ¿Están bloqueados para el acceso público los directorios administrativos o de metadatos (por ejemplo, `.git/`, `.svn/`, `.DS_Store`)?
- [ ] No — estos directorios/archivos **no** existen en la raíz web  
- [ ] Sí — el acceso **no es posible** debido a reglas de rewrite del lado del servidor o permisos  
- [ ] Sí — los directorios de metadatos **en efecto** son accesibles y permiten la reconstrucción completa del código fuente  

### ¿Cómo maneja el servidor las solicitudes de archivos con múltiples extensiones (por ejemplo, `file.php.jpg`)?
- [ ] No — el servidor prioriza correctamente la seguridad de la extensión interna o bloquea la solicitud  
- [ ] Sí — el servidor procesa el archivo como la primera extensión, pero el bypass **no es posible** para la ejecución  
- [ ] Sí — el servidor ignora la extensión final, permitiendo que la ejecución de código o la divulgación de código fuente **sea posible**

---

## WSTG-CONF-04 — Revisión de archivos de respaldo antiguos y no referenciados en busca de información sensible

La revisión de archivos de respaldo antiguos y archivos no referenciados consiste en identificar archivos olvidados, temporales o ocultos en un servidor web que no están destinados al acceso público. Estos archivos suelen incluir copias de seguridad del código fuente (`.zip`, `.bak`), archivos de intercambio de editores de texto (`.swp`, `~`) o metadatos de control de versiones (`.git`, `.svn`) que pueden filtrar información sensible. Los atacantes utilizan wordlists automatizadas y herramientas de descubrimiento para realizar Brute Force sobre convenciones de nomenclatura comunes, con el objetivo de exfiltrar credenciales de bases de datos, API keys hardcoded o lógica que facilite un Exploit posterior. Esta vulnerabilidad suele originarse por un mantenimiento manual del servidor o por pipelines de CI/CD defectuosos que no logran sanear el web root de producción.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Media / Alta* |

> *La severidad se vuelve Alta si se descubren archivos de configuración, archivos de código fuente o credenciales.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### ¿Están habilitados los listados de directorios en el servidor web?
- [ ] No — el listado de directorios está **deshabilitado** y devuelve un 403 Forbidden o un error personalizado  
- [ ] Sí — el listado de directorios está **habilitado** en directorios no sensibles  
- [ ] Sí — el listado de directorios está **habilitado** en directorios que contienen código fuente o archivos sensibles *(Crítico)*  

### ¿Son descubribles los archivos de respaldo con extensiones comunes (ej., .bak, .old, .save)?
- [ ] No — no se **encuentran** extensiones de respaldo comunes o están bloqueadas por la configuración del servidor  
- [ ] Sí — existen archivos de respaldo pero **no** contienen información sensible  
- [ ] Sí — los archivos de respaldo de configuración o código fuente **son** accesibles *(Alta)*  

### ¿Están expuestos los directorios de control de versiones (ej., .git, .svn) o sus metadatos?
- [ ] No — los metadatos de control de versiones **no están presentes** o están correctamente bloqueados  
- [ ] Sí — existen metadatos pero el repositorio completo **no puede** ser reconstruido  
- [ ] Sí — el repositorio de código fuente completo **puede** ser exfiltrado a través de los metadatos expuestos  

### ¿Existen archivos comprimidos (ej., .zip, .tar.gz, .rar) de la aplicación en el web root?
- [ ] No — no se descubrieron archivos comprimidos sensibles mediante Brute Force o enumeración  
- [ ] Sí — se encontraron archivos comprimidos pero están protegidos por contraseña o contienen activos públicos  
- [ ] Sí — los archivos comprimidos sin cifrar que contienen el código fuente o datos de la aplicación **son** accesibles públicamente  

### ¿La aplicación filtra archivos temporales creados por editores de texto o IDEs?
- [ ] No — los archivos temporales como `.swp`, `~` o `.DS_Store` **no están presentes**  
- [ ] Sí — archivos temporales no sensibles están **presentes**  
- [ ] Sí — los archivos temporales revelan fragmentos de código fuente o rutas internas

---

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

## WSTG-CONF-06 — Probar métodos HTTP

La prueba de métodos HTTP consiste en identificar qué verbos son compatibles con el servidor web y la aplicación para asegurar que solo se exponga la funcionalidad necesaria. Más allá de los estándares `GET` y `POST`, los servidores pueden habilitar inadvertidamente métodos peligrosos como `PUT`, `DELETE` o `TRACE`, los cuales pueden permitir a los atacantes cargar archivos maliciosos, eliminar contenido existente o exfiltrar cookies de sesión mediante Cross-Site Tracing (XST). Los pentesters examinan los encabezados de respuesta como `Allow` o `Public` e intentan evadir las restricciones utilizando técnicas de method overriding o verb tampering para acceder a funcionalidades no autorizadas. Esta prueba es fundamental para fortalecer la superficie de ataque y prevenir configuraciones erróneas que podrían conducir al compromiso del servidor o a la manipulación de datos no autorizada.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Baja / Media* |

> *La severidad se considera Alta si `PUT` o `DELETE` permiten la manipulación de archivos no autorizada o si `TRACE` facilita la exfiltración de tokens de sesión.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### ¿Están deshabilitados los métodos HTTP peligrosos como `PUT` o `DELETE` en toda la aplicación?
- [ ] Sí — solo los métodos seguros (GET, POST, HEAD) **están habilitados**  
- [ ] No — `PUT` o `DELETE` **están habilitados** pero requieren una autenticación válida  
- [ ] No — `PUT` o `DELETE` **están habilitados** y son accesibles sin autenticación *(Crítico)*  

### ¿Está deshabilitado el método `TRACE` para prevenir Cross-Site Tracing (XST)?
- [ ] Sí — el método `TRACE` **está deshabilitado** o devuelve un 405 Method Not Allowed  
- [ ] No — el método `TRACE` **está habilitado** pero no refleja encabezados sensibles  
- [ ] No — el método `TRACE` **está habilitado** y refleja encabezados `Cookie` o `Authorization` *(Media)*  

### ¿El servidor admite el HTTP Method Overriding (p. ej., `X-HTTP-Method-Override`)?
- [ ] No — el servidor **no** procesa encabezados de invalidación de método (method override)  
- [ ] Sí — el servidor procesa encabezados de invalidación pero **se aplican** los controles de acceso  
- [ ] Sí — el servidor procesa encabezados de invalidación y **es posible** evadir el control de acceso  

### ¿Responde el servidor de forma segura ante métodos HTTP arbitrarios o malformados?
- [ ] Sí — el servidor devuelve un 405 Method Not Allowed o un 501 Not Implemented  
- [ ] No — el servidor devuelve un 200 OK o un Error 500 para métodos arbitrarios como `JEFF` o `TEST`  
- [ ] No — el uso de métodos arbitrarios permite evadir filtros de autenticación o autorización  

### ¿Está el método `OPTIONS` restringido o configurado para minimizar la divulgación de información?
- [ ] Sí — `OPTIONS` está deshabilitado o devuelve un encabezado `Allow` mínimo  
- [ ] No — `OPTIONS` revela una amplia gama de métodos habilitados, incluyendo DAV o verbos de depuración (debugging)

---

## WSTG-CONF-07 — Pruebas de HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) es un mecanismo de seguridad que permite a un servidor web informar a los navegadores que solo se debe acceder a él a través de HTTPS, previniendo ataques de degradación de protocolo (protocol downgrade) y el secuestro de cookies (cookie hijacking). Al forzar una conexión cifrada, HSTS mitiga ataques Man-in-the-Middle (MitM) como SSLStrip, donde un atacante intercepta una solicitud HTTP inicial no segura para evitar la transición a TLS. Desde la perspectiva de un atacante, la ausencia de este encabezado o una duración corta de `max-age` proporciona una ventana de oportunidad para interceptar tokens de sesión sensibles o credenciales durante el Handshake inicial en texto claro. Esta prueba se centra en verificar la presencia, duración y alcance del encabezado `Strict-Transport-Security` en todos los endpoints sensibles de la aplicación.

| Campo | Valor |
|---|---|
| **ID WSTG** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Baja / Media* |

> *La severidad aumenta a Media si la aplicación maneja PII, tokens de sesión o credenciales, ya que la falta de HSTS incrementa la tasa de éxito de los ataques MitM.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### ¿Está presente el encabezado `Strict-Transport-Security` en las respuestas HTTPS?
- [ ] Sí — el encabezado **está presente** en todos los endpoints sensibles  
- [ ] Sí — el encabezado **está presente** pero solo en la página de inicio (landing page)  
- [ ] No — el encabezado **no se encuentra** en la aplicación en absoluto  

### ¿Está la directiva `max-age` configurada con una duración suficiente?
- [ ] Sí — `max-age` está configurado para un año o más (ej. `31536000`)  
- [ ] Sí — `max-age` está configurado pero es **demasiado corto** (ej. menos de 6 meses)  
- [ ] No — la directiva `max-age` **no está presente** o está configurada en `0` *(Crítico)*  

### ¿Cubre la política HSTS todos los subdominios?
- [ ] Sí — la directiva `includeSubDomains` **está aplicada**  
- [ ] No — la directiva `includeSubDomains` **no está presente**, dejando a los subdominios vulnerables a la degradación de protocolo  
- [ ] No — la característica no aplica ya que no existen subdominios  

### ¿Está la aplicación registrada para HSTS Preloading?
- [ ] Sí — la directiva `preload` **está presente** y el dominio está en la lista de HSTS preload  
- [ ] Sí — la directiva `preload` **está presente** pero el dominio **aún no está** en la lista de preload  
- [ ] No — la directiva `preload` **no está presente**, permitiendo una ventana para MitM en la primera visita  

### ¿Se envía el encabezado HSTS incorrectamente a través de HTTP en texto claro?
- [ ] No — el encabezado **solo** se envía a través de conexiones seguras HTTPS  
- [ ] Sí — el encabezado **se envía** a través de HTTP, lo cual es ignorado por los navegadores y puede filtrar detalles de configuración

---

## WSTG-CONF-08 — Prueba de Política de Dominio Cruzado en RIA

Las políticas de dominio cruzado de las Aplicaciones de Internet Enriquecidas (RIA) involucran archivos como `crossdomain.xml` y `clientaccesspolicy.xml`, los cuales definen cómo los clientes web —específicamente Adobe Flash y Microsoft Silverlight— gestionan las peticiones de origen cruzado (cross-origin). Estos archivos son críticos para la seguridad porque anulan explícitamente la Política de Mismo Origen (SOP), determinando qué dominios externos tienen permiso para interactuar con la aplicación y leer sus respuestas. Las configuraciones excesivamente permisivas, como el uso de comodines (wildcards) en el atributo `domain`, permiten que los atacantes alojen objetos RIA maliciosos en un sitio de terceros que pueden exfiltrar datos sensibles o realizar acciones en nombre de usuarios autenticados. Los pentesters suelen localizar estos archivos en la raíz web para evaluar el nivel de confianza otorgado a orígenes de terceros e identificar posibles fugas de datos de origen cruzado.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si la aplicación maneja datos sensibles de usuario o identificadores de sesión que pueden ser exfiltrados a través de una política permisiva.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### ¿Están presentes los archivos de política de dominio cruzado (`crossdomain.xml` o `clientaccesspolicy.xml`) en la raíz web?
- [ ] No — los archivos **no** se pueden encontrar en el directorio raíz  
- [ ] Sí — `crossdomain.xml` (Flash) **está** presente  
- [ ] Sí — `clientaccesspolicy.xml` (Silverlight) **está** presente  
- [ ] Sí — ambos archivos de política RIA **están** presentes  

### ¿Está el atributo de dominio `allow-access-from` restringido a orígenes de confianza?
- [ ] No — los archivos existen pero no contienen entradas `allow-access-from`  
- [ ] Sí — dominios específicos y de confianza están explícitamente en la lista blanca (whitelist) y **no es posible** realizar un bypass  
- [ ] Sí — los dominios están en la lista blanca pero incluyen subdominios o terceros no confiables  
- [ ] Sí — se utiliza un comodín `*` (**wildcard**), permitiendo el acceso desde **cualquier** origen *(Crítico)*  

### ¿Permite la política comunicaciones inseguras a través de HTTP para recursos HTTPS?
- [ ] No — `secure="true"` está configurado (o por defecto), evitando que orígenes HTTP accedan a datos HTTPS  
- [ ] Sí — `secure="false"` está configurado explícitamente, permitiendo que atacantes MITM exfiltren datos seguros  

### ¿Son los encabezados HTTP excesivamente permisivos en la configuración de dominio cruzado?
- [ ] No — `allow-http-request-headers-from` está restringido o **no está habilitado**  
- [ ] Sí — se permiten encabezados específicos para dominios de confianza  
- [ ] Sí — se utiliza el comodín `*` para los encabezados, permitiendo a los atacantes enviar encabezados personalizados desde cualquier dominio  

### ¿Es restrictiva la configuración de `site-control` (master-policy)?
- [ ] No — `permitted-cross-domain-policies` está configurado como `none` o `master-only`  
- [ ] Sí — la política permite que otros archivos en el servidor definan sus propias reglas (`all`)

---

## WSTG-CONF-09 — Prueba de Permisos de Archivos

Las pruebas de permisos de archivos consisten en auditar los niveles de control de acceso asignados a los archivos y directorios en el servidor web para garantizar que los recursos sensibles no estén sobreexpuestos a usuarios o procesos no autorizados. Los permisos configurados incorrectamente pueden permitir que los atacantes lean archivos de configuración sensibles que contienen credenciales de bases de datos, visualicen código fuente propietario o modifiquen scripts del lado del servidor para lograr una ejecución remota de comandos (Remote Code Execution). Esta vulnerabilidad ocurre típicamente durante la fase de despliegue cuando se dejan habilitados los indicadores de "lectura global" (world-readable) o "escritura global" (world-writable) en directorios críticos de la aplicación, como rutas de configuración, carpetas de registros o repositorios de carga de archivos. Desde la perspectiva de un atacante, el descubrimiento de un archivo asegurado incorrectamente suele servir como el catalizador principal para el escalamiento de privilegios (Privilege Escalation) o el compromiso total del sistema.

| Campo | Valor |
|---|---|
| **ID WSTG** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta* |

> *La severidad se vuelve Alta si los archivos de configuración sensibles (p. ej., `.env`, `web.config`) son legibles o si el acceso de escritura a la raíz web (web root) es posible.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Herramientas:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### ¿Están protegidos los archivos de configuración sensibles de la aplicación contra el acceso no autorizado?
- [ ] Sí — los archivos de configuración (p. ej., `.env`, `config.php`) **no son accesibles** mediante peticiones web  
- [ ] Sí — los archivos están presentes pero el acceso está **restringido** solo a usuarios locales autorizados  
- [ ] No — los archivos de configuración sensibles **son accesibles** y exponen credenciales o secretos  

### ¿Puede un atacante modificar archivos o directorios dentro de la raíz web?
- [ ] No — todos los directorios de la raíz web son de **solo lectura** para el usuario del servidor web  
- [ ] Sí — existen directorios con permisos de escritura pero la ejecución de scripts está **deshabilitada**  
- [ ] Sí — existen directorios con permisos de escritura global (world-writable) que **permiten** la carga y ejecución de scripts *(Crítico)*  

### ¿Están expuestos metadatos de control de versiones o archivos de respaldo debido a permisos laxos?
- [ ] No — `.git`, `.svn` y archivos de respaldo (p. ej., `.bak`, `~`) **no están presentes** o están protegidos  
- [ ] Sí — existen directorios de metadatos pero el listado de directorios (directory listing) está **deshabilitado**  
- [ ] Sí — los metadatos sensibles o respaldos son **completamente accesibles**, lo que permite la recuperación del código fuente  

### ¿Opera el usuario del servidor web bajo el principio de menor privilegio?
- [ ] Sí — el proceso del servidor web se ejecuta como un **usuario dedicado de bajos privilegios**  
- [ ] No — el proceso del servidor web se ejecuta con **privilegios innecesarios** (p. ej., `root` o `Administrator`)  

### ¿Están los archivos de registro (log files) protegidos contra lectura o modificación no autorizada?
- [ ] Sí — los archivos de registro están **restringidos** a usuarios administrativos  
- [ ] Sí — los archivos de registro son legibles pero **no** contienen tokens de sesión o PII (información de identificación personal)  
- [ ] No — los archivos de registro son **públicamente legibles** y exponen datos sensibles de transacciones o información de usuarios

---

## WSTG-CONF-10 — Subdomain Takeover

El Subdomain Takeover (secuestro de subdominio) ocurre cuando una entrada DNS (típicamente un registro CNAME, pero ocasionalmente registros A o MX) apunta a un recurso retirado o inexistente en un proveedor de la nube de terceros o un servicio de alojamiento. Los atacantes identifican estos registros DNS "dangling" (huérfanos) buscando mensajes de error específicos de proveedores como AWS, GitHub o Azure que indican que un recurso ya no está reclamado. Al registrar el nombre del recurso abandonado en la plataforma del proveedor, el atacante obtiene el control total sobre el subdominio, lo que le permite alojar contenido malicioso, exfiltrar cookies de sesión con ámbito (scope) en el dominio base y omitir las protecciones de Content Security Policy (CSP) o CORS. Esta vulnerabilidad es particularmente peligrosa porque aprovecha la confianza asociada con la estructura de dominio legítima de la organización para facilitar ataques de phishing y robo de datos de alto impacto.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Alta / Crítica* |

> *La severidad se vuelve Crítica si el subdominio puede utilizarse para secuestrar cookies de sesión del dominio base o eludir redireccionamientos de SSO/OAuth.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Herramientas:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### ¿Utiliza la aplicación servicios de alojamiento de terceros o servicios en la nube a través de subdominios?
- [ ] No — todos los subdominios apuntan a un espacio de direcciones IP controlado por la organización  
- [ ] Sí — los subdominios apuntan a proveedores externos (S3, GitHub Pages, Heroku, etc.)  

### ¿Existen registros DNS "dangling" que apunten a recursos inexistentes?
- [ ] No — todos los registros DNS identificados resuelven a recursos activos y controlados  
- [ ] Sí — existen registros CNAME o A para recursos que **ya no existen** en el proveedor  
- [ ] Sí — los registros DNS resuelven a NXDOMAIN o a páginas de error específicas del proveedor *(ej., "NoSuchBucket")*  

### ¿Se puede reclamar con éxito el recurso "dangling" identificado?
- [ ] No — el proveedor tiene protecciones contra la reclamación de nombres abandonados o se requiere verificación de cuenta  
- [ ] Sí — el nombre del recurso **puede** ser registrado en la plataforma del tercero por cualquier usuario  

### ¿Utiliza el dominio base cookies con ámbito de "Domain" que podrían verse comprometidas?
- [ ] No — las cookies están estrictamente limitadas a subdominios específicos o utilizan flags `Host-Only`  
- [ ] Sí — las cookies sensibles (IDs de sesión, JWTs) tienen como ámbito el dominio principal y **pueden** ser exfiltradas a través de un subdominio secuestrado  

### ¿Puede utilizarse el subdominio para omitir cabeceras de seguridad o flujos de autenticación?
- [ ] No — no hay dependencias de seguridad en los subdominios identificados  
- [ ] Sí — el subdominio está en la lista blanca (whitelist) en las directivas `script-src` o `connect-src` de CSP  
- [ ] Sí — el subdominio es una URI de redireccionamiento válida para configuraciones de OAuth o SSO

---

## WSTG-CONF-11 — Probar el Almacenamiento en la Nube

Las pruebas de desconfiguraciones en el almacenamiento en la nube implican identificar y auditar servicios de almacenamiento de objetos accesibles públicamente, tales como Amazon S3, Azure Blobs o Google Cloud Storage. Estos recursos a menudo contienen datos sensibles, incluyendo respaldos, código fuente, PII (información de identificación personal) o archivos de configuración que quedan expuestos debido a Listas de Control de Acceso (ACL) o políticas de Gestión de Identidad y Acceso (IAM) excesivamente permisivas. Los atacantes enumeran nombres de buckets a través del reconocimiento de archivos JavaScript, código fuente HTML y técnicas de Brute Force para encontrar puntos de entrada para la exfiltración de datos o la modificación no autorizada de archivos. La explotación puede tener un impacto significativo, incluyendo brechas de datos completas o la capacidad de servir archivos maliciosos desde un dominio de confianza.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica* |

> *La severidad se vuelve Crítica si la PII, las credenciales o los respaldos de producción son accesibles, o si se permite el acceso de escritura sin autenticación.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### ¿Se han identificado y enumerado los endpoints de almacenamiento en la nube (buckets/containers)?
- [ ] No — no se utilizan o no son descubribles endpoints de almacenamiento en la nube  
- [ ] Sí — los endpoints de almacenamiento se identifican mediante análisis de código o monitoreo de tráfico  
- [ ] Sí — los endpoints de almacenamiento se descubren mediante Brute Force de convenciones de nombres  

### ¿Pueden los usuarios no autenticados listar el contenido del almacenamiento en la nube?
- [ ] No — el listado del bucket está **deshabilitado** y devuelve 403 Forbidden o 404 Not Found  
- [ ] Sí — el listado está **habilitado** pero no hay archivos sensibles presentes  
- [ ] Sí — el listado está **habilitado** y los nombres de archivos/metadatos sensibles son visibles  

### ¿Es posible la descarga de archivos sensibles desde los buckets identificados?
- [ ] No — los archivos están protegidos por políticas IAM o se requiere una autenticación válida  
- [ ] Sí — los archivos son accesibles pero requieren una URL firmada (Signed URL) que **no** puede adivinarse fácilmente  
- [ ] Sí — los archivos sensibles (backups, `.env`, PII) son **públicamente accesibles** para su descarga *(Crítica)*  

### ¿Se permite la carga o modificación de archivos sin autenticación?
- [ ] No — las cargas o modificaciones no autenticadas **no son posibles**  
- [ ] Sí — se requiere carga autenticada pero es posible realizar un **bypass** mediante condiciones de política débiles  
- [ ] Sí — la escritura, sobrescritura o eliminación de archivos sin autenticación es **posible** *(Crítica)*  

### ¿Son restrictivas las políticas de Cross-Origin Resource Sharing (CORS) en el endpoint de almacenamiento?
- [ ] Sí — CORS está **deshabilitado** o restringido a orígenes específicos de confianza  
- [ ] No — CORS está **habilitado** con un comodín `*` (wildcard), lo que permite el acceso no autorizado basado en web

---

## WSTG-CONF-12 — Content Security Policy (CSP)

Content Security Policy (CSP) es un mecanismo crítico de defensa en profundidad (defense-in-depth) implementado a través de encabezados de respuesta HTTP para mitigar riesgos como Cross-Site Scripting (XSS), clickjacking y ataques de inyección de datos. Al definir qué recursos dinámicos están autorizados para cargarse, una CSP bien configurada restringe la capacidad de un atacante para ejecutar scripts no autorizados o exfiltrar datos sensibles a dominios externos. Los pentesters evalúan la política en busca de directivas excesivamente permisivas, el uso de palabras clave no seguras como `'unsafe-inline'` y la dependencia de CDNs inseguras que podrían facilitar bypasses. Una CSP débil o ausente no crea una vulnerabilidad directamente, pero aumenta significativamente el impacto de ataques de inyección exitosos al no proporcionar una capa secundaria de protección.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Herramientas:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### ¿Está presente el encabezado Content Security Policy (CSP) en las respuestas de la aplicación?
- [ ] Sí — el encabezado `Content-Security-Policy` está **presente** y se **aplica (enforced)**  
- [ ] Sí — `Content-Security-Policy-Report-Only` está **presente** para pruebas  
- [ ] No — no hay ningún encabezado CSP **presente** en las respuestas  

### ¿Están las directivas `script-src` o `default-src` restringidas adecuadamente?
- [ ] Sí — las directivas utilizan listas blancas (whitelisting) estrictas, nonces o hashes y el bypass **no es posible**  
- [ ] Sí — las directivas están **configuradas correctamente**, pero la lista blanca incluye CDNs que permiten bypasses conocidos  
- [ ] No — las directivas utilizan comodines (`*`) o esquemas `data:`, permitiendo que el bypass sea **posible**  

### ¿Permite la política la ejecución de scripts alineados (inline) no seguros o eval?
- [ ] No — `'unsafe-inline'` y `'unsafe-eval'` **no están presentes**  
- [ ] Sí — `'unsafe-inline'` está **presente** pero protegido por un `nonce` o `hash`  
- [ ] Sí — `'unsafe-inline'` o `'unsafe-eval'` están **habilitados** sin protecciones adicionales *(Crítico)*  

### ¿Está la aplicación protegida contra clickjacking mediante la directiva `frame-ancestors`?
- [ ] Sí — `frame-ancestors` está configurado como `'none'` o `'self'`  
- [ ] Sí — `frame-ancestors` está **habilitado** pero permite dominios específicos de terceros de confianza  
- [ ] No — `frame-ancestors` **no está presente**, dependiendo del encabezado legado `X-Frame-Options` o careciendo de protección  

### ¿Existe un mecanismo para reportar violaciones de CSP?
- [ ] Sí — `report-uri` o `report-to` está **configurado** y activo  
- [ ] No — el reporte de violaciones está **deshabilitado** o **no configurado**

---

## WSTG-CONF-13 — Path Confusion

Path Confusion surge de las discrepancias en cómo los diferentes componentes web, como proxies inversos, balanceadores de carga y servidores de aplicaciones backend, analizan e interpretan las rutas URL. Los atacantes explotan estas inconsistencias inyectando caracteres específicos como puntos y coma, barras codificadas o segmentos de puntos para engañar a los filtros de seguridad y acceder a endpoints restringidos o recursos estáticos. Esta vulnerabilidad puede derivar en fallos de seguridad críticos, incluyendo la omisión de autenticación (Authentication Bypass), el acceso no autorizado a datos y el envenenamiento de caché web (Web Cache Poisoning), ya que el front-end puede aplicar reglas de seguridad a una ruta que el back-end interpreta de manera diferente. La explotación exitosa suele ocurrir en el límite arquitectónico donde la lógica de enrutamiento y normalización de solicitudes diverge a lo largo del stack tecnológico.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si la confusión de rutas resulta en una omisión de autenticación o Broken Access Control para endpoints administrativos sensibles.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### ¿Interpretan las diferentes capas arquitectónicas los delimitadores de ruta (ej., `;`, `#`, `?`) de manera consistente?
- [ ] Sí — todas las capas normalizan e interpretan los delimitadores de ruta de manera idéntica  
- [ ] No — existen inconsistencias pero **no** pueden utilizarse para acceder a recursos restringidos  
- [ ] No — las discrepancias permiten que un atacante omita los filtros del front-end y alcance la lógica del back-end  

### ¿Se pueden omitir los controles de acceso utilizando secuencias de Path Traversal o caracteres codificados?
- [ ] No — la normalización se aplica de manera consistente antes de las comprobaciones de seguridad  
- [ ] Sí — la omisión es **posible** mediante secuencias de segmentos de puntos (ej., `/admin/..;/`)  
- [ ] Sí — la omisión es **posible** mediante caracteres codificados en URL (ej., `%2f`, `%2e%2e%2f`)  

### ¿Es la aplicación susceptible a Web Cache Poisoning a través de Path Confusion?
- [ ] No — la lógica de almacenamiento en caché no se ve afectada por la ambigüedad de rutas o información de ruta adicional  
- [ ] Sí — el contenido malicioso **puede** almacenarse en caché para rutas legítimas debido a discrepancias en el mapeo  
- [ ] Sí — la información sensible **puede** almacenarse en caché en directorios públicos a través de técnicas de `RCD` (Relative Path Overwrite)  

### ¿Maneja el servidor la "información de ruta adicional" (PathInfo) de forma segura?
- [ ] No — la función está **deshabilitada** o no existe  
- [ ] Sí — `PathInfo` se maneja y **no** interfiere con los filtros de seguridad  
- [ ] No — `PathInfo` permite a un atacante añadir datos que confunden la lógica de enrutamiento de la aplicación

---

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

## WSTG-IDNT-01 — Test Role Definitions

La prueba de definiciones de roles consiste en identificar y documentar los diversos roles de usuario y niveles de permiso definidos dentro de la aplicación para asegurar que estén separados lógicamente y cumplan con el principio de **least privilege** (mínimo privilegio). Un atacante analiza la aplicación para mapear los roles de alto privilegio frente a los roles de usuario estándar, buscando cualquier ambigüedad en los límites de los permisos o funciones administrativas no documentadas. La identificación de estos roles es el primer paso para descubrir rutas de **Privilege Escalation** (escalada de privilegios), ya que las inconsistencias en las definiciones de roles a menudo conducen al acceso no autorizado a datos sensibles o funciones administrativas. Esta fase ocurre típicamente durante el reconocimiento inicial y el mapeo de la lógica de negocio, centrándose en los perfiles de usuario, dashboards administrativos y estructuras de permisos de **API**.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### ¿Están los roles claramente definidos y documentados en la aplicación o en su documentación?
- [ ] Sí — los roles están completamente documentados y siguen el principio de **least privilege**  
- [ ] Sí — los roles están documentados pero contienen **permisos superpuestos**  
- [ ] No — no existe documentación formal ni definiciones de roles  

### ¿Existe una separación clara entre las funciones administrativas y las de usuario estándar?
- [ ] Sí — las funciones administrativas están **completamente aisladas** de las vistas de usuario estándar  
- [ ] Sí — existe separación, pero los **endpoints** administrativos son **predecibles o adivinables**  
- [ ] No — las funciones administrativas y estándar están **mezcladas** dentro de las mismas interfaces  

### ¿Utiliza la aplicación un mecanismo centralizado para el Control de Acceso Basado en Roles (RBAC)?
- [ ] Sí — se ha **implementado** un **RBAC** o **ABAC** centralizado y se aplica de manera consistente  
- [ ] Sí — existen controles descentralizados pero se aplican de forma **inconsistente** en los módulos  
- [ ] No — la lógica de control de acceso está **hardcoded** en páginas o componentes individuales  

### ¿Existen roles no documentados u ocultos presentes en el sistema?
- [ ] No — todos los roles descubiertos coinciden con la funcionalidad documentada  
- [ ] Sí — **existen** roles ocultos o "en la sombra" (por ejemplo, `super_admin`, `support`, `test`)  

### ¿Puede un usuario con bajos privilegios enumerar los permisos o roles de otros usuarios?
- [ ] No — los usuarios **no pueden** ver ni enumerar los roles/permisos de otras entidades  
- [ ] Sí — los usuarios **pueden** enumerar roles a través de páginas de perfil, metadatos o respuestas de la **API** *(Media)*

---

## WSTG-IDNT-02 — Prueba del Proceso de Registro de Usuarios

La prueba del proceso de registro de usuarios implica evaluar el flujo de trabajo mediante el cual se crean nuevas identidades para garantizar que no pueda ser abusado para el acceso no autorizado o la explotación automatizada. Las vulnerabilidades en este proceso a menudo se manifiestan como la falta de verificación de identidad (email/SMS), susceptibilidad a la creación masiva de cuentas mediante bots, o divulgación de información a través de mensajes de error detallados que revelan nombres de usuario existentes. Los atacantes explotan estos fallos para realizar enumeración de usuarios (User Enumeration), llevar a cabo campañas de spam a gran escala, o manipular los parámetros de registro para obtener privilegios elevados durante la fase de creación de la cuenta. Esta prueba es vital para proteger la integridad de la aplicación y evitar que los atacantes pueblen el sistema con cuentas fraudulentas o maliciosas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### ¿Se requiere verificación de identidad antes de que una cuenta nueva se active?
- [ ] Sí — el registro requiere un token único y limitado en el tiempo enviado por correo electrónico o SMS  
- [ ] Sí — se requiere verificación, pero los tokens son **débiles** o **predecibles**  
- [ ] No — las cuentas se activan **inmediatamente** sin ningún paso de verificación  

### ¿El proceso de registro evita la enumeración de usuarios (User Enumeration)?
- [ ] Sí — la aplicación devuelve respuestas **idénticas** tanto para correos/nombres de usuario nuevos como existentes  
- [ ] No — los mensajes de error (p. ej., "Email ya está en uso") **permiten** a un atacante mapear usuarios registrados  
- [ ] No — las diferencias de tiempo en las respuestas del servidor **revelan** si un usuario existe  

### ¿Se han implementado controles contra la automatización para evitar el registro masivo?
- [ ] Sí — los mecanismos de CAPTCHA o prueba de trabajo (Proof-of-Work) están **presentes** y son **efectivos**  
- [ ] Sí — los controles existen, pero el bypass **es posible** a través de endpoints de API o manipulación de encabezados (Headers)  
- [ ] No — no se aplica Rate Limiting o CAPTCHA al endpoint de registro  

### ¿Pueden manipularse los parámetros de registro para escalar privilegios?
- [ ] No — los roles de usuario se asignan **estrictamente** mediante la lógica del lado del servidor  
- [ ] Sí — parámetros como `role`, `admin`, o `group_id` **pueden** ser modificados en la solicitud para obtener un acceso superior  

### ¿Se aplica la política de contraseñas durante la fase de registro?
- [ ] Sí — se **imponen** requisitos de contraseñas robustas en el lado del servidor  
- [ ] No — el sistema **acepta** contraseñas débiles (p. ej., "password123")

---

## WSTG-IDNT-03 — Test Account Provisioning Process

El proceso de aprovisionamiento de cuentas rige cómo se crean, verifican y asignan privilegios a las nuevas identidades dentro del entorno de una aplicación. Los atacantes se dirigen a este flujo de trabajo para realizar registros masivos automatizados para spam o denegación de servicio (DoS), evadir los pasos de verificación de identidad para crear cuentas fraudulentas, o manipular parámetros para otorgarse permisos elevados durante la fase de registro (signup). Las vulnerabilidades suelen manifestarse en formularios de autoregistro, sistemas basados solo en invitaciones o consolas de aprovisionamiento administrativo donde los fallos de lógica de negocio permiten la creación de cuentas no autorizadas o la escalada de privilegios (Privilege Escalation). Desde la perspectiva de un atacante, comprometer el flujo de aprovisionamiento proporciona un punto de apoyo legítimo que puede utilizarse como base para una explotación más profunda, la filtración de datos (Data Exfiltration) o ataques de ingeniería social.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta* |

> *La severidad se vuelve Crítica si un atacante puede aprovisionar cuentas administrativas o evadir las restricciones globales de registro.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### ¿El autoregistro está estrictamente controlado o limitado a usuarios autorizados?
- [ ] Sí — el registro está **deshabilitado** o requiere aprobación administrativa manual  
- [ ] Sí — el registro está abierto pero restringido a dominios de correo electrónico **autorizados** o tokens de invitación  
- [ ] No — el registro está **abierto** para cualquier usuario sin restricciones de dominio o tokens  

### ¿Se requiere un mecanismo robusto de verificación de identidad (p. ej., email/SMS) antes de la activación de la cuenta?
- [ ] Sí — la verificación es **obligatoria** y **no puede** ser evadida  
- [ ] Sí — la verificación es **obligatoria** pero **puede** ser evadida mediante manipulación de parámetros (Parameter Tampering) o tokens predecibles  
- [ ] No — las cuentas se activan **inmediatamente** tras el envío sin ninguna verificación  

### ¿Puede el usuario manipular los parámetros de rol o permisos durante la solicitud de registro?
- [ ] No — los roles se **asignan en el lado del servidor** (Server-side) y no existen parámetros en el lado del cliente  
- [ ] Sí — existen parámetros de rol pero su manipulación **no es posible** debido a la validación en el lado del servidor  
- [ ] Sí — los parámetros de rol/permisos (p. ej., `role=admin`, `is_staff=true`) **pueden** ser modificados para obtener acceso elevado *(Crítica)*  

### ¿Se han implementado controles de límite de tasa (Rate Limiting) o medidas anti-automatización para prevenir la creación masiva de cuentas?
- [ ] Sí — los límites de tasa y CAPTCHAs están **activos** y son efectivos  
- [ ] Sí — existen controles pero la evasión **es posible** mediante suplantación de cabeceras (Header Spoofing) o servicios de resolución de CAPTCHA  
- [ ] No — no se **aplican** límites de tasa ni controles anti-automatización  

### ¿El proceso de aprovisionamiento filtra información sobre cuentas existentes (Enumeración de Usuarios / User Enumeration)?
- [ ] No — los mensajes de respuesta son **idénticos** para usuarios existentes y no existentes  
- [ ] Sí — diferentes respuestas o variaciones de tiempo **permiten** la enumeración de usuarios existentes

---

## WSTG-IDNT-04 — Testing for Account Enumeration and Guessable User Account

La **Account Enumeration** ocurre cuando una aplicación revela la existencia o inexistencia de una cuenta de usuario a través de variaciones en las respuestas HTTP, mensajes de error o diferencias de **Timing** medibles. Los atacantes explotan este comportamiento enviando sistemáticamente listas de nombres de usuario para identificar cuentas válidas, las cuales son posteriormente blanco de **Credential Stuffing**, ataques de **Brute Force** o ingeniería social. Esta vulnerabilidad se manifiesta comúnmente en portales de inicio de sesión, funciones de restablecimiento de contraseña, formularios de registro y **API** endpoints que manejan identificadores específicos del usuario. Desde la perspectiva de un atacante, una enumeración exitosa reduce significativamente la superficie de ataque al transformar un intento de **Brute Force** a ciegas en una explotación dirigida de identidades conocidas y válidas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Bajo / Medio |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### ¿Son los mensajes de error de autenticación consistentes en todos los escenarios?
- [ ] Sí — se utilizan mensajes de error genéricos tanto para nombres de usuario inválidos como para contraseñas inválidas  
- [ ] Sí — los mensajes son similares, pero **están presentes** ligeras variaciones en la puntuación o en el uso de mayúsculas/minúsculas  
- [ ] No — los mensajes de error indican explícitamente "User not found" o "Incorrect password" *(Vulnerable)*  

### ¿El flujo de restablecimiento de contraseña o "olvidé mi contraseña" filtra la existencia de cuentas?
- [ ] No — la aplicación devuelve un mensaje genérico independientemente de si el correo/nombre de usuario existe  
- [ ] Sí — existen controles, pero el **Bypass** es posible mediante la longitud de la respuesta o los códigos de estado  
- [ ] Sí — la aplicación confirma explícitamente si se envió un correo de restablecimiento o si la cuenta **no existe**  

### ¿Está el proceso de registro protegido contra la Account Enumeration?
- [ ] No — el registro no filtra información o utiliza CAPTCHA/**Rate Limiting** para prevenir verificaciones masivas  
- [ ] Sí — existen controles, pero el **Bypass** es posible mediante diferencias de **Timing** o en la respuesta de la **API**  
- [ ] Sí — la aplicación notifica inmediatamente al usuario si el nombre de usuario o correo electrónico **ya está en uso**  

### ¿Son medibles las diferencias de Timing al comparar nombres de usuario válidos vs. inválidos?
- [ ] No — los tiempos de respuesta son consistentes independientemente de la validez de la cuenta debido a comparaciones de tiempo constante  
- [ ] Sí — existen diferencias de **Timing** pero son demasiado pequeñas para ser medidas de forma fiable a través de la red  
- [ ] Sí — **están presentes** discrepancias de **Timing** medibles (por ejemplo, debido a la ejecución del hashing de contraseña solo para usuarios válidos)  

### ¿Utiliza la aplicación convenciones de nombres predecibles o adivinables para las cuentas?
- [ ] No — los identificadores de cuenta son aleatorios (UUIDs) o definidos por el usuario y complejos  
- [ ] Sí — los identificadores de cuenta siguen un patrón (ej. `emp001`, `emp002`) y la enumeración **es posible**  
- [ ] Sí — los identificadores son enteros secuenciales, lo que hace **posible** la enumeración completa de la base de datos

---

## WSTG-IDNT-05 — Prueba de políticas de nombres de usuario débiles o no aplicadas

Las pruebas de políticas de nombres de usuario débiles o no aplicadas se centran en las reglas que rigen la creación de identificadores de cuenta y su susceptibilidad a la enumeración o suplantación (spoofing). Las políticas débiles a menudo permiten secuencias predecibles, palabras comunes de diccionarios o identificadores que reflejan los ID internos de los empleados, lo que reduce significativamente el espacio de búsqueda para ataques de Brute Force y credential stuffing. Desde la perspectiva de un atacante, identificar estos patrones es el primer paso en el descubrimiento de cuentas a gran escala, lo cual se logra analizando mensajes de error de registro, el comportamiento del restablecimiento de contraseñas o metadatos en perfiles públicos. Garantizar una política robusta evita que los atacantes mapeen sistemáticamente la base de usuarios y facilita la defensa contra ataques dirigidos de phishing e intentos automatizados de bypass de autenticación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### ¿Se basan los nombres de usuario en patrones altamente predecibles o secuenciales?
- [ ] No — los nombres de usuario son aleatorios, definidos por el usuario o cadenas de alta entropía.
- [ ] Sí — los nombres de usuario siguen un formato predecible (p. ej., `user1001`, `user1002`), pero la autenticación es robusta.
- [ ] Sí — los nombres de usuario son **estrictamente secuenciales** o siguen un formato corporativo conocido, lo que facilita la enumeración.

### ¿Aplica la aplicación requisitos mínimos de longitud y complejidad para los nombres de usuario?
- [ ] Sí — se **aplican** políticas estrictas de longitud y juego de caracteres durante el registro.
- [ ] Sí — existen políticas, pero permiten nombres de usuario extremadamente cortos (p. ej., 1-2 caracteres) o excesivamente simples.
- [ ] No — **no se aplica ninguna política** con respecto a la longitud del nombre de usuario o los tipos de caracteres.

### ¿Se pueden enumerar nombres de usuario válidos a través de las respuestas de la aplicación?
- [ ] No — la aplicación devuelve mensajes de error genéricos y muestra tiempos de respuesta consistentes para todos los intentos.
- [ ] Sí — la aplicación devuelve mensajes de error distintos (p. ej., "El nombre de usuario ya está en uso"), pero se aplica Rate Limiting.
- [ ] Sí — la aplicación **filtra** nombres de usuario válidos a través de errores de registro, restablecimientos de contraseña o diferencias de tiempo.

### ¿Impide la política el registro de nombres de usuario comunes o administrativos reservados?
- [ ] Sí — la aplicación bloquea nombres comunes (p. ej., `admin`, `root`, `support`) y términos reservados del sistema.
- [ ] No — los usuarios **pueden** registrar cuentas con nombres sensibles o administrativos.

### ¿Es la política de nombres de usuario consistente en todos los puntos de entrada (API, Móvil, Web)?
- [ ] Sí — las políticas **se aplican** de manera consistente en todas las interfaces y versiones.
- [ ] No — los endpoints heredados o versiones específicas de la API **no aplican** las mismas restricciones de nombre de usuario.

---

## WSTG-ATHN-01 — Pruebas de transporte de credenciales a través de un canal cifrado

Las pruebas de transporte de credenciales a través de un canal cifrado garantizan que los datos de autenticación sensibles, como nombres de usuario, contraseñas y tokens de sesión, estén protegidos contra la interceptación durante el tránsito. Los atacantes posicionados en la red —por ejemplo, mediante ataques Man-in-the-Middle (MitM) en redes Wi-Fi públicas o redes internas comprometidas— pueden utilizar herramientas de sniffing de paquetes para capturar credenciales en texto claro si no se aplica correctamente TLS/SSL. Esta vulnerabilidad suele ocurrir en los endpoints de inicio de sesión, formularios de restablecimiento de contraseña y cabeceras de autenticación de API donde la aplicación no exige HTTPS o realiza redirecciones incorrectas desde HTTP. Una explotación exitosa otorga al atacante acceso total a las cuentas de usuario, lo que conlleva al compromiso total de los datos del usuario y a un posible movimiento lateral dentro de la aplicación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Herramientas:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### ¿Se exige HTTPS durante todo el proceso de autenticación?
- [ ] Sí — HSTS está **habilitado** y todo el tráfico se fuerza estrictamente a través de HTTPS  
- [ ] Sí — se produce la redirección a HTTPS, pero HSTS **no está habilitado**  
- [ ] No — las credenciales **pueden** enviarse a través de HTTP no cifrado  

### ¿Se sirven las páginas y formularios de inicio de sesión a través de un canal cifrado?
- [ ] Sí — la página que contiene el formulario de inicio de sesión se entrega vía HTTPS  
- [ ] No — la página de inicio de sesión se sirve a través de HTTP, permitiendo el secuestro de la acción del formulario (form-action hijacking) o el sniffing de credenciales  

### ¿Se aplica el flag `Secure` a todas las cookies de sesión sensibles?
- [ ] Sí — el flag `Secure` está **aplicado** a todas las cookies de autenticación y de sesión  
- [ ] Sí — el flag `Secure` está **aplicado** solo a algunas cookies  
- [ ] No — el flag `Secure` **no está aplicado**, lo que permite que las cookies sean exfiltradas a través de solicitudes no cifradas  

### ¿Soporta el servidor versiones de TLS débiles o suites de cifrado inseguras?
- [ ] No — solo TLS moderno (1.2+) y cifrados fuertes están **habilitados**  
- [ ] Sí — se **soportan** versiones legadas (TLS 1.0/1.1) o cifrados débiles (RC4, DES, CBC)  

### ¿Evita la aplicación la transmisión de credenciales a dominios de terceros a través de canales no cifrados?
- [ ] Sí — no se envían datos sensibles a dominios externos o todas las llamadas externas se fuerzan a través de HTTPS  
- [ ] No — las cabeceras de autenticación o credenciales se envían a endpoints de terceros (p. ej., analíticas o SSO) a través de HTTP

---

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

## WSTG-ATHN-03 — Pruebas de mecanismos de bloqueo de cuenta débiles

Un mecanismo de bloqueo de cuenta es un control crítico de defensa en profundidad diseñado para mitigar ataques de Brute Force y Credential Stuffing mediante la desactivación de una cuenta tras un número específico de intentos de autenticación fallidos. Sin una política de bloqueo robusta, los atacantes pueden utilizar herramientas automatizadas para adivinar contraseñas sistemáticamente en múltiples cuentas (Password Spraying) o dirigirse a una sola cuenta de alto valor hasta tener éxito. Esta vulnerabilidad se manifiesta típicamente en portales de inicio de sesión, formularios de restablecimiento de contraseña y endpoints de API que carecen de Rate Limiting o protección a nivel de cuenta. Desde la perspectiva de un atacante, un mecanismo de bloqueo débil o ausente permite intentos de contraseña infinitos, mientras que uno excesivamente agresivo puede ser explotado para causar un Denial of Service (DoS) distribuido contra usuarios legítimos al bloquear intencionadamente sus cuentas.

| Campo | Valor |
|---|---|
| **ID WSTG** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad aumenta a Alta si la aplicación maneja PII sensible, datos financieros, o si las cuentas administrativas son susceptibles a Brute Force sin ningún control secundario.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Herramientas:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### ¿Se aplica un mecanismo de bloqueo de cuenta tras un número determinado de intentos fallidos?
- [ ] Sí — la cuenta se bloquea tras un umbral definido y razonable (ej. 5-10 intentos)  
- [ ] Sí — la cuenta se bloquea pero solo tras un número excesivamente alto de intentos  
- [ ] No — la cuenta **no** puede ser bloqueada y permite Brute Force indefinido  

### ¿Se puede omitir (bypass) el mecanismo de bloqueo mediante técnicas comunes de evasión?
- [ ] No — el bloqueo se aplica en el lado del servidor (server-side) y **no** se ve afectado por cambios en el lado del cliente  
- [ ] Sí — **es posible** omitir el bloqueo mediante la rotación de direcciones IP o el uso de un pool de proxies  
- [ ] Sí — **es posible** omitir el bloqueo manipulando cabeceras HTTP como `X-Forwarded-For` o `User-Agent`  
- [ ] Sí — **es posible** omitir el bloqueo borrando cookies o iniciando nuevas sesiones  

### ¿Proporciona el mecanismo de bloqueo una vía para la recuperación o expiración de la cuenta?
- [ ] Sí — la cuenta se desbloquea automáticamente tras una duración establecida o mediante verificación por correo electrónico  
- [ ] Sí — la cuenta requiere la intervención administrativa para ser desbloqueada  
- [ ] No — el bloqueo es permanente sin una vía de recuperación clara, lo que aumenta el riesgo de DoS  

### ¿Diferencia la aplicación entre un nombre de usuario válido e inválido durante el bloqueo?
- [ ] No — la respuesta de la aplicación es idéntica tanto para usuarios existentes como para los no existentes  
- [ ] Sí — la aplicación revela si una cuenta está bloqueada, lo que permite la **enumeración de nombres de usuario** (username enumeration)  

### ¿Es el mecanismo de bloqueo susceptible a un ataque de Denegación de Servicio (DoS)?
- [ ] No — controles como CAPTCHA o Rate Limiting basado en IP previenen el bloqueo masivo de cuentas  
- [ ] Sí — un atacante **puede** bloquear sistemáticamente a todos los usuarios conocidos proporcionando contraseñas incorrectas

---

## WSTG-ATHN-04 — Testing for Bypassing Authentication Schema

La evasión del esquema de autenticación (Bypassing the authentication schema) implica identificar y explotar fallos en la lógica de la aplicación que permiten a un atacante obtener acceso a recursos protegidos sin proporcionar credenciales válidas. Esta vulnerabilidad ocurre típicamente cuando la aplicación depende de controles del lado del cliente, falla al aplicar la autorización del lado del servidor en cada solicitud, o deja "backdoors" administrativos y endpoints de depuración (debug) expuestos en producción. Los atacantes explotan estas debilidades realizando una navegación forzada (forced browsing) hacia URLs sensibles, manipulando parámetros HTTP como `authenticated=true`, o realizando spoofing de cabeceras para engañar a la aplicación y que esta asuma que ya se ha establecido una sesión. Una explotación exitosa resulta en una ruptura completa del perímetro de autenticación, lo que puede conducir a una exfiltración de datos no autorizada o al compromiso total del sistema.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Herramientas:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### ¿Se puede acceder directamente a páginas internas sensibles sin una sesión activa?
- [ ] No — el acceso directo resulta en una redirección al login o una respuesta 403/404  
- [ ] Sí — algunas páginas sensibles son accesibles pero proporcionan datos limitados o no sensibles  
- [ ] Sí — las páginas administrativas o específicas de usuario sensibles **son** totalmente accesibles sin autenticación *(Crítica)*  

### ¿Depende la aplicación de flags o parámetros del lado del cliente para determinar el estado de autenticación?
- [ ] No — el estado de autenticación se gestiona estrictamente a través del estado de la sesión en el servidor  
- [ ] Sí — existen flags del lado del cliente pero su modificación **no** otorga acceso a áreas protegidas  
- [ ] Sí — la modificación de parámetros (ej. `is_authenticated=true`, `user_role=admin`) **permite** la evasión de la autenticación  

### ¿Se puede evadir la autenticación mediante el spoofing o la manipulación de cabeceras HTTP específicas?
- [ ] No — las cabeceras como `X-Forwarded-For`, `Referer` o cabeceras personalizadas no tienen impacto en la autenticación  
- [ ] Sí — la manipulación de cabeceras (ej. `X-Custom-IP-Authorization`, `X-Remote-User`) **es posible** y otorga acceso no autorizado  

### ¿Existen rutas alternativas o artefactos de desarrollo que eludan el flujo de inicio de sesión estándar?
- [ ] No — todos los recursos protegidos están restringidos por una comprobación de autenticación centralizada y lista para producción  
- [ ] Sí — los endpoints heredados, rutas de depuración (ej. `/debug/login`) o versiones de la API olvidadas **son** accesibles sin credenciales  

### ¿Falla la aplicación al re-autenticar o verificar sesiones para acciones de alto privilegio?
- [ ] No — las acciones de alto privilegio o sensibles requieren un token de sesión fresco o válido  
- [ ] Sí — una vez que se encuentra una evasión en un endpoint, esta **se puede** utilizar para realizar acciones administrativas en toda la aplicación

---

## WSTG-ATHN-05 — Pruebas de Vulnerabilidades en la Función "Recordar Contraseña"

La funcionalidad de "Recordarme" o inicio de sesión persistente está diseñada para mantener el estado autenticado de un usuario a través de los reinicios del navegador mediante el almacenamiento de un token o credencial en el lado del cliente. Si esta característica está mal implementada —como al almacenar contraseñas en texto claro (plaintext), hashes débiles o tokens de baja entropía en cookies— expone al usuario a un secuestro de cuenta (account takeover) a través de Cross-Site Scripting (XSS), acceso físico o interceptación de red. Los pentesters evalúan la entropía de estos tokens de persistencia, los flags de seguridad aplicados al mecanismo de almacenamiento y el ciclo de vida de la sesión para asegurar que la conveniencia del acceso persistente no eluda controles de seguridad críticos como la Multi-Factor Authentication (MFA) o la invalidación de sesión. La explotación a menudo implica la recolección de estos tokens de larga duración para obtener acceso no autorizado a las cuentas sin requerir las credenciales originales.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta* |

> *La severidad se considera Alta si se almacenan credenciales en texto claro o hashes sin sal (unsalted) en la cookie persistente, o si el token permite evadir la Multi-Factor Authentication (MFA).

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Herramientas:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### ¿Implementa la aplicación una función de "Recordarme" o "Mantener sesión iniciada"?
- [ ] No — la función **no** existe en la aplicación  
- [ ] Sí — la función existe y utiliza tokens criptográficamente fuertes y no predecibles  
- [ ] Sí — la función existe pero utiliza tokens predecibles o de baja entropía *(Media)*  
- [ ] Sí — la función existe y almacena credenciales en texto claro o contraseñas codificadas en base64 *(Alta)*  

### ¿Se aplican correctamente los flags de seguridad a la cookie persistente?
- [ ] Sí — los flags `HttpOnly`, `Secure` y `SameSite` están **aplicados**  
- [ ] Sí — se aplican algunos flags pero **falta** `HttpOnly`  
- [ ] Sí — se aplican algunos flags pero **falta** `Secure` sobre HTTPS  
- [ ] No — no se han **aplicado** flags de seguridad a la cookie persistente  

### ¿El token de "Recordarme" sigue siendo válido después de un cierre de sesión manual?
- [ ] No — el token se invalida en el lado del servidor inmediatamente al cerrar la sesión  
- [ ] Sí — el token permanece válido pero requiere una contraseña para acciones sensibles  
- [ ] Sí — el token permanece válido y permite el acceso completo a la cuenta **sin** re-autenticación *(Media)*  

### ¿Se finaliza la sesión persistente al realizar un cambio de contraseña?
- [ ] Sí — todos los tokens persistentes son revocados cuando el usuario cambia su contraseña  
- [ ] No — el token de "Recordarme" permanece válido después de que **se realiza** un restablecimiento o cambio de contraseña *(Alta)*  

### ¿El token persistente evade la Multi-Factor Authentication (MFA) en visitas posteriores?
- [ ] No — se requiere MFA incluso cuando hay un token de "Recordarme" presente  
- [ ] Sí — se evade la MFA, pero el token está vinculado a una huella digital (fingerprint) de dispositivo específica  
- [ ] Sí — la MFA se **evade completamente** utilizando solo la cookie persistente desde cualquier dispositivo *(Alta)*

---

## WSTG-ATHN-06 — Pruebas de debilidad en la caché del navegador

La debilidad en la caché del navegador ocurre cuando la información sensible se almacena en la caché local del navegador y puede ser recuperada por un usuario no autorizado con acceso a la misma máquina física. Esta falta de encabezados de control de caché adecuados permite que datos potencialmente sensibles, como información personal, detalles de la cuenta o identificadores de sesión, persistan después de que el usuario haya cerrado la sesión o cerrado el navegador. Los atacantes o usuarios posteriores en terminales compartidos o públicos pueden explotar esto navegando a través del historial del navegador, usando el botón "Atrás" o inspeccionando los archivos de la caché local para exfiltrar respuestas almacenadas en caché. Garantizar la implementación de directivas estrictas como `Cache-Control: no-store` y `Pragma: no-cache` es fundamental para cualquier aplicación que maneje contenido autenticado, con el fin de asegurar que los datos sensibles nunca se escriban en el disco.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Baja / Media* |

> *La severidad pasa a ser Media si se accede a la aplicación frecuentemente desde quioscos compartidos/públicos o si contiene datos PII/PHI altamente sensibles.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Herramientas:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### ¿Están presentes los encabezados de control de caché adecuados en las páginas autenticadas o sensibles?
- [ ] Sí — `Cache-Control: no-store, no-cache` y `Pragma: no-cache` están **presentes** y **configurados correctamente**  
- [ ] Sí — algunos encabezados están presentes pero **falta** `no-store`, permitiendo el almacenamiento en caché en disco  
- [ ] No — no se **aplican** encabezados relacionados con la caché, dependiendo del comportamiento predeterminado del navegador  

### ¿Utiliza la aplicación la directiva `private` para los datos específicos del usuario?
- [ ] Sí — `Cache-Control: private` está **habilitado** para todo el contenido personalizado para evitar el almacenamiento en caché en proxies  
- [ ] No — el contenido está marcado como `public` o carece de la directiva `private`, lo que lo hace **almacenable en caché** por proxies intermedios  

### ¿Es accesible la información sensible a través del botón "Atrás" del navegador después de un cierre de sesión exitoso?
- [ ] No — el navegador solicita una nueva autenticación o la página **no se carga** desde la caché local  
- [ ] Sí — la información sensible **sigue siendo visible** a través del botón "Atrás" después de que la sesión ha finalizado  

### ¿Están protegidos contra el almacenamiento en caché los archivos no-HTML sensibles (por ejemplo, PDFs, exportaciones CSV, respuestas JSON de API)?
- [ ] No — la aplicación no genera ni maneja archivos no-HTML sensibles  
- [ ] Sí — se **aplican** encabezados estrictos de control de caché a todas las descargas de archivos sensibles y endpoints de API  
- [ ] No — las descargas sensibles o las respuestas de la API se **almacenan** en la caché local del navegador o en directorios temporales  

### ¿Utiliza la aplicación `Expires: 0` o una fecha en el pasado para evitar el uso de datos obsoletos?
- [ ] Sí — el encabezado `Expires` está configurado en `0` o en una fecha histórica, **asegurando** que el navegador trate el contenido como inmediatamente obsoleto  
- [ ] No — el encabezado `Expires` **falta** o está configurado en una fecha futura para páginas sensibles

---

## WSTG-ATHN-07 — Pruebas de Política de Contraseñas Débiles

Las pruebas de políticas de contraseñas débiles consisten en evaluar los requisitos de complejidad, longitud y entropía aplicados por una aplicación durante la creación de cuentas y las actualizaciones de credenciales. Las políticas insuficientes comprometen directamente la confidencialidad de las cuentas de usuario al hacerlas susceptibles a ataques de diccionario automatizados, intentos de Brute Force y Credential Stuffing. Estas vulnerabilidades suelen residir en los endpoints de registro, flujos de trabajo de restablecimiento de contraseña y paneles administrativos de gestión de usuarios. Desde la perspectiva de un atacante, una política permisiva reduce significativamente el coste computacional y el tiempo necesarios para comprometer credenciales con éxito utilizando wordlists comunes o cracking basado en patrones.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Baja* |

> *La severidad pasa a ser Alta si la aplicación carece de mecanismos de bloqueo de cuenta o Rate Limiting para prevenir el guessing automatizado.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Herramientas:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### ¿Aplica la aplicación una longitud mínima de contraseña?
- [ ] Sí — la longitud mínima es de 12+ caracteres *(Más seguro)*  
- [ ] Sí — la longitud mínima es de entre 8 y 11 caracteres  
- [ ] Sí — la longitud mínima es **menor de** 8 caracteres  
- [ ] No — no se aplica ninguna longitud mínima  

### ¿Se exigen requisitos de complejidad (mayúsculas, minúsculas, dígitos, símbolos)?
- [ ] Sí — el uso de múltiples clases de caracteres es obligatorio y el **bypass** no es posible  
- [ ] Sí — se sugieren clases de caracteres pero **no** se exigen  
- [ ] No — no se aplican requisitos de complejidad  

### ¿Bloquea la aplicación contraseñas comunes/débiles o contraseñas que contienen datos específicos del usuario?
- [ ] Sí — las contraseñas débiles conocidas (ej. "password123") y las basadas en el nombre de usuario **no** pueden utilizarse  
- [ ] Sí — se bloquean las contraseñas comunes, pero se **pueden** utilizar datos específicos del usuario (ej. nombre de usuario, correo electrónico)  
- [ ] No — se acepta cualquier cadena que cumpla con los requisitos de longitud/complejidad  

### ¿Pueden omitirse los requisitos de complejidad o longitud mediante una modificación en el lado del cliente (Client-Side)?
- [ ] No — las políticas se aplican estrictamente en el **lado del servidor (Server-Side)**  
- [ ] Sí — las políticas se aplican mediante JavaScript y **pueden** omitirse interceptando la solicitud  

### ¿Se comunica claramente la política de contraseñas al usuario sin filtrar detalles de implementación?
- [ ] Sí — la política es visible durante la entrada de datos y proporciona mensajes de error genéricos  
- [ ] Sí — la política es visible, pero los mensajes de error revelan regex específicos o brechas en la lógica  
- [ ] No — la política **no** es visible, lo que obliga al descubrimiento mediante ensayo y error

---

## WSTG-ATHN-08 — Pruebas de Respuestas a Preguntas de Seguridad Débiles

Las pruebas de respuestas a preguntas de seguridad débiles consisten en evaluar los mecanismos de autenticación basada en el conocimiento (KBA) utilizados para verificar la identidad de un usuario, generalmente durante el restablecimiento de contraseñas o en flujos de autenticación de múltiples factores. Esto es relevante porque las preguntas de seguridad a menudo dependen de información estática y no secreta que puede ser descubierta fácilmente mediante inteligencia de fuentes abiertas (OSINT) o ingeniería social, lo que conduce a una toma de control de cuentas (Account Takeover) no autorizada. Estas vulnerabilidades suelen ocurrir en los módulos de "Olvidé mi contraseña" o "Desbloqueo de cuenta", donde los usuarios proporcionan respuestas a preguntas preestablecidas. Desde la perspectiva de un atacante, este es un objetivo principal para realizar Brute Force automatizado o investigación dirigida, ya que muchos usuarios proporcionan respuestas predecibles o públicamente verificables a preguntas comunes como "¿Cuál es el apellido de soltera de su madre?" o "¿A qué escuela secundaria asistió?".

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Estado de la Prueba** | No Realizado |
| **Severidad** | Media / Alta* |

> *La severidad se considera Alta si la pregunta de seguridad es el único requisito para un restablecimiento completo de contraseña y la toma de control de la cuenta sin verificaciones adicionales.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Herramientas:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### ¿Implementa la aplicación preguntas de seguridad para la verificación de identidad o la recuperación de contraseñas?
- [ ] No — las preguntas de seguridad **no se utilizan** para la autenticación o recuperación  
- [ ] Sí — las preguntas de seguridad **se utilizan** como un factor secundario opcional  
- [ ] Sí — las preguntas de seguridad **se utilizan** como el mecanismo de recuperación principal o único *(Riesgo Alto)*  

### ¿Las preguntas de seguridad se seleccionan de una lista fija o son definidas por el usuario?
- [ ] No — las preguntas son definidas enteramente por el usuario y requieren una alta entropía  
- [ ] Sí — las preguntas son fijas pero incluyen opciones que no son descubribles mediante OSINT  
- [ ] Sí — las preguntas provienen de una lista fija de temas comunes descubribles mediante OSINT *(Vulnerable)*  

### ¿Se aplica Rate Limiting o bloqueo de cuenta a los intentos de respuesta a las preguntas de seguridad?
- [ ] Sí — se aplican políticas estrictas de **Rate Limiting** y bloqueo de cuenta para prevenir el Brute Force  
- [ ] Sí — se aplica **Rate Limiting** pero es posible realizar un **bypass** mediante rotación de IP o manipulación de cabeceras  
- [ ] No — **no se aplica Rate Limiting** en los intentos de respuesta  

### ¿La aplicación impone complejidad o validación en el contenido de la respuesta?
- [ ] Sí — la validación impide el uso de palabras comunes y garantiza la complejidad o unicidad de la respuesta  
- [ ] Sí — existe una validación básica pero **puede** ser omitida con listas de palabras comunes o ataques de diccionario  
- [ ] No — **no se realiza validación**; se permiten respuestas simples o de un solo carácter  

### ¿Puede omitirse el desafío de la pregunta de seguridad mediante fallos lógicos?
- [ ] No — el flujo de recuperación requiere estrictamente una respuesta válida para continuar  
- [ ] Sí — el flujo de recuperación **puede** ser vulnerado mediante un **bypass** manipulando códigos de respuesta HTTP o parámetros de sesión  
- [ ] Sí — la respuesta se refleja en el código fuente o en campos ocultos de la página

---

## WSTG-ATHN-09 — Pruebas de Funcionalidades de Cambio o Restablecimiento de Contraseña Débiles

Los mecanismos de cambio y restablecimiento de contraseña son componentes críticos de la gestión de la autenticación que, si se implementan incorrectamente, permiten a los atacantes tomar el control de las cuentas de usuario. Estas vulnerabilidades suelen implicar tokens de restablecimiento predecibles, falta de bloqueos de cuenta durante intentos de Brute Force, entrega insegura de tokens o la posibilidad de cambiar contraseñas sin verificar la contraseña actual. Los atacantes explotan estos fallos interceptando o adivinando enlaces de recuperación, realizando Host Header Injection para redirigir correos electrónicos de restablecimiento o utilizando Session Fixation para mantener el acceso tras un cambio de contraseña. Garantizar que estos procesos requieran una verificación sólida y utilicen aleatoriedad criptográfica es esencial para mantener la integridad de la cuenta y evitar el acceso no autorizado.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### ¿Se requiere la contraseña actual para establecer una nueva contraseña durante un cambio estándar?
- [ ] Sí — la contraseña actual **es requerida** y verificada  
- [ ] Sí — la contraseña actual **es requerida** pero puede omitirse mediante Parameter Pollution o manipulación  
- [ ] No — la contraseña actual **no es** solicitada, lo que permite el Account Takeover mediante Session Hijacking *(Alta)*  

### ¿Son los tokens de restablecimiento de contraseña criptográficamente seguros e impredecibles?
- [ ] Sí — los tokens tienen alta entropía, son aleatorios y **no son predecibles**  
- [ ] Sí — se generan tokens pero muestran baja entropía o patrones predecibles (p. ej., basados en marcas de tiempo)  
- [ ] No — los tokens son **predecibles** o se basan en datos estáticos del usuario como email/ID codificados en Base64 *(Crítica)*  

### ¿Se puede redirigir el enlace de restablecimiento de contraseña mediante Host Header Injection?
- [ ] No — la aplicación ignora o valida el encabezado `Host` para la generación del enlace  
- [ ] Sí — la aplicación utiliza el encabezado `Host` o `X-Forwarded-Host` para construir los enlaces, pero la validación **está presente**  
- [ ] Sí — los enlaces **pueden** ser redirigidos a un dominio controlado por el atacante mediante la manipulación de encabezados *(Alta)*  

### ¿Tiene el token de restablecimiento un ciclo de vida limitado e invalidación inmediata?
- [ ] Sí — los tokens expiran rápidamente y se invalidan **inmediatamente** después de su uso o del cambio de contraseña  
- [ ] Sí — los tokens expiran tras una duración prolongada (p. ej., más de 24 horas) o permiten **múltiples usos**  
- [ ] No — los tokens **nunca expiran** o permanecen válidos después de que la contraseña ha sido cambiada con éxito  

### ¿Existen límites de tasa (Rate Limiting) o bloqueos en los endpoints de restablecimiento y cambio de contraseña?
- [ ] Sí — **se aplican** Rate Limiting estricto y CAPTCHAs para evitar la enumeración y el Brute Force  
- [ ] Sí — existen controles limitados pero su elusión (bypass) **es posible** mediante rotación de IP o spoofing de encabezados  
- [ ] No — no **se aplica** Rate Limiting, lo que permite la enumeración masiva de cuentas o el Brute Force de tokens

---

## WSTG-ATHN-10 — Pruebas de autenticación débil en canales alternativos

Las pruebas de autenticación débil en canales alternativos consisten en identificar y analizar rutas secundarias de acceso a la cuenta —como APIs móviles, flujos de recuperación de contraseñas, interfaces de soporte técnico o subdominios heredados (legacy)— que pueden no aplicar el mismo rigor de seguridad que el portal web principal. Estos canales alternativos suelen carecer de una Multi-factor Authentication (MFA) robusta, de un Rate Limiting estricto o de requisitos de contraseñas complejas, creando un "eslabón más débil" que compromete todo el sistema de autenticación. Los atacantes se dirigen específicamente a estos puntos de entrada ignorados para realizar Credential Stuffing o eludir la MFA aprovechando las discrepancias en cómo las diferentes interfaces gestionan la verificación de identidad. Garantizar la paridad en todas las superficies de autenticación es esencial para prevenir el acceso no autorizado y mantener la integridad de la sesión y los datos del usuario.

| Campo | Valor |
|---|---|
| **ID de WSTG** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### ¿Existen y se han identificado canales de autenticación alternativos?
- [ ] No — solo existe un único canal de autenticación  
- [ ] Sí — se identificaron múltiples canales (p. ej., API de aplicación móvil, cliente de escritorio, portal heredado, servicios SOAP)  

### ¿Los canales alternativos imponen una paridad de seguridad con el canal principal?
- [ ] Sí — todos los canales aplican requisitos **idénticos** de complejidad de contraseña y MFA  
- [ ] Sí — los canales aplican complejidad de contraseña, pero la MFA **no es obligatoria** o **puede** ser eludida  
- [ ] No — los canales alternativos tienen requisitos de autenticación **significativamente más débiles**  

### ¿Son consistentes el Rate Limiting y el bloqueo de cuentas en todos los canales?
- [ ] Sí — el Rate Limiting y el bloqueo se **aplican de manera consistente** en todos los endpoints  
- [ ] Sí — existen controles, pero la elusión (bypass) **es posible** en canales específicos (p. ej., API móvil)  
- [ ] No — el Rate Limiting o el bloqueo **no se aplican** a las rutas de autenticación secundarias  

### ¿Se puede eludir la MFA cambiando a un canal alternativo?
- [ ] No — la MFA es obligatoria y **no puede** ser eludida independientemente del punto de entrada  
- [ ] Sí — la MFA es requerida en el portal web pero **no se aplica** en la API móvil o heredada  

### ¿Son los flujos de recuperación de contraseña o de "olvidé mi contraseña" más débiles que el inicio de sesión estándar?
- [ ] No — los flujos de recuperación utilizan una verificación sólida fuera de banda (out-of-band) y no filtran información  
- [ ] Sí — los flujos de recuperación utilizan preguntas de seguridad débiles que **pueden** ser adivinadas o investigadas fácilmente  
- [ ] Sí — los flujos de recuperación son vulnerables a la enumeración o **no requieren** el mismo nivel de prueba que un inicio de sesión estándar

---

## WSTG-ATHN-11 — Pruebas de Autenticación de Múltiple Factor (MFA)

Las pruebas de autenticación de múltiple factor (MFA) evalúan la robustez de la capa de seguridad secundaria diseñada para prevenir el acceso no autorizado incluso cuando las credenciales primarias están comprometidas. Los atacantes intentan omitir el MFA con frecuencia identificando puntos finales (endpoints) donde no se aplica de manera consistente, como versiones heredadas (legacy) de API, backends móviles o flujos de trabajo de restablecimiento de contraseña. La explotación a menudo implica la manipulación de respuestas del servidor, el ataque de fuerza bruta (Brute Force) a códigos de corta duración (OTP) o la explotación de condiciones de carrera (race conditions) y fallos en la gestión de sesiones que permiten al usuario pasar a un estado autenticado sin proporcionar el segundo factor.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### ¿Se aplica el MFA de manera consistente en todos los portales de autenticación?
- [ ] Sí — El MFA es obligatorio para todos los intentos de inicio de sesión basados en web, móviles y API  
- [ ] Sí — Pero el MFA **no se aplica** en puntos finales específicos (ej. `/api/v1/login` heredado o portales específicos para móviles)  
- [ ] No — El MFA **no está implementado** en la aplicación *(Crítico)*  

### ¿Se puede omitir el paso de verificación de MFA mediante la navegación directa por URL o la manipulación de respuestas?
- [ ] No — La navegación directa o la manipulación de parámetros **no pueden** omitir el desafío  
- [ ] Sí — Navegar directamente a las URL del panel de control (dashboard) interno **es posible** sin completar el desafío de MFA  
- [ ] Sí — Manipular la respuesta del servidor (ej. cambiar un `401 Unauthorized` a `200 OK`) **es posible** para obtener acceso  

### ¿Está el proceso de verificación del código MFA protegido contra ataques de fuerza bruta (Brute Force)?
- [ ] Sí — Se aplica una limitación de tasa (Rate Limiting) estricta o bloqueo de cuenta tras múltiples intentos fallidos de OTP  
- [ ] Sí — Existe una limitación de tasa (Rate Limiting), pero **es posible** omitirla mediante rotación de IP o manipulación de cabeceras (headers)  
- [ ] No — No se aplica ninguna limitación de tasa (Rate Limiting) y el ataque automatizado de fuerza bruta (Brute Force) de códigos **es posible**  

### ¿Mantiene la aplicación un estado de sesión seguro durante la transición al MFA?
- [ ] Sí — Los tokens de sesión de altos privilegios se emiten **solo después** de completar con éxito el MFA  
- [ ] No — Se emite una cookie de sesión totalmente funcional **antes** de completar el MFA, permitiendo el acceso a algunas funciones  
- [ ] No — La aplicación utiliza un identificador estático o una transición de sesión predecible que **puede** ser secuestrada (hijacked)  

### ¿Son vulnerables a la explotación los factores de MFA alternativos (SMS, Email, códigos de respaldo)?
- [ ] No — Todos los factores utilizan valores seguros, no predecibles y limitados en el tiempo  
- [ ] Sí — Los códigos de respaldo son predecibles o **pueden** ser enumerados  
- [ ] Sí — La funcionalidad "Reenviar código" puede ser abusada para realizar inundaciones (flooding) de SMS/Email o revelar detalles de contacto parciales

---

## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Las vulnerabilidades de Directory Traversal y File Inclusion surgen cuando una aplicación utiliza entradas controlables por el usuario para construir rutas a archivos o directorios sin una validación o sanitización suficiente. Los atacantes explotan estos fallos inyectando secuencias como `../` para navegar fuera del directorio previsto, accediendo potencialmente a archivos sensibles del sistema, datos de configuración o código fuente de la aplicación. En casos más graves que involucran Local File Inclusion (LFI) o Remote File Inclusion (RFI), un atacante puede lograr la ejecución remota de código (RCE) incluyendo scripts maliciosos o aprovechando el log poisoning y wrappers de PHP. Estas vulnerabilidades suelen residir en parámetros utilizados para la carga dinámica de contenido, motores de plantillas o endpoints de recuperación de imágenes donde la lógica del lado del servidor maneja rutas del sistema de archivos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Herramientas:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### ¿Aceptan los parámetros nombres de archivos o rutas para su procesamiento en el lado del servidor?
- [ ] No — ningún parámetro parece interactuar con el sistema de archivos  
- [ ] Sí — existen parámetros pero utilizan una lista de permitidos (allowlist) estricta de identificadores de archivos  
- [ ] Sí — los parámetros aceptan nombres de archivos o rutas directas  

### ¿Se sanitiza la entrada para prevenir secuencias de directory traversal?
- [ ] Sí — la entrada se valida contra una lista de permitidos estricta y las secuencias de salto de directorio **no son posibles**  
- [ ] Sí — la entrada se sanitiza eliminando secuencias `../`, pero un bypass recursivo **es posible**  
- [ ] No — no **se aplica** ninguna sanitización o validación a la entrada relacionada con rutas  

### ¿Es posible acceder a archivos fuera del directorio restringido mediante secuencias de salto de directorio?
- [ ] No — la aplicación o el SO impiden el acceso a archivos fuera del alcance definido  
- [ ] Sí — el acceso a archivos dentro de la raíz web **es posible**  
- [ ] Sí — el acceso a archivos sensibles del sistema (ej. `/etc/passwd`, `C:\Windows\win.ini`) **es posible** *(Crítica)*  

### ¿Permite el servidor la inclusión de URLs remotas (RFI)?
- [ ] No — la inclusión de archivos remotos está **deshabilitada** a nivel de servidor/aplicación  
- [ ] Sí — los archivos remotos **pueden** ser incluidos pero la ejecución **no es posible**  
- [ ] Sí — los archivos remotos **pueden** ser incluidos y ejecutados en el servidor *(Crítica)*  

### ¿Se pueden omitir (bypass) los filtros utilizando codificación o caracteres especiales?
- [ ] No — los filtros manejan eficazmente varias codificaciones y bytes nulos  
- [ ] Sí — el bypass **es posible** utilizando URL encoding, double URL encoding o Unicode de 16 bits  
- [ ] Sí — el bypass **es posible** utilizando inyección de byte nulo (`%00`) o wrappers del sistema de archivos (ej. `php://filter`)

---

## WSTG-ATHZ-02 — Pruebas de evasión del esquema de autorización

La evasión de esquemas de autorización (Bypassing authorization schemas) ocurre cuando una aplicación no logra aplicar los controles de acceso, permitiendo que un atacante acceda a recursos o funciones fuera de sus permisos previstos. Esta vulnerabilidad se manifiesta típicamente como Escalada de Privilegios Horizontal (Horizontal Privilege Escalation), donde un atacante accede a datos pertenecientes a otro usuario del mismo nivel, o Escalada de Privilegios Vertical (Vertical Privilege Escalation), donde un usuario con bajos privilegios obtiene capacidades administrativas. Los atacantes explotan estos fallos manipulando parámetros de solicitud como los IDs de recursos, modificando tokens de sesión o aprovechando cabeceras HTTP como `X-Original-URL` para eludir las restricciones basadas en rutas. Garantizar una autorización robusta es crítico para mantener la confidencialidad de los datos y prevenir acciones administrativas no autorizadas que podrían comprometer todo el sistema.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Herramientas:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### ¿Se previene la escalada de privilegios horizontal entre usuarios del mismo rol?
- [ ] Sí — los controles de autorización **se aplican** a cada solicitud de recurso y la evasión (**bypass**) no es posible  
- [ ] Sí — los controles de autorización **se aplican** pero pueden ser evadidos mediante IDOR o manipulación de parámetros  
- [ ] No — los usuarios **pueden** acceder a datos pertenecientes a otros usuarios simplemente cambiando un ID de recurso *(Alta)*  

### ¿Se previene la escalada de privilegios vertical para usuarios con bajos privilegios?
- [ ] No — la aplicación tiene un solo nivel de privilegios  
- [ ] Sí — las funciones administrativas **no pueden** ser accedidas por usuarios con bajos privilegios  
- [ ] Sí — las funciones administrativas están ocultas en la interfaz de usuario (UI) pero **pueden** ser accedidas mediante URL directa  
- [ ] No — los usuarios con bajos privilegios **pueden** realizar acciones administrativas manipulando roles o permisos *(Crítica)*  

### ¿Se puede evadir la autorización mediante la manipulación de verbos HTTP?
- [ ] No — los controles de acceso se aplican independientemente del método HTTP utilizado  
- [ ] Sí — cambiar el método (por ejemplo, de `GET` a `POST` o `PUT`) **evade** los controles de autorización  

### ¿Son accesibles los endpoints administrativos o restringidos sin ninguna autenticación?
- [ ] No — todos los endpoints sensibles requieren una sesión válida y permisos adecuados  
- [ ] Sí — algunos endpoints administrativos son accesibles si el atacante conoce la URL directa  
- [ ] Sí — el acceso no autenticado a funciones sensibles **es posible** *(Crítica)*  

### ¿Permiten las cabeceras HTTP la elusión de la autorización basada en rutas?
- [ ] No — la aplicación **no** es susceptible a evasiones basadas en cabeceras  
- [ ] Sí — cabeceras como `X-Original-URL`, `X-Rewrite-URL` o `X-Forwarded-For` **pueden** ser utilizadas para evadir los controles de acceso

---

## WSTG-ATHZ-03 — Pruebas de escalamiento de privilegios

El escalamiento de privilegios ocurre cuando un atacante explota vulnerabilidades para obtener acceso a recursos o funcionalidades reservadas para usuarios con niveles de autorización superiores o identidades diferentes. En la escalada vertical, un usuario estándar intenta acceder a funciones administrativas, mientras que la escalada horizontal implica acceder a datos que pertenecen a otro usuario con el mismo nivel de privilegios. Estas fallas se manifiestan típicamente en listas de control de acceso (ACL) mal implementadas, referencias directas inseguras a objetos (IDOR) o manipulación de parámetros dentro de tokens de sesión o identidad. Desde la perspectiva de un atacante, esto se logra a menudo manipulando IDs numéricos, modificando parámetros basados en roles en cookies o JWTs, o llamando directamente a endpoints de API ocultos que carecen de controles de autorización en el lado del servidor.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Herramientas:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### ¿La aplicación implementa múltiples niveles de privilegio o multitenencia?
- [ ] No — la aplicación es un sistema de usuario único o de rol único  
- [ ] Sí — se **han definido** múltiples roles o inquilinos y requieren pruebas de autorización  

### ¿Es posible el escalamiento horizontal de privilegios (acceso al mismo nivel)?
- [ ] No — se **aplican** controles de autorización a todos los identificadores de recursos y propietarios de sesiones  
- [ ] Sí — el acceso a los datos de otros usuarios **es posible** a través de IDOR o manipulación de parámetros  
- [ ] Sí — la filtración de datos **es posible**, pero la modificación de los datos de otros usuarios **no es posible**  

### ¿Es posible el escalamiento vertical de privilegios (de bajo a alto)?
- [ ] No — los endpoints administrativos aplican estrictamente controles de acceso basados en roles (RBAC)  
- [ ] Sí — los usuarios con bajos privilegios **pueden** acceder a las funciones administrativas mediante el acceso directo a la URL  
- [ ] Sí — las funciones administrativas **pueden** ser accedidas manipulando encabezados relacionados con roles, cookies o claims de JWT  

### ¿Se pueden modificar los roles o permisos de usuario a través de Mass Assignment o Parameter Pollution?
- [ ] No — los campos basados en roles son estrictamente de solo lectura y **no pueden** ser modificados por los usuarios  
- [ ] Sí — los permisos de usuario **pueden** elevarse incluyendo parámetros ocultos (por ejemplo, `role=admin`) en las actualizaciones de perfil o en el registro  

### ¿Se aplican los controles de autorización de manera consistente en todas las versiones de la API y métodos HTTP?
- [ ] Sí — los controles **se aplican** consistentemente en todas las versiones y métodos  
- [ ] No — las versiones heredadas de la API (por ejemplo, `/v1/`) o métodos HTTP específicos (por ejemplo, `PUT`, `DELETE`, `PATCH`) **evaden** la autorización  
- [ ] No — las funciones administrativas están ocultas en la interfaz de usuario pero están **habilitadas** a nivel de API para todos los usuarios

---

## WSTG-ATHZ-04 — Pruebas de Referencias Directas Inseguras a Objetos (Insecure Direct Object References)

Las Referencias Directas Inseguras a Objetos (IDOR) ocurren cuando una aplicación proporciona acceso directo a objetos basándose en la entrada suministrada por el usuario sin realizar una comprobación de autorización para verificar los permisos del solicitante. Esta vulnerabilidad impacta significativamente la confidencialidad e integridad, ya que permite a los atacantes visualizar, modificar o eliminar datos pertenecientes a otros usuarios o al sistema. Normalmente se manifiesta en parámetros de URL, datos del cuerpo POST o claves JSON que hacen referencia a claves internas de la base de datos, nombres de archivos o identificadores de cuenta. Desde la perspectiva de un atacante, la explotación implica manipular estos identificadores —a menudo mediante el incremento de enteros o fuzzing de UUID— para acceder a recursos fuera de su ámbito autorizado.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Herramientas:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### ¿Utiliza la aplicación identificadores directos de objetos en los parámetros de la solicitud?
- [ ] No — la aplicación utiliza referencias indirectas o mapas cifrados *(Más seguro)*  
- [ ] Sí — la aplicación utiliza identificadores no predecibles (ej. UUID largos/hashes)  
- [ ] Sí — la aplicación utiliza identificadores predecibles (ej. enteros incrementales o nombres de usuario)  

### ¿Se realiza la autorización en el lado del servidor para cada solicitud que involucre un identificador de objeto?
- [ ] Sí — los controles en el lado del servidor **no pueden** ser omitidos para ningún identificador  
- [ ] Sí — existen controles en el lado del servidor, pero su omisión (bypass) **es posible** mediante Parameter Pollution o manipulación de encabezados  
- [ ] No — no **se aplica** ninguna comprobación de autorización para verificar la propiedad del objeto solicitado  

### ¿Puede un atacante acceder o modificar objetos pertenecientes a otros usuarios (Horizontal IDOR)?
- [ ] No — el acceso a los datos de otros usuarios **no es posible**  
- [ ] Sí — la visualización de los datos de otros usuarios **es posible**, pero la modificación **no es posible**  
- [ ] Sí — tanto la visualización como la modificación de los datos de otros usuarios **son posibles** *(Crítico)*  

### ¿Es posible acceder a objetos de nivel administrativo o de sistema (Vertical IDOR)?
- [ ] No — el acceso a objetos con privilegios superiores **no es posible**  
- [ ] Sí — el acceso a configuraciones administrativas o archivos del sistema **es posible**  

### ¿Filtra la aplicación identificadores de objetos a través de búsquedas, metadatos u otros endpoints?
- [ ] No — los identificadores **no se exponen** involuntariamente  
- [ ] Sí — los identificadores de otros usuarios/objetos **pueden** ser enumerados a través de endpoints secundarios o perfiles públicos

---

## WSTG-ATHZ-05 — Testing for OAuth Weaknesses

Las debilidades en OAuth abarcan una variedad de fallos en la implementación de los protocolos OAuth 2.0 o OpenID Connect (OIDC), que a menudo resultan en un account takeover completo o en el acceso no autorizado a datos. Estas vulnerabilidades suelen manifestarse durante el flujo de autorización (authorization flow), específicamente en relación con la validación de `redirect_uri`, la entropía y verificación del parámetro `state`, y el manejo seguro de los tokens de acceso o de ID. Los atacantes explotan estas debilidades interceptando códigos de autorización, redirigiendo tokens a dominios controlados por el atacante mediante open redirects, o realizando Cross-Site Request Forgery (CSRF) para vincular la sesión de una víctima a la cuenta de un atacante. Debido a que OAuth a menudo sirve como un mecanismo de autenticación primario, una sola configuración incorrecta puede comprometer la integridad de todo el sistema de identidad de los usuarios.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Herramientas:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### ¿Se ha implementado y validado el parámetro `state` para prevenir CSRF?
- [ ] Sí — `state` es obligatorio, único por sesión y se valida estrictamente en el servidor.
- [ ] Sí — `state` está presente pero es estático o predecible entre diferentes sesiones.
- [ ] Sí — `state` está presente pero la aplicación **no** lo valida en el callback.
- [ ] No — el parámetro `state` **no** se utiliza en la solicitud de autorización *(Crítica)*.

### ¿Se valida estrictamente el `redirect_uri` contra una lista blanca (whitelist)?
- [ ] Sí — solo se **aceptan** coincidencias exactas contra una whitelist prerregistrada.
- [ ] Sí — se aplica validación pero es **posible un bypass** mediante path traversal o manipulación de subdominios.
- [ ] Sí — se aplica validación pero es **posible un bypass** mediante parameter pollution o inyección de fragmentos (fragment injection).
- [ ] No — la aplicación **permite** URLs arbitrarias en el parámetro `redirect_uri` *(Crítica)*.

### ¿Puede manipularse el `response_type` para alterar el flujo?
- [ ] No — la aplicación solo acepta el flujo previsto (por ejemplo, `code`) y rechaza otros.
- [ ] Sí — cambiar de `code` a `token` (Implicit flow) **es posible** y filtra tokens en la URL.
- [ ] Sí — los flujos híbridos (hybrid flows) o valores de `response_type` no autorizados están **habilitados** y se procesan.

### ¿Se filtran los tokens de acceso o códigos de autorización mediante cabeceras Referer o el historial del navegador?
- [ ] No — los tokens/códigos se manejan de forma segura y las páginas sensibles utilizan una `Referrer-Policy` adecuada.
- [ ] Sí — los tokens/códigos **se filtran** a dominios de terceros a través de la cabecera `Referer`.
- [ ] Sí — los tokens/códigos **son visibles** en el historial del navegador debido a un almacenamiento en caché inseguro o solicitudes GET.

### ¿Aplica la aplicación el principio de "Mínimo Privilegio" (Least Privilege) para los alcances (scopes) de OAuth?
- [ ] Sí — la aplicación solicita solo los scopes mínimos necesarios para la funcionalidad.
- [ ] No — la aplicación solicita scopes excesivos o administrativos por defecto.
- [ ] No — el usuario **no puede** revisar o excluirse (opt-out) de scopes específicos durante la fase de consentimiento.

---

## WSTG-SESS-01 — Pruebas del Esquema de Gestión de Sesiones

Las pruebas del esquema de gestión de sesiones se centran en analizar cómo la aplicación maneja el ciclo de vida y la estructura de los identificadores de sesión para garantizar que sean robustos frente a la predicción y el secuestro (hijacking). Los atacantes capturan múltiples tokens de sesión para realizar un análisis estadístico, buscando patrones, baja entropía o incrementos predecibles que permitan falsificar sesiones válidas para otros usuarios. Las debilidades en el esquema a menudo se manifiestan como vulnerabilidades de Session Fixation o el uso de valores insuficientemente aleatorios, que normalmente se encuentran en cookies HTTP, parámetros URL o implementaciones de encabezados personalizados. Comprometer el esquema de sesión es un ataque de alto impacto que otorga al adversario acceso completo al estado autenticado de una víctima sin necesidad de sus credenciales principales.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Herramientas:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### ¿Es el identificador de sesión suficientemente complejo y aleatorio?
- [ ] Sí — el token supera las pruebas estadísticas de aleatoriedad (alta entropía) y el bypass **no es posible**  
- [ ] Sí — el token es largo pero contiene patrones predecibles o segmentos estáticos  
- [ ] No — el token es secuencial, basado en marcas de tiempo (timestamps) predecibles, o tiene una entropía **críticamente baja** *(Crítico)*  

### ¿Dónde se transmite y almacena el identificador de sesión?
- [ ] El identificador se almacena en una cookie `Secure` y `HttpOnly` *(Más seguro)*  
- [ ] El identificador se almacena en una cookie pero carece de los flags `Secure` o `HttpOnly`  
- [ ] El identificador se transmite **únicamente** a través de parámetros URL o peticiones `GET` *(Riesgo Alto)*  

### ¿Emite la aplicación un nuevo identificador de sesión tras una autenticación exitosa?
- [ ] Sí — **se genera** un nuevo ID de sesión único inmediatamente después del login  
- [ ] No — el ID de sesión sigue siendo el mismo antes y después del login *(Posible Session Fixation)*  

### ¿Está el identificador de sesión vinculado a una dirección IP o User-Agent específicos para evitar la reutilización?
- [ ] Sí — la sesión se invalida si la huella digital (fingerprint) del cliente cambia significativamente  
- [ ] No — la sesión **puede** ser reutilizada en diferentes direcciones IP o dispositivos sin verificación adicional  

### ¿Se invalidan correctamente los identificadores de sesión en el lado del servidor tras el cierre de sesión o el tiempo de espera (timeout)?
- [ ] Sí — el estado de la sesión en el lado del servidor se **destruye completamente** al cerrar la sesión  
- [ ] No — la sesión permanece válida en el servidor incluso si se elimina la cookie del lado del cliente  
- [ ] No — la sesión **nunca expira** o tiene un periodo de timeout excesivamente largo

---

## WSTG-SESS-02 — Testing for Cookies Attributes

La gestión de sesiones a menudo depende de las cookies HTTP para mantener el estado, lo que convierte a los atributos de seguridad de estas cookies en un objetivo principal para los atacantes. Al omitir o configurar incorrectamente atributos como `HttpOnly`, `Secure` y `SameSite`, las aplicaciones exponen los tokens de sesión al robo mediante Cross-Site Scripting (XSS), interceptación a través de canales no cifrados o ataques de Cross-Site Request Forgery (CSRF). Los pentesters examinan estas etiquetas para determinar si un atacante puede exfiltrar identificadores de sesión del modelo de objetos del documento (DOM) del navegador o forzar al navegador a enviarlos en solicitudes de origen cruzado (cross-origin). La configuración adecuada de los atributos es una medida fundamental de defensa en profundidad para garantizar que los tokens sensibles permanezcan limitados a contextos autorizados y seguros.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Herramientas:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Análisis de la Implementación de Etiquetas de Cookies
| Atributo | Presente | Ausente |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### ¿Se aplica la etiqueta `HttpOnly` a las cookies de sesión sensibles?
- [ ] Sí — aplicada a **todas** las cookies de sesión sensibles *(Más seguro)*  
- [ ] Sí — aplicada solo a algunas cookies de sesión  
- [ ] No — la etiqueta **no se aplica**, permitiendo el acceso a través de scripts del lado del cliente *(Crítico)*  

### ¿Se aplica la etiqueta `Secure` para las cookies transmitidas a través de HTTPS?
- [ ] Sí — aplicada a **todas** las cookies transmitidas a través de TLS  
- [ ] No — la etiqueta **no se aplica**, permitiendo que las cookies se envíen a través de HTTP en texto plano  

### ¿Se utiliza el atributo `SameSite` para mitigar los riesgos de CSRF?
- [ ] Sí — configurado como `Strict` o `Lax` para las cookies de sesión  
- [ ] Sí — configurado como `None` pero con la etiqueta `Secure` presente  
- [ ] No — el atributo **está ausente** o configurado como `None` **sin** `Secure`  

### ¿Están los atributos `Domain` y `Path` restringidos al alcance mínimo necesario?
- [ ] Sí — limitado al subdominio y la ruta de la aplicación **específicos**  
- [ ] No — el alcance es demasiado **amplio** (p. ej., configurado para un dominio primario o la raíz `/`), lo que aumenta la superficie de ataque  

### ¿Expiran correctamente las cookies de sesión sensibles a través de los atributos `Expires` o `Max-Age`?
- [ ] Sí — se han configurado fechas de expiración adecuadas  
- [ ] No — las cookies son persistentes con tiempos de vida excesivamente **largos**  
- [ ] No — las cookies son solo de sesión pero **carecen** de la aplicación de tiempo de espera en el lado del servidor

---

## WSTG-SESS-03 — Session Fixation

La fijación de sesión (Session Fixation) ocurre cuando una aplicación no invalida o rota el identificador de sesión después de que un usuario se autentica con éxito, lo que permite a un atacante forzar un token de sesión conocido a una víctima. Si la aplicación conserva el ID de sesión previo a la autenticación después del inicio de sesión, un atacante puede proporcionar a una víctima un enlace específicamente diseñado que contenga un ID de sesión fijo y, posteriormente, secuestrar la sesión autenticada. Esta vulnerabilidad se manifiesta típicamente en formularios de inicio de sesión o a través de identificadores de sesión aceptados mediante parámetros de URL y cookies. Desde la perspectiva de un atacante, la explotación consiste en "fijar" la sesión a través de un enlace malicioso o una vulnerabilidad de inyección de cabeceras y esperar a que la víctima proporcione sus credenciales, evitando eficazmente la necesidad de robar un token activo.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Estado de la Prueba** | No realizado |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### ¿Cambia el identificador de sesión después de una autenticación exitosa?
- [ ] Sí — **se emite** un nuevo identificador de sesión y el anterior es invalidado *(Más seguro)*  
- [ ] Sí — **se emite** un nuevo identificador de sesión, pero el anterior **permanece válido**  
- [ ] No — el identificador de sesión **sigue siendo el mismo** antes y después de la autenticación *(Crítico)*  

### ¿Acepta la aplicación identificadores de sesión proporcionados a través de parámetros de URL?
- [ ] No — los IDs de sesión se gestionan **exclusivamente** mediante cookies  
- [ ] Sí — se aceptan IDs de sesión en la URL, pero son **ignorados** o **rotados** al iniciar sesión  
- [ ] Sí — los IDs de sesión en la URL son **aceptados** y **persisten** después de la autenticación  

### ¿Se puede forzar un ID de sesión definido por el atacante en la aplicación?
- [ ] No — la aplicación **rechaza** IDs de sesión inválidos o inexistentes proporcionados por el usuario  
- [ ] Sí — la aplicación **acepta** e inicializa una sesión utilizando cualquier ID arbitrario proporcionado en una cookie  
- [ ] Sí — la aplicación **acepta** e inicializa una sesión utilizando cualquier ID arbitrario proporcionado en un parámetro de URL  

### ¿Se finalizan correctamente las sesiones previas a la autenticación?
- [ ] Sí — la sesión anónima se **destruye por completo** al producirse el evento de inicio de sesión  
- [ ] No — la sesión anónima **no se destruye**, lo que permite la posible recolección de sesiones  

### ¿Está la cookie de sesión protegida contra la inyección en el lado del cliente?
- [ ] Sí — se **aplican** los flags `HttpOnly` y `Secure` para evitar la fijación basada en scripts  
- [ ] No — los flags **no se aplican**, lo que permite establecer IDs de sesión a través de Cross-Site Scripting (XSS)

---

## WSTG-SESS-04 — Testing for Exposed Session Variables

Las variables de sesión expuestas ocurren cuando identificadores de sesión sensibles o datos relacionados con el estado se transmiten a través de canales inseguros como cadenas de consulta URL (URL query strings), encabezados Referer o registros del sistema (logs). Esta exposición incrementa significativamente el riesgo de secuestro de sesión (Session Hijacking), ya que los identificadores pueden almacenarse en el historial del navegador, ser registrados por proxies intermedios o filtrarse a sitios de terceros a través del encabezado Referer. Los pentesters identifican estas exposiciones monitoreando todas las peticiones salientes e inspeccionando los registros de la aplicación o las configuraciones del servidor para detectar filtraciones de datos involuntarias. En entornos reales, los atacantes explotan estas vulnerabilidades recolectando tokens de sesión válidos de equipos compartidos o analizando patrones de tráfico para evadir la autenticación y obtener acceso no autorizado a las cuentas de usuario.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Estado de la Prueba** | No Realizado |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si la variable expuesta es un token de sesión que permite la toma de control de la cuenta (Account Takeover) de forma inmediata.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Herramientas:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### ¿Se transmite el identificador de sesión en la cadena de consulta (URL query string)?
- [ ] No — los identificadores de sesión **no** están presentes en la URL query string  
- [ ] Sí — los identificadores están presentes pero la sesión es de corta duración o de bajo riesgo  
- [ ] Sí — identificadores de sesión (Session IDs) únicos **están** presentes en la URL query string *(Crítico)*  

### ¿La aplicación filtra tokens de sesión a través del encabezado Referer?
- [ ] No — la política `Referrer-Policy` evita la filtración o no existen enlaces a terceros  
- [ ] Sí — los tokens de sesión **son** transmitidos a dominios de terceros a través del encabezado Referer  

### ¿Se almacenan las variables de sesión en los registros (logs) de la aplicación o del servidor?
- [ ] No — las variables de sesión están enmascaradas o excluidas de los registros  
- [ ] Sí — las variables de sesión **se registran** en los logs de la aplicación o del servidor web en texto plano  

### ¿Utiliza la aplicación peticiones GET para operaciones que cambian el estado?
- [ ] No — la aplicación utiliza `POST` u otros métodos no idempotentes para operaciones sensibles  
- [ ] Sí — se utiliza `GET`, lo que provoca que los datos de sesión se almacenen en caché en el historial del navegador y en proxies intermedios  

### ¿Está configurado el encabezado `Cache-Control` para evitar el almacenamiento en caché de datos relacionados con la sesión?
- [ ] Sí — se aplica `Cache-Control: no-store` (o similar) a las respuestas sensibles  
- [ ] Sí — existen controles pero es posible un bypass a través de casos de borde (edge cases) del navegador  
- [ ] No — las respuestas sensibles que contienen datos de sesión **pueden** ser almacenadas en caché por el navegador

---

## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) es una vulnerabilidad en la que un atacante engaña al navegador de una víctima para que realice una acción no deseada en un sitio web diferente en el que la víctima está autenticada actualmente. Este exploit aprovecha el comportamiento del navegador de adjuntar automáticamente credenciales "ambientales", como cookies de sesión o encabezados de Authorization, a las solicitudes salientes. Los atacantes suelen dirigirse a operaciones que cambian el estado, como el cambio de contraseñas, la actualización de direcciones de correo electrónico o la ejecución de transferencias financieras, alojando scripts maliciosos o formularios ocultos en un sitio de terceros. Una explotación exitosa puede llevar al secuestro total de la cuenta (account takeover) o a la modificación de datos no autorizada sin el conocimiento o consentimiento del usuario.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Estado de la Prueba** | No Realizada |
| **Gravedad** | Media / Alta* |

> *La gravedad pasa a ser Alta si la acción vulnerable permite el secuestro de la cuenta, la escalada de privilegios o transacciones financieras no autorizadas.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Herramientas:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### ¿Se han implementado tokens anti-CSRF para las solicitudes que cambian el estado?
- [ ] Sí — se requieren tokens únicos y criptográficamente fuertes para todas las acciones que cambian el estado  
- [ ] Sí — los tokens están presentes pero **no** son únicos por sesión o son predecibles  
- [ ] No — **no** se han implementado tokens anti-CSRF  

### ¿Es robusta la validación del token anti-CSRF en el lado del servidor?
- [ ] Sí — el servidor valida estrictamente el token y **no es posible** realizar un bypass  
- [ ] Sí — la validación se realiza pero **se puede** evadir eliminando el parámetro del token  
- [ ] Sí — la validación se realiza pero **se puede** evadir proporcionando un token en blanco o ficticio  
- [ ] Sí — la validación se realiza pero el token **no** está vinculado a la sesión del usuario  

### ¿Depende la aplicación de métodos fácilmente evadibles para la protección contra CSRF?
- [ ] No — la aplicación no depende únicamente de encabezados débiles o comprobaciones de origen  
- [ ] Sí — la protección depende únicamente del encabezado `Referer` u `Origin`, el cual **puede** ser falsificado o eliminado  
- [ ] Sí — la protección depende de la comprobación del encabezado `X-Requested-With`, el cual **puede** ser evadido mediante configuraciones incorrectas de CORS  

### ¿Están configuradas las cookies de sesión con el atributo `SameSite`?
- [ ] Sí — `SameSite` está configurado como `Strict` o `Lax` en todas las cookies relacionadas con la sesión  
- [ ] No — el atributo `SameSite` **está ausente**, delegando en el comportamiento predeterminado del navegador  
- [ ] No — `SameSite` está configurado explícitamente como `None` sin el flag `Secure` o mitigaciones adicionales  

### ¿Se puede evadir la protección CSRF cambiando el método de solicitud HTTP?
- [ ] No — la protección se aplica independientemente del método HTTP utilizado  
- [ ] Sí — los tokens solo se validan en solicitudes `POST`, pero la acción **se puede** realizar a través de `GET`  
- [ ] Sí — cambiar el método (por ejemplo, a `PUT` o `DELETE`) evade la comprobación del token

---

## WSTG-SESS-06 — Pruebas de la Funcionalidad de Cierre de Sesión (Logout)

La funcionalidad de cierre de sesión es un control de seguridad crítico diseñado para terminar la sesión de un usuario e invalidar los identificadores de sesión asociados tanto en el lado del cliente como en el del servidor. El fallo al implementar correctamente el cierre de sesión permite ataques de Session Fixation o Hijacking, ya que un atacante que obtenga acceso a una máquina después de que un usuario haya "cerrado sesión" podría reutilizar el token de sesión que aún permanece activo. Los pentesters evalúan esto verificando si la cookie de sesión se elimina del navegador, si el estado de la sesión en el servidor se destruye explícitamente y si la navegación con el botón "Atrás" permite el acceso a datos sensibles almacenados en caché. Una implementación de logout segura garantiza que, una vez activada la acción, ninguna solicitud posterior con el antiguo token de sesión sea autorizada por la aplicación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Herramientas:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### ¿Existe un mecanismo de cierre de sesión funcional y accesible?
- [ ] Sí — el botón de logout existe y activa una solicitud de terminación  
- [ ] No — el botón de logout **no aparece** o **no** activa un evento de terminación  

### ¿Se invalida el identificador de sesión en el lado del servidor?
- [ ] Sí — el servidor rechaza todas las solicitudes posteriores que utilicen el antiguo token de sesión  
- [ ] No — el servidor **continúa aceptando** el antiguo token de sesión después del cierre de sesión  

### ¿La aplicación elimina las cookies de sesión en el navegador al cerrar la sesión?
- [ ] Sí — las cookies se sobrescriben con una fecha de expiración pasada o un valor vacío  
- [ ] Sí — las cookies permanecen pero **ya no son válidas** en el servidor  
- [ ] No — las cookies permanecen en el navegador y **conservan** sus valores originales  

### ¿Se puede acceder a contenido autenticado sensible mediante el botón 'Atrás' del navegador después del cierre de sesión?
- [ ] No — los encabezados `Cache-Control: no-store` o similares impiden ver páginas sensibles tras el cierre de sesión  
- [ ] Sí — las páginas sensibles están en caché y **pueden** ser visualizadas mediante la navegación después de que la sesión ha terminado

---

## WSTG-SESS-07 — Testing Session Timeout

Las pruebas de tiempo de espera de la sesión (Session Timeout) evalúan si una aplicación termina de manera efectiva la sesión de un usuario tras un periodo predefinido de inactividad o una duración total. Este control es vital para mitigar el riesgo de secuestro de sesión (Session Hijacking), particularmente en estaciones de trabajo compartidas o en escenarios donde un atacante captura un identificador de sesión a través de la interceptación de red o XSS. Los Pentesters analizan tanto los tiempos de espera por inactividad (Idle Timeouts), que se activan tras un periodo de inactividad, como los tiempos de espera absolutos (Absolute Timeouts), que limitan la vida útil total de una sesión independientemente de la actividad. Desde la perspectiva de un atacante, las sesiones indefinidas o excesivamente largas proporcionan una ventana extendida de oportunidad para mantener el acceso no autorizado y evadir la necesidad de re-autenticación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### ¿Implementa la aplicación un tiempo de espera de sesión por inactividad (Idle Session Timeout)?
- [ ] Sí — la sesión expira tras un periodo corto (ej. 15-30 minutos) de inactividad  
- [ ] Sí — la sesión expira pero la duración del tiempo de espera es **excesivamente larga** (ej. > 24 horas)  
- [ ] No — la sesión **permanece activa** indefinidamente durante la inactividad  

### ¿Se aplica el tiempo de espera de la sesión en el lado del servidor?
- [ ] Sí — el servidor rechaza peticiones con tokens expirados independientemente del estado en el lado del cliente  
- [ ] No — el tiempo de espera **solo** se aplica a través de JavaScript en el lado del cliente o meta-refresh  

### ¿Implementa la aplicación un tiempo de espera de sesión absoluto (Absolute Session Timeout)?
- [ ] Sí — la sesión se termina tras una duración máxima fija independientemente de la actividad  
- [ ] No — la sesión **puede** extenderse indefinidamente mientras haya actividad continua  

### ¿Se invalida el identificador de sesión en el servidor al agotarse el tiempo de espera?
- [ ] Sí — el token de sesión **no puede** ser reutilizado una vez alcanzado el umbral de tiempo de espera  
- [ ] No — el token de sesión **sigue siendo válido** en el servidor incluso después de que el cliente es redirigido a la página de inicio de sesión  

### ¿Puede mantenerse la sesión activa indefinidamente mediante peticiones automatizadas de tipo heartbeat?
- [ ] No — el tiempo de espera absoluto o controles secundarios **previenen** la extensión infinita de la sesión  
- [ ] Sí — las peticiones periódicas en segundo plano (ej. AJAX) **pueden** mantener la sesión indefinidamente

---

## WSTG-SESS-08 — Session Puzzling

El Session Puzzling, también conocido como Session Variable Overloading (Sobrecarga de Variables de Sesión), ocurre cuando una aplicación utiliza la misma variable de sesión para múltiples propósitos en diferentes módulos o no limpia adecuadamente los estados de sesión durante las transiciones de flujo de trabajo. Los atacantes explotan este comportamiento poblando variables de sesión en un contexto —como una vista previa de perfil no autenticada o un registro de varios pasos— y luego navegando hacia un área protegida donde la aplicación confía incorrectamente en esas variables preexistentes. Esto puede derivar en impactos críticos, incluyendo el Authentication Bypass, Privilege Escalation o el acceso no autorizado a datos sensibles al engañar a la aplicación para que crea que una sesión se encuentra en un estado de mayor privilegio del que realmente tiene. La vulnerabilidad es más frecuente en aplicaciones complejas con estado (stateful) donde la lógica de gestión de sesiones se aplica de forma inconsistente entre los diferentes componentes funcionales.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### ¿Utiliza la aplicación los mismos nombres de variables de sesión para diferentes propósitos en módulos distintos?
- [ ] No — los nombres de las variables son únicos o los alcances (scopes) están aislados  
- [ ] Sí — existen nombres de variables compartidos pero el estado se limpia entre módulos  
- [ ] Sí — existen nombres de variables compartidos y el estado **no se limpia**  

### ¿Pueden las acciones no autenticadas influir en las variables de sesión utilizadas en contextos autenticados?
- [ ] No — las variables de sesión se inicializan solo **después** de una autenticación exitosa  
- [ ] Sí — las variables de sesión pueden establecerse antes del login pero **no** afectan al estado post-login  
- [ ] Sí — las variables de sesión establecidas durante la pre-autenticación **son** confiables post-autenticación *(Crítico)*  

### ¿La aplicación reinicializa el identificador de sesión y sus variables asociadas ante un cambio en el nivel de privilegios?
- [ ] Sí — la sesión se restablece por completo y **se emite** un nuevo ID  
- [ ] Parcial — se emite un nuevo ID pero algunas variables de sesión **persisten**  
- [ ] No — el ID de sesión y todas las variables **permanecen** inalterados  

### ¿Se pueden obtener privilegios administrativos mediante la manipulación de variables de sesión a través de una cuenta sin privilegios?
- [ ] No — las variables de privilegio **no pueden** ser modificadas por los usuarios  
- [ ] Sí — las variables pueden ser modificadas pero la aplicación las **valida** contra la base de datos del backend  
- [ ] Sí — las variables pueden ser modificadas y la aplicación **confía** en el estado de la sesión implícitamente *(Crítico)*  

### ¿Se limpia el estado de la sesión inmediatamente después de completar un flujo de trabajo específico (ej. restablecimiento de contraseña, checkout)?
- [ ] Sí — las variables de sesión del flujo de trabajo se destruyen al finalizar  
- [ ] No — las variables del flujo de trabajo **persisten** y pueden ser reutilizadas en otras áreas de la aplicación

---

## WSTG-SESS-09 — Pruebas de secuestro de sesión (Session Hijacking)

El secuestro de sesión (Session Hijacking) ocurre cuando un atacante captura, predice o fija un identificador de sesión (SID) válido para obtener acceso no autorizado a la sesión activa de un usuario. Esta vulnerabilidad se manifiesta típicamente a través de una seguridad insuficiente en la capa de transporte, la falta de banderas de seguridad en las cookies o algoritmos de generación de SID predecibles que permiten a un atacante omitir la autenticación por completo. Desde la perspectiva de un atacante, la explotación exitosa permite la suplantación total de la víctima sin requerir sus credenciales, lo cual se logra a menudo mediante el rastreo de red (sniffing), Cross-Site Scripting (XSS) o ataques de fijación de sesión (session fixation).


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Herramientas:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### ¿Están los identificadores de sesión protegidos durante el tránsito?
- [ ] Sí — La bandera `Secure` está **habilitada** y HSTS se aplica estrictamente  
- [ ] Sí — La bandera `Secure` está **habilitada** pero HSTS está **deshabilitado**  
- [ ] No — La bandera `Secure` **no se aplica**, permitiendo la interceptación del SID a través de canales no cifrados *(Alta)*  

### ¿Está el identificador de sesión protegido contra el acceso por scripts del lado del cliente?
- [ ] Sí — La bandera `HttpOnly` se **aplica** a todas las cookies relacionadas con la sesión  
- [ ] No — La bandera `HttpOnly` **no se aplica**, permitiendo la exfiltración del SID vía XSS *(Crítica)*  

### ¿Implementa la aplicación el amarre de sesión (session binding) a atributos del cliente?
- [ ] Sí — La sesión está vinculada a atributos del cliente (IP/User-Agent) y su reutilización **no es posible**  
- [ ] Sí — El amarre de sesión está presente pero su omisión **es posible** mediante la suplantación de cabeceras (header spoofing)  
- [ ] No — No existe amarre de sesión; el SID **es válido** cuando se utiliza desde cualquier origen  

### ¿Es el identificador de sesión lo suficientemente aleatorio para prevenir la predicción?
- [ ] Sí — El SID utiliza un generador de números pseudoaleatorios criptográficamente seguro (CSPRNG)  
- [ ] No — La longitud del SID es insuficiente o sigue una secuencia/patrón **predecible**  

### ¿Se gestionan las sesiones concurrentes para limitar la ventana de ataque?
- [ ] No — La aplicación impone estrictamente una única sesión activa por usuario  
- [ ] Sí — Las sesiones concurrentes múltiples están **habilitadas** pero son monitoreadas  
- [ ] Sí — Las sesiones concurrentes ilimitadas **son posibles** a través de diferentes dispositivos/ubicaciones

---

## WSTG-SESS-10 — Testing JSON Web Tokens

Los JSON Web Tokens (JWT) se utilizan comúnmente para la gestión de sesiones sin estado (stateless) y la propagación de identidad, pero su seguridad depende enteramente de la implementación correcta de los algoritmos de firma y de una verificación estricta de los claims. Los atacantes intentan frecuentemente eludir la autenticación explotando fallos del algoritmo "none", realizando fuerza bruta (Brute Force) sobre claves secretas HS256 débiles, o llevando a cabo ataques de intercambio de algoritmos (algorithm-switching) donde una clave pública asimétrica se utiliza como un secreto HMAC simétrico. Estas vulnerabilidades suelen manifestarse en las cookies de sesión o en las cabeceras de Authorization, permitiendo potencialmente que un atacante realice una escalada de privilegios o suplante a cualquier usuario modificando claims como `role` o `user_id` sin invalidar la integridad criptográfica del token.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Estado de la Prueba** | No realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Herramientas:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### ¿Acepta el servidor el algoritmo "none"?
- [ ] No — el servidor **rechaza** tokens que utilicen `alg: none` en la cabecera (header)  
- [ ] Sí — el servidor **acepta** tokens con `alg: none` y una firma vacía *(Crítico)*  

### ¿Cómo se verifica la firma del JWT y cómo se protege contra fuerza bruta (Brute Force)?
- [ ] Sí — se utilizan claves asimétricas fuertes (RS256/ES256) o secretos HMAC de alta entropía  
- [ ] Sí — se utiliza HS256 pero el secreto **puede** ser descifrado por Brute Force debido a una baja entropía o a valores por defecto  
- [ ] No — la verificación de la firma **no se aplica** o **puede** eludirse mediante el intercambio de algoritmos (de RS256 a HS256)  

### ¿El backend impone estrictamente los claims estándar (exp, nbf, iat)?
- [ ] Sí — el claim `exp` (expiración) está presente y **no puede** eludirse  
- [ ] Sí — `exp` está presente pero el servidor **no** lo impone durante la validación  
- [ ] No — los claims de expiración **faltan** o se ignoran, lo que permite un Replay Attack de sesión indefinido  

### ¿Evita la implementación la inyección de claves basada en cabeceras (kid, jku, x5u)?
- [ ] Sí — las cabeceras se sanean y solo se **permiten** fuentes o claves internas de confianza  
- [ ] No — la cabecera `kid` (Key ID) es vulnerable a Directory Traversal o SQL Injection para apuntar a un archivo conocido  
- [ ] No — las cabeceras `jku` o `x5u` **pueden** apuntar a URLs controladas por el atacante para cargar claves maliciosas

---

## WSTG-SESS-11 — Testing for Concurrent Sessions

Las pruebas de sesiones concurrentes (Concurrent Sessions) evalúan si una aplicación permite que una única cuenta de usuario mantenga múltiples sesiones activas simultáneas en diferentes navegadores, dispositivos o direcciones IP. La falta de restricción de las sesiones concurrentes aumenta la ventana de oportunidad para que los atacantes utilicen tokens de sesión secuestrados (Session Hijacking) o credenciales comprometidas sin alertar al usuario legítimo ni finalizar las sesiones existentes. Este comportamiento se evalúa normalmente autenticándose desde múltiples fuentes y monitoreando la estabilidad de la sesión, lo que a menudo revela debilidades en la lógica de Session Management o una falta de reglas de negocio centradas en la seguridad. Desde la perspectiva de un atacante, la falta de control de concurrencia permite una persistencia sigilosa tras un compromiso inicial y facilita ataques paralelos automatizados.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### ¿Están limitadas las sesiones concurrentes por la política de la aplicación?
- [ ] Sí — solo **se permite** una sesión por usuario; los nuevos inicios de sesión invalidan los anteriores *(Más seguro)*  
- [ ] Sí — **se impone** un número fijo de sesiones concurrentes y no se puede exceder  
- [ ] No — **es posible** un número ilimitado de sesiones concurrentes  

### ¿Cómo responde la aplicación cuando se alcanza el límite de sesiones?
- [ ] La sesión activa más antigua **se finaliza automáticamente** (mitigación de session fixation/hijacking)  
- [ ] **Se solicita** al usuario que finalice las sesiones existentes antes de que se autorice la nueva sesión  
- [ ] El nuevo intento de inicio de sesión **se bloquea** hasta que se produzca un logout manual desde otro dispositivo  
- [ ] No se toma ninguna medida — múltiples sesiones permanecen **habilitadas** simultáneamente  

### ¿Proporciona la aplicación una interfaz de gestión de sesiones para el usuario?
- [ ] Sí — los usuarios **pueden** ver las sesiones activas (IP, dispositivo, hora) y finalizarlas de forma remota  
- [ ] Sí — los usuarios **pueden** ver las sesiones activas pero **no pueden** finalizarlas de forma remota  
- [ ] No — los usuarios **no pueden** ver ni gestionar otras sesiones activas asociadas a su cuenta  

### ¿Se notifica al usuario cuando se detecta actividad de inicio de sesión concurrente?
- [ ] Sí — **se activa** una alerta (correo electrónico, SMS o in-app) para inicios de sesión desde nuevos dispositivos o ubicaciones  
- [ ] No — **no se proporciona** ninguna notificación cuando se establecen sesiones concurrentes

---

## WSTG-INPV-01 — Cross Site Scripting (XSS) Reflejado

El Cross Site Scripting (XSS) Reflejado ocurre cuando una aplicación incluye datos no confiables en una respuesta HTTP sin una validación o codificación suficiente, lo que provoca que el Payload se ejecute en el contexto del navegador de la víctima. Los atacantes distribuyen payloads maliciosos a través de ingeniería social, típicamente mediante URLs o formularios manipulados, para secuestrar sesiones de usuario, exfiltrar cookies sensibles o realizar acciones no autorizadas en nombre del usuario. Esta vulnerabilidad se encuentra comúnmente en parámetros de búsqueda, mensajes de error y cualquier endpoint que refleje la entrada directamente de vuelta a la interfaz de usuario. La explotación depende de que la víctima interactúe con un enlace malicioso, lo que lo convierte en un vector de ataque no persistente pero altamente efectivo para dirigirse a usuarios administrativos o autenticados específicos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Estado de la Prueba** | No realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### ¿Se reflejan las entradas proporcionadas por el usuario en el cuerpo de la respuesta?
- [ ] No — la entrada **nunca** se refleja de vuelta al usuario  
- [ ] Sí — la entrada se refleja pero está codificada correctamente y el bypass **no es posible**  
- [ ] Sí — la entrada se refleja **sin** ninguna codificación o sanitización  

### ¿Se ha implementado la codificación de salida sensible al contexto?
- [ ] Sí — se **aplica** la codificación adecuada (HTML, Atributo, JavaScript) basada en el contexto específico de la reflexión  
- [ ] Sí — se **aplica** codificación pero es insuficiente para el contexto específico (por ejemplo, codificación HTML dentro de una etiqueta `<script>`)  
- [ ] No — **no se aplica** codificación  

### ¿Se pueden omitir (bypass) la validación de entrada o los filtros del Web Application Firewall (WAF)?
- [ ] No — los filtros bloquean eficazmente todos los payloads de XSS comunes y ofuscados  
- [ ] Sí — existen filtros pero el bypass **es posible** utilizando codificación de caracteres, etiquetas no estándar o polyglots  
- [ ] No — no hay filtros ni mecanismos de validación implementados  

### ¿Cuál es el contexto de ejecución de la entrada reflejada?
- [ ] Seguro — la entrada se refleja en una ubicación no ejecutable (por ejemplo, dentro de etiquetas estándar `<div>` o `<span>`)  
- [ ] Riesgoso — la entrada se refleja dentro de atributos HTML o dentro del `src`/`href` de las etiquetas  
- [ ] Crítico — la entrada se refleja directamente dentro de bloques `<script>`, manejadores de eventos o literales de plantilla (template literals)  

### ¿Existen cabeceras de seguridad de navegador modernas para mitigar el impacto de XSS?
- [ ] Sí — `Content-Security-Policy` (CSP) está **habilitado** con un script-src restrictivo  
- [ ] Sí — `Content-Security-Policy` (CSP) está **habilitado** pero contiene `unsafe-inline` o listas blancas (whitelists) débiles  
- [ ] No — no hay cabeceras CSP ni las legadas `X-XSS-Protection` presentes

---

## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

El Stored Cross Site Scripting (XSS), también conocido como Persistent XSS, ocurre cuando una aplicación recibe datos de un usuario y los almacena en una base de datos persistente, sistema de archivos u otro medio de almacenamiento sin la validación o codificación adecuada. Cuando una víctima navega posteriormente a una página que recupera y muestra estos datos no saneados, el script malicioso se ejecuta dentro del contexto de su navegador bajo el origen de la aplicación. Esta vulnerabilidad es particularmente peligrosa porque no requiere que la víctima haga clic en un enlace especialmente diseñado; cualquier usuario que visualice la página afectada se convierte en un objetivo, lo que puede conducir al secuestro de sesión (session hijacking), acciones no autorizadas o recolección de credenciales. Los atacantes suelen dirigirse a secciones de comentarios, perfiles de usuario, foros de mensajes y registros administrativos donde la entrada se renderiza de manera constante para otros usuarios o administradores.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Herramientas:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### ¿Existen puntos de entrada que almacenen la entrada del usuario para su posterior visualización por otros usuarios?
- [ ] No — la aplicación no almacena entrada controlable por el usuario para su posterior renderizado  
- [ ] Sí — la entrada se almacena (ej. perfiles, comentarios) pero **no** se renderiza en un contexto HTML  
- [ ] Sí — la entrada se almacena y se renderiza para otros usuarios o administradores  

### ¿Se aplica codificación de salida (output encoding) o sanitización cuando se renderizan los datos almacenados?
- [ ] Sí — se **aplica** una codificación de salida sensible al contexto y **no es posible** realizar un bypass *(Más seguro)*  
- [ ] Sí — se **aplica** sanitización (ej. DOMPurify) pero la evasión (bypass) **es posible** debido a errores de configuración  
- [ ] No — los datos se renderizan directamente en el DOM **sin** codificación ni sanitización *(Alta)*  

### ¿Se puede evadir la validación de entrada o la sanitización utilizando payloads alternativos?
- [ ] No — los filtros gestionan de forma segura diversas codificaciones, etiquetas no estándar y controladores de eventos  
- [ ] Sí — el bypass **es posible** utilizando codificación de entidades HTML o etiquetas específicas (ej. `<svg>`, `<img>`)  
- [ ] Sí — el bypass **es posible** a través de payloads políglotas o XSS basado en mutación (mXSS)  

### ¿Proporciona una Content Security Policy (CSP) una capa secundaria de defensa?
- [ ] Sí — se ha **habilitado** una CSP estricta que evita la ejecución de scripts inline y fuentes no autorizadas  
- [ ] Sí — se ha **habilitado** una CSP pero está mal configurada (ej. `unsafe-inline` o un `script-src` demasiado permisivo)  
- [ ] No — no se ha **habilitado** ninguna CSP para la aplicación  

### ¿Es posible lograr un "Stored XSS to RCE" u otras cadenas de alto impacto (Blind XSS)?
- [ ] No — el impacto está limitado al contexto del navegador del lado del cliente  
- [ ] Sí — el XSS almacenado **puede** ser utilizado para atacar a administradores (Blind XSS) en paneles internos  
- [ ] Sí — el XSS almacenado **puede** encadenarse con otras vulnerabilidades (ej. CSRF, exfiltración de datos sensibles)

---

## WSTG-INPV-03 — Pruebas de HTTP Verb Tampering

El HTTP Verb Tampering explota debilidades en la forma en que los servidores web y los frameworks de aplicaciones autorizan el acceso a recursos específicos basándose en el método HTTP utilizado. Los atacantes intentan omitir las restricciones de seguridad sustituyendo métodos estándar como `GET` o `POST` por alternativas como `HEAD`, `PUT`, `OPTIONS`, o incluso cadenas arbitrarias no estándar que el backend podría procesar de manera inconsistente. Esta vulnerabilidad ocurre típicamente cuando las configuraciones de seguridad —como los filtros web.xml de Java EE o las reglas de autorización de .NET— enumeran explícitamente los métodos permitidos pero no tienen en cuenta otros, o cuando se utiliza un enfoque de "denegación por método" (deny-by-method) en lugar de "denegar todo" (deny-all). Una explotación exitosa puede resultar en acceso no autorizado a funciones administrativas, modificación de datos o divulgación de información sobre la configuración interna del servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Estado de la Prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si se pueden realizar funciones administrativas o modificación de datos (PUT/DELETE) sin autenticación.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### ¿Responde la aplicación a métodos HTTP no estándar o arbitrarios?
- [ ] No — la aplicación rechaza todos los métodos excepto aquellos explícitamente requeridos  
- [ ] Sí — la aplicación responde a métodos estándar (OPTIONS, TRACE) pero **no puede** omitir la seguridad  
- [ ] Sí — la aplicación acepta cadenas arbitrarias (ej. FOO, TEST) y las procesa como solicitudes `GET` o `POST`  

### ¿Es posible omitir la autenticación/autorización cambiando el método HTTP?
- [ ] No — los controles de seguridad se aplican globalmente independientemente del verbo HTTP  
- [ ] Sí — existen controles pero la omisión **es posible** utilizando el método `HEAD`  
- [ ] Sí — los controles de seguridad **no se aplican** a métodos alternativos, permitiendo el acceso no autorizado a endpoints restringidos  

### ¿Están habilitados en el servidor métodos peligrosos como `PUT` o `DELETE`?
- [ ] No — los métodos peligrosos están **deshabilitados** o devuelven un 405 Method Not Allowed  
- [ ] Sí — los métodos están habilitados pero requieren una autenticación válida de alto privilegio  
- [ ] Sí — los métodos están **habilitados** y permiten la subida de archivos o eliminación de recursos no autorizada *(Crítico)*  

### ¿Revela el método `OPTIONS` información sensible sobre los verbos permitidos?
- [ ] No — `OPTIONS` está **deshabilitado** o devuelve una respuesta genérica  
- [ ] Sí — `OPTIONS` devuelve el encabezado `Allow` pero no expone métodos internos restringidos  
- [ ] Sí — `OPTIONS` revela métodos de uso interno o administrativos que ayudan a una mayor explotación  

### ¿Está habilitado el método `TRACE`, permitiendo potencialmente Cross-Site Tracing (XST)?
- [ ] No — los métodos `TRACE` y `TRACK` están **deshabilitados**  
- [ ] Sí — `TRACE` está **habilitado** y refleja los encabezados HTTP, incluyendo cookies o tokens sensibles

---

## WSTG-INPV-04 — Testing for HTTP Parameter Pollution

El HTTP Parameter Pollution (HPP) ocurre cuando una aplicación recibe múltiples parámetros HTTP con el mismo nombre y los procesa de manera inconsistente o insegura. Al suministrar parámetros duplicados, un atacante puede manipular la lógica interna de la aplicación, eludiendo potencialmente Web Application Firewalls (WAF), filtros de validación de entrada o mecanismos de control de acceso. Esta vulnerabilidad depende en gran medida del servidor web subyacente o del framework de la aplicación, que puede elegir el primero, el último o una versión concatenada de los parámetros repetidos. La explotación generalmente se dirige a endpoints que pasan parámetros a APIs de back-end, consultas de bases de datos o mecanismos de redirección de URL, lo que genera impactos que van desde Cross-Site Scripting (XSS) hasta la exfiltración de datos no autorizada.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### ¿Cómo maneja la aplicación múltiples parámetros HTTP con el mismo nombre?
- [ ] No — los parámetros duplicados son **rechazados** o ignorados por el servidor  
- [ ] Sí — el servidor selecciona consistentemente solo la **primera** o la **última** ocurrencia  
- [ ] Sí — el servidor **concatena** los valores, permitiendo potencialmente una inyección  

### ¿Se puede aprovechar el HPP para eludir controles de seguridad como un WAF o un filtro de entrada?
- [ ] No — los controles de seguridad inspeccionan correctamente **todas** las ocurrencias de un parámetro  
- [ ] Sí — un WAF o filtro solo inspecciona la **primera** ocurrencia, permitiendo que una **segunda** ocurrencia maliciosa pase  
- [ ] Sí — la concatenación permite a un atacante **dividir** un Payload a través de múltiples parámetros para evadir la detección  

### ¿La aplicación refleja parámetros contaminados en la respuesta sin una sanitización adecuada?
- [ ] No — los parámetros se sanitizan o codifican antes de reflejarse en la interfaz de usuario (UI)  
- [ ] Sí — la contaminación resulta en contenido reflejado, pero **se aplica** sanitización  
- [ ] Sí — la contaminación resulta en contenido reflejado y **no se aplica** sanitización, lo que deriva en XSS o errores de lógica  

### ¿Son vulnerables a la contaminación los parámetros pasados a llamadas de API internas o sistemas de back-end?
- [ ] No — las peticiones de back-end utilizan métodos de construcción seguros y no dinámicos  
- [ ] Sí — el HPP permite a un atacante **inyectar** o **sobrescribir** parámetros en la estructura de la petición del back-end  

### ¿Exhibe la aplicación un comportamiento diferente cuando los parámetros se contaminan en peticiones GET frente a POST?
- [ ] No — el comportamiento es consistente en todos los métodos HTTP  
- [ ] Sí — solo los parámetros GET son vulnerables a la contaminación  
- [ ] Sí — solo los parámetros POST (body) son vulnerables a la contaminación

---

## WSTG-INPV-05 — SQL Injection

La SQL Injection ocurre cuando la entrada suministrada por el usuario se incorpora en consultas SQL sin la debida parametrización o sanitización, lo que permite a los atacantes manipular la lógica de la consulta. Una explotación exitosa puede derivar en la exfiltración no autorizada de datos, bypass de autenticación, modificación de datos y, en algunos casos, ejecución remota de código a través de funciones de la base de datos como `xp_cmdshell` o `LOAD_FILE()`. Esta vulnerabilidad afecta comúnmente a formularios de inicio de sesión, funcionalidades de búsqueda, parámetros de REST API, cabeceras HTTP y cualquier endpoint que construya consultas SQL dinámicas a partir de entradas controladas por el usuario. Desde la perspectiva de un atacante, la identificación de un punto de inyección es la puerta de entrada principal para el compromiso total de la base de datos y el potencial movimiento lateral dentro de la infraestructura.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Herramientas:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### ¿Se procesan los parámetros controlables por el usuario mediante métodos seguros de acceso a la base de datos?
- [ ] Sí — la aplicación utiliza exclusivamente ORM o consultas parametrizadas y el bypass **no es posible**  
- [ ] Sí — se utilizan consultas parametrizadas pero el bypass **es posible** a través de casos de borde (ej., cláusulas `ORDER BY`)  
- [ ] No — se utiliza concatenación directa de cadenas para construir consultas SQL **sin** parametrización  

### ¿Puede omitirse el mecanismo de autenticación a través de SQL Injection?
- [ ] No — los parámetros de inicio de sesión **no** son vulnerables a la inyección  
- [ ] Sí — el bypass de autenticación **es posible** utilizando payloads basados en tautologías (ej., `' OR 1=1 --`)  

### ¿Es posible la exfiltración de datos mediante técnicas in-band o out-of-band?
- [ ] No — no se ha identificado exfiltración de datos  
- [ ] Sí — la exfiltración basada en UNION o en errores **es posible**  
- [ ] Sí — la exfiltración blind (basada en booleanos o tiempo) **es posible**  
- [ ] Sí — la exfiltración out-of-band (DNS/HTTP) **es posible**  

### ¿Presentan las funciones de la aplicación vulnerabilidades de SQL Injection de segundo orden?
- [ ] No — los datos almacenados se manejan de forma segura cuando se recuperan para consultas posteriores  
- [ ] Sí — la entrada del usuario se almacena y se utiliza posteriormente en una consulta SQL insegura **sin** validación  

### ¿Están los privilegios de la cuenta de servicio de la base de datos restringidos al mínimo necesario?
- [ ] Sí — la cuenta de servicio tiene el **mínimo privilegio** (ej., limitada a tablas/acciones específicas)  
- [ ] No — la cuenta de servicio tiene privilegios elevados (ej., privilegios de `DROP TABLE`, `FILE`, o `xp_cmdshell` **habilitado**)

---

## WSTG-INPV-06 — Pruebas de LDAP Injection

La LDAP Injection ocurre cuando una aplicación incorpora datos proporcionados por el usuario en filtros LDAP (Lightweight Directory Access Protocol) sin la debida sanitización o escapado, lo que permite a un atacante manipular la lógica de la consulta. Al inyectar caracteres especiales como asteriscos, paréntesis y operadores lógicos, los atacantes pueden modificar el filtro de búsqueda para evadir los mecanismos de autenticación o exfiltrar información sensible del directorio, incluyendo nombres de usuario, membresías de grupos y atributos organizacionales. Esta vulnerabilidad se manifiesta típicamente en funcionalidades como búsquedas en directorios corporativos, portales de empleados o sistemas de Single Sign-On (SSO) que interactúan con Active Directory u OpenLDAP. Desde la perspectiva de un atacante, la explotación exitosa a menudo implica técnicas ciegas (Blind) para enumerar la estructura del directorio o los valores de los atributos bit a bit cuando la salida directa de la consulta es suprimida por la aplicación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Herramientas:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### ¿Utiliza la aplicación LDAP para la autenticación o búsquedas en el directorio?
- [ ] No — LDAP **no se utiliza** en la arquitectura de la aplicación  
- [ ] Sí — LDAP se utiliza y todas las entradas están estrictamente sanitizadas o parametrizadas  
- [ ] Sí — LDAP se utiliza y la entrada del usuario se concatena en los filtros **sin** el escapado adecuado  

### ¿Se escapan o filtran adecuadamente los metacaracteres LDAP en la entrada del usuario?
- [ ] Sí — los caracteres como `(`, `)`, `&`, `|`, `*` y `\` **no están permitidos** o se escapan correctamente  
- [ ] No — los metacaracteres **pueden** ser inyectados para alterar la estructura del filtro LDAP  

### ¿Puede el mecanismo de autenticación ser evadido mediante inyección?
- [ ] No — la lógica de inicio de sesión **no es susceptible** a la manipulación de consultas  
- [ ] Sí — la autenticación **puede** ser evadida utilizando la inyección de un OR lógico (`|`) o comodines (`*`) en los campos de nombre de usuario o contraseña  

### ¿Es posible la exfiltración ciega de datos desde el servicio de directorio?
- [ ] No — los resultados de búsqueda son limitados y no se observan diferencias temporales (Timing) o booleanas  
- [ ] Sí — los atributos del directorio **pueden** ser enumerados bit a bit mediante técnicas de Blind Injection basadas en booleanos  
- [ ] Sí — los registros completos del directorio **se ven reflejados** en la respuesta de la aplicación debido a la manipulación de la consulta  

### ¿Son vulnerables los componentes secundarios de la aplicación (ej. sistemas de correo, SSO) a la inyección de atributos basada en LDAP?
- [ ] No — los atributos LDAP se manejan de forma segura en todos los componentes  
- [ ] Sí — un atacante **puede** modificar sus propios atributos (ej. correo electrónico, membresía de grupos) a través de la inyección para escalar privilegios o redireccionar comunicaciones

---

## WSTG-INPV-07 — XML Injection

La XML Injection ocurre cuando una aplicación incorpora datos proporcionados por el usuario en un documento o flujo XML sin una validación, sanitización o escape de metacaracteres XML adecuados. Al inyectar etiquetas XML específicas o elementos estructurales, un atacante puede manipular la lógica del documento, evadir mecanismos de autenticación o interferir con la integridad de los datos procesados por el backend. Esta vulnerabilidad se encuentra comúnmente en servicios web basados en SOAP, API REST que aceptan XML, aserciones SAML y aplicaciones que generan archivos de configuración XML o reportes de forma dinámica. Desde la perspectiva de un atacante, el objetivo es salir del contexto de datos previsto para alterar la estructura del documento, lo que potencialmente conduce a una escalada de privilegios no autorizada o a la ejecución de una lógica de negocio no deseada.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Estado de la Prueba** | No realizado |
| **Severidad** | Alta / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Herramientas:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### ¿La aplicación acepta y procesa entradas en formato XML provenientes del usuario?
- [ ] No — la aplicación **no** procesa entradas XML  
- [ ] Sí — las entradas XML se procesan a través de APIs (SOAP/REST)  
- [ ] Sí — el XML se procesa mediante la carga de archivos (SVG, DOCX, etc.)  

### ¿Se escapan o sanitizan adecuadamente los metacaracteres XML (ej. `<`, `>`, `&`, `'`, `"`)?
- [ ] Sí — todos los metacaracteres se escapan o sanitizan **adecuadamente** *(Más seguro)*  
- [ ] Sí — se aplica algo de filtrado, pero es **posible** realizar un bypass mediante codificación  
- [ ] No — la entrada sin procesar se incrusta directamente en las estructuras XML **sin** sanitización  

### ¿Se aplica la validación de esquema del lado del servidor (XSD/DTD) para todas las entradas XML?
- [ ] Sí — la validación estricta de esquema está **habilitada** y evita cambios estructurales  
- [ ] Sí — la validación está **habilitada**, pero **no** evita la inyección de etiquetas dentro de los campos de datos  
- [ ] No — la validación de esquema está **deshabilitada** o no se ha implementado  

### ¿Puede un atacante inyectar nuevas etiquetas XML para alterar la estructura o lógica del documento?
- [ ] No — la estructura permanece intacta independientemente de la entrada  
- [ ] Sí — se pueden inyectar etiquetas, pero **no** pueden alterar la lógica de negocio  
- [ ] Sí — la inyección de etiquetas **es posible** y permite la evasión de lógica o la modificación de datos *(Crítico)*  

### ¿Puede el analizador (parser) XML ser manipulado utilizando secciones CDATA o comentarios XML?
- [ ] No — el analizador maneja o rechaza correctamente los marcadores CDATA/comentarios  
- [ ] Sí — la inyección de `<![CDATA[...]]>` o `<!-- ... -->` **es posible** para ocultar o evadir filtros

---

## WSTG-INPV-08 — Pruebas de Inyección SSI (SSI Injection)

La Inyección de Server-Side Includes (SSI Injection) ocurre cuando una aplicación falla al desinfectar la entrada del usuario antes de incorporarla en archivos HTML procesados por el motor SSI del servidor. Los atacantes explotan esto inyectando directivas SSI, como `<!--#exec cmd="id" -->`, para ejecutar comandos de shell arbitrarios, leer archivos locales o exfiltrar variables de entorno. Esta vulnerabilidad se encuentra típicamente en aplicaciones que utilizan extensiones de archivo heredadas como `.shtml`, `.shtm` o `.stm`, o donde el servidor web está configurado explícitamente para procesar SSI dentro de archivos HTML estándar. Una explotación exitosa otorga al atacante los mismos permisos que el proceso del servidor web, lo que a menudo conduce a un compromiso total del sistema o a la exposición de datos sensibles.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### ¿El servidor web admite y procesa directivas SSI?
- [ ] No — el servidor **no** admite SSI o las extensiones de archivo como `.shtml` están **deshabilitadas**  
- [ ] Sí — SSI **está habilitado** pero la entrada del usuario **no** se refleja en las páginas procesadas  
- [ ] Sí — SSI **está habilitado** y la entrada del usuario **se** refleja en las páginas procesadas  

### ¿Se pueden ejecutar comandos de sistema arbitrarios a través de la directiva `#exec`?
- [ ] No — la directiva `#exec` está **deshabilitada** o restringida por la configuración del servidor  
- [ ] Sí — la ejecución de comandos **es posible** a través de payloads de `#exec cmd` o `#exec cgi`  

### ¿Es posible la exfiltración de archivos sensibles o variables de entorno?
- [ ] No — las directivas como `#include` o `#config` están **deshabilitadas**  
- [ ] Sí — la lectura de archivos locales (p. ej., `/etc/passwd`) **es posible** a través de `#include virtual`  
- [ ] Sí — las variables de entorno del servidor **pueden** ser exfiltradas a través de `#printenv` o `#echo`  

### ¿Son efectivos los controles de validación de entrada o WAF contra los payloads de SSI?
- [ ] Sí — se **aplica** una validación de entrada estricta y el bypass **no es posible**  
- [ ] Sí — se **aplica** validación/WAF pero el bypass **es posible** utilizando codificación de caracteres o una sintaxis SSI diferente  
- [ ] No — no existe validación ni protección WAF para los caracteres de SSI (`<`, `!`, `#`, `-`, `"`)

---

## WSTG-INPV-09 — Pruebas de XPath Injection

XPath Injection ocurre cuando una aplicación utiliza información proporcionada por el usuario para construir una consulta XPath para datos XML, lo que permite a un atacante interferir con la lógica de la consulta. Al inyectar sintaxis específica como comillas simples, corchetes y operadores lógicos, un atacante puede navegar por la estructura del documento XML, eludir la autenticación o exfiltrar nodos de datos sensibles. Esta vulnerabilidad se encuentra comúnmente en servicios web, interfaces de gestión de configuración y sistemas heredados que dependen de almacenes de datos basados en XML en lugar de bases de datos SQL tradicionales. Desde la perspectiva de un atacante, esto se explota a menudo utilizando técnicas basadas en booleanos o en errores para mapear sistemáticamente el árbol XML y recuperar valores ocultos del backend.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Herramientas:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### ¿Se sanean o parametrizan adecuadamente las entradas del usuario utilizadas para construir consultas XPath?
- [ ] Sí — las entradas **no** se utilizan en consultas XPath o están estrictamente parametrizadas  
- [ ] Sí — **se aplica** una validación/codificación de entrada robusta y el bypass **no es posible**  
- [ ] Sí — **se aplica** algo de validación pero el bypass **es posible** utilizando caracteres codificados  
- [ ] No — la entrada del usuario se concatena directamente en las consultas XPath *(Crítico)*  

### ¿Revela la aplicación detalles estructurales de XML/XPath a través de mensajes de error?
- [ ] No — se muestran páginas de error genéricas y **no** se filtran detalles de XML  
- [ ] Sí — los mensajes de error revelan la presencia de procesamiento XML pero **no** detalles de la ruta  
- [ ] Sí — los mensajes de error detallados revelan la sintaxis de XPath, nombres de nodos o rutas de archivos  

### ¿Es posible eludir la lógica de autenticación o autorización utilizando lógica de XPath?
- [ ] No — la lógica de autenticación **no** depende de XML/XPath o está implementada de forma segura  
- [ ] Sí — los controles de inicio de sesión o de permisos pueden eludirse utilizando payloads basados en lógica como `' or 1=1 or '`  

### ¿Se puede enumerar la estructura del documento XML o los datos a través de inyección ciega?
- [ ] No — la aplicación **no** es susceptible a inferencias basadas en booleanos o en tiempo  
- [ ] Sí — los nombres de los nodos y los datos **pueden** ser exfiltrados utilizando inferencia basada en booleanos  
- [ ] Sí — el recorrido completo del árbol XML y la extracción de datos **es posible**

---

## WSTG-INPV-10 — Inyección IMAP SMTP

Las vulnerabilidades de inyección IMAP y SMTP surgen cuando una aplicación web filtra incorrectamente los datos proporcionados por el usuario antes de incorporarlos en los comandos enviados a un servidor de correo. Al inyectar caracteres de Retorno de Carro y Salto de Línea (CRLF), un atacante puede finalizar el comando previsto y añadir instrucciones de correo arbitrarias, como agregar destinatarios adicionales (CC/BCC) o modificar el cuerpo del mensaje. Estos fallos se encuentran con mayor frecuencia en pasarelas de web-a-correo, formularios de contacto y clientes de correo electrónico que se comunican directamente con los servidores de correo a través de protocolos como IMAP4 o SMTP. La explotación exitosa permite el reenvío no autorizado de spam, la exfiltración de contenidos de correo electrónico sensibles y, en algunos casos, el compromiso total de la integridad del servidor de correo.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### ¿Procesa la aplicación la entrada proporcionada por el usuario para construir comandos IMAP o SMTP?
- [ ] No — la aplicación **no** interactúa con servidores de correo mediante la entrada del usuario  
- [ ] Sí — la aplicación interactúa con servidores de correo pero utiliza una API o librería segura  
- [ ] Sí — la aplicación construye comandos de correo en bruto utilizando parámetros controlados por el usuario  

### ¿Se valida la entrada para prevenir la inyección de secuencias de Retorno de Carro (`\r`) y Salto de Línea (`\n`)?
- [ ] Sí — los caracteres CRLF son filtrados, codificados o rechazados **estrictamente**  
- [ ] Sí — la entrada se valida contra una lista de permitidos (allowlist) estricta de caracteres  
- [ ] No — los caracteres CRLF **no** se filtran, lo que permite la terminación e inyección de comandos  

### ¿Puede un atacante inyectar cabeceras de correo adicionales como `Bcc:` o `Cc:`?
- [ ] No — la modificación de cabeceras **no es posible** debido a restricciones estrictas de entrada  
- [ ] Sí — la inyección de cabeceras adicionales **es posible** mediante secuencias CRLF  
- [ ] Sí — el reenvío de correo (mail relaying) a dominios externos **está habilitado** y es explotable *(Crítico)*  

### ¿Permite la aplicación la manipulación de comandos IMAP para acceder a buzones de correo no autorizados?
- [ ] No — la aplicación **no** utiliza IMAP o limita estrictamente el acceso al buzón al usuario autenticado  
- [ ] Sí — la inyección de comandos IMAP **es posible** pero se limita a la enumeración de metadatos  
- [ ] Sí — la inyección de comandos IMAP permite la lectura o eliminación de correos electrónicos de otros usuarios  

### ¿Es el servidor de correo backend vulnerable a command pipelining?
- [ ] No — el servidor de correo rechaza comandos por lotes o múltiples comandos en una sola sesión  
- [ ] Sí — el servidor de correo procesa múltiples comandos inyectados a través de un único campo de entrada

---

## WSTG-INPV-11 — Code Injection

La inyección de código (Code Injection) ocurre cuando una aplicación evalúa fragmentos de código suministrados por el usuario dentro de su entorno de ejecución, típicamente a través de funciones inseguras específicas del lenguaje como `eval()`, `exec()` o `system()`. Esta vulnerabilidad se diferencia de la inyección de comandos (Command Injection) porque se dirige al contexto de ejecución del lenguaje de programación (p. ej., PHP, Python, Node.js) en lugar de a la shell del sistema operativo subyacente. Los atacantes explotan estos puntos para ejecutar lógica arbitraria, exfiltrar variables de entorno o lograr una ejecución remota de código (RCE) completa en el servidor. Los Pentesters deben investigar funcionalidades que procesen expresiones matemáticas, plantillas del lado del servidor (Server-Side Templates) o parámetros de configuración dinámica donde la entrada pueda ser interpretada como código ejecutable.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### ¿Existen funcionalidades de la aplicación que evalúen entradas dinámicas a través de funciones de evaluación específicas del lenguaje?
- [ ] No — no existen funciones de evaluación dinámica presentes en el código base  
- [ ] Sí — se utilizan funciones como `eval()`, `exec()` o `include()`, pero **no** aceptan entradas del usuario  
- [ ] Sí — se utilizan funciones y estas procesan entradas suministradas por el usuario  

### ¿Se aplican mecanismos de validación o sanitización a las entradas antes de su evaluación?
- [ ] Sí — la entrada se valida estrictamente contra una **lista blanca (whitelist)** estática  
- [ ] Sí — la entrada se sanitiza mediante listas negras (blacklisting), pero es posible realizar un **bypass**  
- [ ] No — la entrada se pasa directamente a la función de evaluación y **no se aplican controles**  

### ¿El entorno de ejecución está en un sandbox o restringido para evitar el acceso a nivel de sistema?
- [ ] Sí — la ejecución ocurre en un sandbox altamente restringido donde **no** se pueden realizar llamadas al sistema (system calls)  
- [ ] Sí — existen algunas restricciones, pero es posible realizar un **escape del sandbox**  
- [ ] No — el entorno permite acceso total a las APIs del lenguaje subyacente y a las funciones del SO *(Crítica)*  

### ¿Puede utilizarse la inyección de código para exfiltrar información sensible?
- [ ] No — la ejecución es ciega (blind) y **no** es posible la comunicación fuera de banda (out-of-band)  
- [ ] Sí — las variables de entorno o los archivos locales **pueden** ser exfiltrados a través de peticiones HTTP/DNS  
- [ ] Sí — los resultados del código ejecutado se **reflejan directamente** en la respuesta de la aplicación  

### ¿Permite la aplicación la inclusión remota de archivos o la carga dinámica de librerías?
- [ ] No — solo se pueden ejecutar rutas de código locales y predefinidas  
- [ ] Sí — la aplicación **puede** ser forzada a cargar y ejecutar código desde una fuente remota o controlada por el atacante

---

## WSTG-INPV-12 — Inyección de Comandos (Command Injection)

La inyección de comandos (Command Injection) ocurre cuando una aplicación pasa datos no seguros suministrados por el usuario, como entradas de formularios, encabezados HTTP o cookies, a un shell del sistema. Esta vulnerabilidad permite a un atacante ejecutar comandos arbitrarios del sistema operativo en el servidor, generalmente con los privilegios del proceso de la aplicación web. Desde la perspectiva de un atacante, esto se explota mediante el uso de metacaracteres de shell como puntos y coma, tuberías o comillas invertidas para encadenar comandos maliciosos al flujo de ejecución previsto. El impacto es frecuentemente catastrófico, lo que lleva al compromiso total del sistema, la filtración de datos no autorizada o el movimiento lateral dentro de la red interna.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Herramientas:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### ¿La aplicación invoca comandos a nivel de sistema o ejecutables de shell?
- [ ] No — la aplicación **no** interactúa con el shell del sistema operativo  
- [ ] Sí — la aplicación utiliza APIs internas o librerías de alto nivel para tareas del sistema  
- [ ] Sí — la aplicación invoca directamente comandos de shell a través de llamadas al sistema como `system()`, `exec()` o `popen()`  

### ¿Se valida o sanea la entrada del usuario antes de ser pasada a las llamadas al sistema?
- [ ] Sí — **se aplica** una validación de entrada y listas de permitidos (allow-listing) estrictas  
- [ ] Sí — se utiliza el saneamiento o el escape (escaping) pero el bypass **es posible**  
- [ ] No — la entrada del usuario se pasa directamente al shell **sin** validación  

### ¿Se pueden utilizar metacaracteres de shell para manipular el flujo de ejecución de comandos?
- [ ] No — los metacaracteres se filtran o escapan correctamente  
- [ ] Sí — la ejecución in-band, donde la salida del comando se refleja en la respuesta, **es posible**  
- [ ] Sí — la ejecución ciega (blind), donde no se refleja ninguna salida, **es posible**  

### ¿Es posible la exfiltración fuera de banda (OOB) desde el host?
- [ ] No — el servidor no tiene salida (egress) o las señales OOB (DNS/HTTP) fallan  
- [ ] Sí — la exfiltración de datos a través de señales OOB basadas en DNS o HTTP **es posible**  

### ¿El proceso de la aplicación se ejecuta con privilegios de sistema elevados?
- [ ] No — la aplicación se ejecuta con una cuenta de servicio dedicada de bajos privilegios  
- [ ] Sí — la aplicación se ejecuta como `root`, `Administrator` o un usuario de altos privilegios *(Crítica)*

---

## WSTG-INPV-13 — Format String Injection

La inyección de cadenas de formato (Format String Injection) ocurre cuando una aplicación web pasa una entrada controlada por el usuario directamente al argumento de cadena de formato de una función variádica, como la familia `printf` de C o envoltorios (wrappers) de registro de bajo nivel. Aunque es menos común en los frameworks web modernos de código administrado, esta vulnerabilidad sigue siendo crítica en aplicaciones CGI heredadas (legacy), extensiones nativas o sistemas de registro en casos de borde que interactúan con librerías de sistema de bajo nivel. Los atacantes explotan (exploit) esto proporcionando especificadores de formato como `%x` o `%p` para filtrar direcciones de memoria sensibles y datos de la pila (stack), o el especificador `%n` para realizar escrituras arbitrarias en memoria. Una explotación exitosa generalmente resulta en el compromiso completo del sistema a través de la ejecución remota de código (Remote Code Execution - RCE) o, como mínimo, una condición de denegación de servicio (Denial-of-Service - DoS) impactante mediante la caída de procesos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Estado de la Prueba** | No Realizado |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### ¿Procesa la aplicación entradas de usuario a través de código nativo o interfaces CGI heredadas?
- [ ] No — la aplicación está construida íntegramente en frameworks de código administrado (p. ej., JVM, CLR)  
- [ ] Sí — la aplicación utiliza JNI, extensiones nativas C/C++ o CGIs binarios heredados donde **pueden** existir vulnerabilidades de Format String Injection  

### ¿Se pasan las entradas proporcionadas por el usuario como el argumento principal de funciones sensibles al formato?
- [ ] No — las entradas se pasan como argumentos de datos, y las cadenas de formato son **estáticas** y están **hardcoded**  
- [ ] Sí — la entrada del usuario se concatena directamente en el argumento de la cadena de formato, pero **se aplica** sanitización  
- [ ] Sí — la entrada del usuario se utiliza directamente como cadena de formato y **no se aplica** sanitización  

### ¿Es posible filtrar contenidos de la memoria utilizando especificadores de formato?
- [ ] No — la entrada es validada y los especificadores como `%p`, `%x` o `%s` **no son procesados**  
- [ ] Sí — el envío de especificadores `%p` o `%x` resulta en el reflejo de direcciones de memoria del stack o heap  
- [ ] Sí — los datos sensibles (p. ej., punteros, stack cookies) **pueden** ser exfiltrados a través de especificadores de formato  

### ¿Se puede provocar la caída de la aplicación o manipularla utilizando el especificador `%n`?
- [ ] No — los especificadores que permiten escrituras en memoria están **deshabilitados** o bloqueados por el compilador/entorno  
- [ ] Sí — la aplicación se cae (DoS) cuando se proporcionan `%s%s%s%s` o cadenas similares  
- [ ] Sí — las escrituras arbitrarias en memoria a través de `%n` son **posibles**, lo que potencialmente conduce a una Ejecución Remota de Código (RCE)  

### ¿Son eludibles las protecciones binarias modernas (ASLR, DEP, Stack Canaries) a través de esta inyección?
- [ ] No — el entorno está endurecido (hardened) y la fuga de memoria **no puede** utilizarse para eludir las protecciones  
- [ ] Sí — la inyección de cadenas de formato **puede** utilizarse para filtrar direcciones base, haciendo que la elusión de ASLR/DEP sea **posible**

---

## WSTG-INPV-14 — Pruebas de Vulnerabilidades Incubadas

Las vulnerabilidades incubadas, también conocidas como vulnerabilidades persistentes o de segundo orden (second-order vulnerabilities), ocurren cuando la aplicación almacena un payload malicioso en un estado "frío" (cold state) y luego este es ejecutado o procesado en un contexto diferente. Este patrón de ataque generalmente se dirige a sistemas back-end, consolas administrativas o herramientas de reportes automatizados que recuperan datos de una base de datos o sistema de archivos compartidos sin una re-validación adecuada o codificación de salida (output encoding). Los Pentesters deben rastrear el ciclo de vida de la entrada del usuario desde el punto de entrada (el "incubador") hasta cada ubicación donde esos datos son eventualmente renderizados o utilizados por otros componentes o aplicaciones secundarias. Una explotación exitosa a menudo conduce a resultados de alto impacto, como el secuestro de sesiones administrativas (session hijacking) a través de Stored XSS o la exfiltración de datos mediante SQL Injection de segundo orden en módulos de reportes internos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Herramientas:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### ¿Existen endpoints donde se almacena la entrada controlada por el usuario para su posterior recuperación por otros usuarios o procesos?
- [ ] No — la aplicación **no** almacena la entrada del usuario para su uso posterior  
- [ ] Sí — la entrada **se** almacena en campos de la base de datos, archivos de registro (logs) o ajustes de configuración  

### ¿Se aplica validación o sanitización de la entrada antes de que los datos se escriban en la capa de almacenamiento persistente?
- [ ] Sí — se **aplica** una validación estricta de lista blanca (allow-list) en el punto de entrada  
- [ ] Sí — se **aplica** una sanitización genérica, pero el bypass **es posible** a través de codificación o caracteres no estándar  
- [ ] No — la entrada se almacena en su forma original, sin validar (raw)  

### ¿Los datos almacenados se codifican o sanitizan adecuadamente cuando se renderizan en interfaces administrativas o secundarias?
- [ ] Sí — se **aplica** codificación de salida sensible al contexto (context-aware output encoding) en todos los lugares donde se renderizan los datos  
- [ ] Sí — la codificación **se aplica** en algunas ubicaciones pero **falta** en otras (p. ej., panel de administración interno)  
- [ ] No — los datos se renderizan sin ninguna codificación o sanitización, permitiendo la ejecución del payload  

### ¿Pueden los payloads almacenados en la base de datos afectar los procesos por lotes (batch) del back-end o los sistemas de monitoreo fuera de banda (out-of-band)?
- [ ] No — los procesos de back-end utilizan métodos de análisis (parsing) seguros o consultas parametrizadas (parameterized queries)  
- [ ] Sí — los payloads almacenados **pueden** desencadenar interacciones en la lógica del back-end (p. ej., CSV Injection, Command Injection en logs)  
- [ ] Sí — los payloads almacenados **pueden** desencadenar solicitudes fuera de banda (OOB) hacia servidores controlados por el atacante

---

## WSTG-INPV-15 — HTTP Splitting Smuggling

Las vulnerabilidades de HTTP Splitting y Smuggling surgen de discrepancias en la forma en que los proxies de frontend y los servidores de backend interpretan y procesan los límites de las solicitudes HTTP, específicamente en relación con las cabeceras `Content-Length` y `Transfer-Encoding`. Al crear solicitudes ambiguas, un atacante puede realizar un "smuggling" de una solicitud oculta hacia el backend o inyectar secuencias CRLF para dividir una respuesta, lo que conduce al envenenamiento de caché (Cache Poisoning), secuestro de solicitudes (Request Hijacking) o la evasión de controles de seguridad. Estos fallos se manifiestan típicamente en entornos complejos que utilizan proxies inversos (Reverse Proxies), balanceadores de carga o CDNs que poseen una lógica de análisis (parsing) inconsistente. Desde la perspectiva de un atacante, esto permite la redirección del tráfico de los usuarios, el robo de tokens de sesión sensibles y la ejecución de acciones no autorizadas en el contexto de las sesiones de otros usuarios.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Herramientas:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### ¿Es el entorno susceptible al Request Smuggling a través de discrepancias CL.TE o TE.CL?
- [ ] No — los servidores de frontend y backend gestionan los límites de las solicitudes de manera consistente  
- [ ] Sí — existen discrepancias pero la explotación **no es posible** debido a mitigaciones en la infraestructura  
- [ ] Sí — el smuggling CL.TE o TE.CL **es posible**, permitiendo que solicitudes ocultas alcancen el backend *(Alta)*  
- [ ] Sí — el TE.TE (doble codificación/ofuscación) **es posible** para evadir los filtros de frontend *(Crítica)*  

### ¿Se puede lograr el HTTP Response Splitting a través de la inyección de CRLF en las cabeceras?
- [ ] No — la aplicación sanea adecuadamente las secuencias CRLF en todas las entradas de cabeceras  
- [ ] Sí — las secuencias CRLF se reflejan en las cabeceras pero la división de respuestas (Response Splitting) **no es posible**  
- [ ] Sí — la inyección de CRLF **es posible**, permitiendo la inyección de cabeceras o el envenenamiento de caché  

### ¿Se evaden los controles de seguridad (WAF/ACLs) mediante el uso de solicitudes "smuggled"?
- [ ] No — los controles de seguridad se aplican tanto a la solicitud externa como a la solicitud oculta (smuggled)  
- [ ] Sí — las solicitudes ocultas **pueden** evadir las reglas de WAF de frontend o las ACLs basadas en IP  

### ¿Es posible secuestrar las sesiones de otros usuarios o redireccionar su tráfico?
- [ ] No — los flujos de solicitud/respuesta están aislados y no pueden cruzarse  
- [ ] Sí — el Request Smuggling **es posible** y permite capturar las solicitudes de otros usuarios *(Crítica)*  
- [ ] Sí — el Response Splitting **es posible** y permite el envenenamiento de caché en el lado del navegador o Cross-Site Scripting (XSS)

---

## WSTG-INPV-16 — HTTP Request Smuggling

El HTTP Request Smuggling (HRS) ocurre cuando una cadena de servidores HTTP (como un balanceador de carga y un servidor web back-end) interpreta los límites de una petición de manera diferente, generalmente debido a conflictos en las cabeceras `Content-Length` y `Transfer-Encoding`. Un atacante explota esta desincronización para "introducir" (smuggle) una entrada en el búfer de peticiones del servidor back-end, prefijando efectivamente la siguiente petición de un usuario legítimo con un segmento controlado por el atacante. Esta técnica permite ataques graves, incluyendo el secuestro de sesión (Session Hijacking), la elusión de controles de seguridad y el envenenamiento de caché (Cache Poisoning). La explotación se realiza habitualmente mediante un análisis basado en tiempos (timing-based analysis) o mediante la observación de respuestas inesperadas del back-end cuando se envían peticiones posteriores al servidor.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Crítica / Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Herramientas:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### ¿Manejan los servidores front-end y back-end la cabecera `Transfer-Encoding: chunked` de manera consistente?
- [ ] Sí — ambos servidores manejan el chunked encoding de forma idéntica y la desincronización **no es posible**  
- [ ] No — los servidores interpretan las cabeceras de forma diferente pero **se aplica** sanitización/normalización  
- [ ] No — la desincronización CL.TE (Content-Length/Transfer-Encoding) **es posible** *(Crítica)*  
- [ ] No — la desincronización TE.CL (Transfer-Encoding/Content-Length) **es posible** *(Crítica)*  

### ¿Puede el atacante envenenar la caché del servidor o del lado del cliente utilizando una petición filtrada (smuggled request)?
- [ ] No — el almacenamiento en caché está **deshabilitado** o no es susceptible a envenenamiento  
- [ ] Sí — las peticiones filtradas **pueden** redireccionar a los usuarios a dominios maliciosos o inyectar scripts a través de Cache Poisoning  

### ¿Es posible capturar peticiones o tokens de sesión de otros usuarios mediante la concatenación de peticiones?
- [ ] No — el smuggling **no** permite la exposición de datos entre usuarios  
- [ ] Sí — el smuggling permite concatenar la petición del siguiente usuario a un cuerpo `POST` controlado por el atacante, haciendo que la exfiltración **sea posible**  

### ¿Utiliza la infraestructura HTTP/2 y es susceptible a la desincronización H2.CL o H2.TE?
- [ ] No — la aplicación utiliza solo HTTP/1.1 o HTTP/2 está implementado de forma segura sin degradación (downgrade)  
- [ ] Sí — se producen degradaciones de HTTP/2 a HTTP/1.1 en texto claro y la desincronización **es posible**  

### ¿Se manejan de forma segura las cabeceras malformadas (p. ej., `Transfer-Encoding:  chunked` con un espacio)?
- [ ] Sí — las cabeceras malformadas se rechazan o se normalizan en todas las capas  
- [ ] No — la desincronización TE.TE **es posible** debido a un manejo inconsistente de cabeceras malformadas u ocultas

---

## WSTG-INPV-17 — Testing for Host Header Injection

La inyección de cabecera Host (Host Header Injection) ocurre cuando una aplicación confía en la cabecera `Host` proporcionada por el usuario e la incorpora en la lógica del lado del servidor, como en la generación de enlaces o redirecciones, sin una validación suficiente. Los atacantes manipulan esta cabecera para facilitar el envenenamiento de caché web (Web Cache Poisoning), el envenenamiento del restablecimiento de contraseña (Password Reset Poisoning) mediante la redirección de tokens sensibles a un dominio externo, o para evadir controles de seguridad que dependen del enrutamiento basado en cabeceras. Una explotación exitosa permite a un atacante exfiltrar datos de sesión, realizar tomas de control de cuentas (Account Takeover) o redireccionar a usuarios desprevenidos hacia una infraestructura maliciosa. Esta vulnerabilidad se encuentra comúnmente en entornos con balanceo de carga o en aplicaciones que generan dinámicamente URLs absolutas basadas en el contexto de la solicitud entrante.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad aumenta a Alta si la cabecera Host se utiliza para generar enlaces sensibles en correos electrónicos, como en el caso del restablecimiento de contraseñas.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Herramientas:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### ¿Refleja la aplicación el valor de la cabecera `Host` en enlaces, redirecciones o scripts?
- [ ] No — la aplicación utiliza URLs base estáticas (hardcoded) o una configuración estrictamente validada.
- [ ] Sí — la cabecera `Host` se refleja pero se valida estrictamente contra una lista blanca (Whitelist).
- [ ] Sí — la cabecera `Host` se refleja y es posible realizar un Exploit o bypass mediante valores malformados (ej. `expected.com:attacker.com`).
- [ ] Sí — la cabecera `Host` se refleja **sin** ningún tipo de validación.

### ¿Se procesan cabeceras alternativas relacionadas con el host por parte de la aplicación o la infraestructura?
- [ ] No — las cabeceras como `X-Forwarded-Host`, `X-Host` o `Forwarded` son ignoradas.
- [ ] Sí — se procesan cabeceras alternativas pero **no** anulan la lógica interna.
- [ ] Sí — las cabeceras alternativas **pueden** utilizarse para anular el valor de la cabecera `Host` primaria.

### ¿Puede manipularse la cabecera `Host` para envenenar correos electrónicos generados por el sistema (ej. restablecimiento de contraseña)?
- [ ] No — los enlaces de correo electrónico utilizan un dominio estático y confiable de la configuración del servidor.
- [ ] Sí — la cabecera `Host` influye en los enlaces de correo electrónico pero la validación **no** puede ser evadida.
- [ ] Sí — la cabecera `Host` **puede** manipularse para redireccionar tokens de restablecimiento de contraseña a un dominio controlado por el atacante *(Crítico)*.

### ¿Es la aplicación susceptible a Web Cache Poisoning a través de la cabecera `Host`?
- [ ] No — no hay un mecanismo de caché presente o la cabecera `Host` es parte de la clave de caché (Cache Key).
- [ ] Sí — existe una caché pero valida estrictamente o ignora la cabecera `Host` para entradas sin clave (unkeyed inputs).
- [ ] Sí — existe una caché y **puede** ser envenenada mediante una cabecera `Host` inyectada para servir contenido malicioso a otros usuarios.

### ¿Acepta el servidor cabeceras `Host` arbitrarias para el enrutamiento de hosts virtuales (Virtual Host)?
- [ ] No — el servidor devuelve un error o una página por defecto para cabeceras `Host` no reconocidas.
- [ ] Sí — el servidor acepta cualquier cabecera `Host`, lo cual **es** útil para el reconocimiento o la enumeración de servicios internos.

---

## WSTG-INPV-18 — Testing for Server-Side Template Injection (SSTI)

La Server-Side Template Injection (SSTI) ocurre cuando una aplicación incrusta entrada controlada por el usuario en un motor de plantillas del lado del servidor sin el saneamiento o escape de caracteres suficiente. Esto permite a un atacante inyectar directivas de plantilla maliciosas, que luego se ejecutan en el servidor, lo que potencialmente conduce al compromiso total del sistema a través de Remote Code Execution (RCE). La SSTI típicamente se manifiesta en funcionalidades como la generación dinámica de correos electrónicos, páginas de perfil y sistemas de notificación que utilizan motores como Jinja2, Twig o FreeMarker. Desde la perspectiva de un atacante, esta vulnerabilidad a menudo se explota para filtrar variables de entorno sensibles, leer archivos internos o ejecutar comandos del sistema aprovechando los objetos y métodos nativos del motor de plantillas.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Estado de la Prueba** | No realizada |
| **Severidad** | Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Herramientas:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### ¿Utiliza la aplicación un motor de plantillas del lado del servidor para procesar la entrada del usuario?
- [ ] No — la aplicación **no** utiliza plantillas del lado del servidor para contenido dinámico  
- [ ] Sí — se utilizan plantillas, pero la entrada del usuario **no** está incrustada en las directivas de la plantilla  
- [ ] Sí — la entrada del usuario está incrustada en las plantillas **sin** el saneamiento adecuado  

### ¿Se evalúan expresiones aritméticas básicas cuando se inyectan en los campos de entrada?
- [ ] No — los payloads como `{{7*7}}` o `${7*7}` se reflejan como cadenas literales  
- [ ] Sí — las expresiones se evalúan (p. ej., devolviendo `49`), lo que confirma que el motor de plantillas **es vulnerable**  

### ¿Se puede acceder a los objetos o métodos integrados del motor de plantillas?
- [ ] No — el acceso a objetos, métodos o configuraciones peligrosas está **restringido** o bajo un sandbox  
- [ ] Sí — se **puede** acceder a los objetos integrados (p. ej., `self`, `config`, `__class__`) para filtrar datos sensibles  
- [ ] Sí — el Remote Code Execution (RCE) mediante llamadas al sistema o librerías del SO **es posible**  

### ¿Existe un mecanismo de sandbox o lista de permitidos (allowlist) que impida la ejecución de directivas peligrosas?
- [ ] Sí — se ha implementado una lista de permitidos estricta o un sandbox seguro y el bypass **no es posible**  
- [ ] Sí — existe un sandbox, pero el bypass **es posible** mediante payloads específicos dependientes del motor  
- [ ] No — **no se aplica** ningún sandbox ni lista de permitidos al motor de plantillas

---

## WSTG-INPV-19 — Pruebas de Server-Side Request Forgery (SSRF)

La vulnerabilidad Server-Side Request Forgery (SSRF) ocurre cuando un atacante puede influir en una aplicación web para que realice solicitudes no autorizadas desde el servidor hacia recursos internos o externos. Al manipular parámetros que esperan URLs, hostnames o direcciones IP, un atacante puede evadir controles a nivel de red como firewalls y ACLs para sondear segmentos de red internos, acceder a servicios de metadatos en la nube (IMDS) o interactuar con APIs y bases de datos internas vulnerables. Esta vulnerabilidad se manifiesta típicamente en funcionalidades que involucran la obtención de imágenes, generación de PDF, webhooks o carga de archivos mediante URL. Desde la perspectiva de un atacante, el SSRF es una primitiva potente utilizada para pivotar hacia entornos aislados, exfiltrar datos de configuración sensibles o, potencialmente, lograr la ejecución remota de comandos mediante la interacción con servicios internos como Redis o Memcached.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Herramientas:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### ¿Procesa la aplicación URLs o hostnames proporcionados por el usuario?
- [ ] No — ninguna funcionalidad acepta URLs o hostnames externos  
- [ ] Sí — la funcionalidad existe pero la entrada **se valida estrictamente** contra una lista de permitidos (allow-list)  
- [ ] Sí — la funcionalidad existe y acepta URLs proporcionadas por el usuario  

### ¿Se aplican controles de validación o listas de denegación (deny-lists) al destino solicitado?
- [ ] Sí — se aplica una **allow-list** estricta de dominios/IPs de confianza  
- [ ] Sí — se utiliza una **deny-list** (p. ej., bloqueando `127.0.0.1`, `169.254.169.254`)  
- [ ] No — **no se aplica validación** ni filtrado a la entrada  

### ¿Es posible evadir los filtros mediante encoding, redirecciones o DNS rebinding?
- [ ] No — los filtros son robustos y manejan las redirecciones/resolución DNS de forma segura  
- [ ] Sí — la evasión **es posible** utilizando URL encoding o formatos de IP alternativos (hexadecimal, octal)  
- [ ] Sí — la evasión **es posible** mediante redirecciones HTTP 3xx hacia objetivos internos  
- [ ] Sí — la evasión **es posible** mediante ataques de DNS rebinding para resolver hacia IPs internas  

### ¿Puede la aplicación alcanzar servicios de metadatos internos o interfaces locales?
- [ ] No — la red local y los endpoints de metadatos en la nube **no son alcanzables**  
- [ ] Sí — el acceso a localhost (`127.0.0.1`) o servicios de loopback **es posible**  
- [ ] Sí — el acceso a servicios de metadatos en la nube (p. ej., AWS/GCP/Azure IMDS) **es posible** *(Crítico)*  

### ¿Cuál es la naturaleza de la respuesta de SSRF?
- [ ] Blind — no se devuelve ninguna respuesta ni metadatos al usuario  
- [ ] Parcial — se devuelven metadatos de la respuesta (cabeceras, tamaño, tiempos) al usuario  
- [ ] Full — el cuerpo completo de la respuesta del recurso interno **se refleja** al usuario

---

## WSTG-INPV-20 — Mass Assignment

Mass Assignment, también conocido como Overposting o Insecure Deserialization de parámetros, ocurre cuando una aplicación web vincula automáticamente los parámetros de una solicitud HTTP entrante con las propiedades de objetos internos sin un filtrado de atributos adecuado. Esta vulnerabilidad es prevalente en los frameworks MVC modernos y APIs RESTful donde los datos en formato JSON, XML o codificados en formularios se deserializan directamente en los modelos de datos del lado de la aplicación. Los atacantes explotan este comportamiento inyectando parámetros adicionales no listados en una solicitud —tales como `isAdmin`, `role` o `account_balance`— para modificar campos sensibles que los desarrolladores no pretendían exponer al control del usuario. Una explotación exitosa generalmente resulta en una escalada de privilegios, modificación no autorizada de datos o el bypass de flujos críticos de lógica de negocio.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Media* |

> *La severidad aumenta a Alta si se pueden modificar atributos administrativos, niveles de permisos o campos de facturación.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### ¿Utiliza la aplicación la vinculación automática para los parámetros de las solicitudes entrantes?
- [ ] No — los parámetros se mapean manualmente a variables específicas u objetos seguros  
- [ ] Sí — se utiliza la vinculación automática pero se aplican estrictas **listas blancas** (white-lists) o DTOs (Data Transfer Objects)  
- [ ] Sí — se utiliza la vinculación automática y los atributos internos sensibles **pueden** ser modificados  

### ¿Es posible inyectar con éxito atributos administrativos o relacionados con permisos?
- [ ] No — los parámetros inyectados son ignorados, eliminados o provocan un error de validación  
- [ ] Sí — se aceptan parámetros adicionales pero **no** afectan al estado del objeto en el backend  
- [ ] Sí — la inyección de parámetros como `is_admin`, `role` o `permissions` escala privilegios **exitosamente** *(Crítico)*  

### ¿Es posible modificar la lógica de negocio sensible o campos financieros mediante overposting?
- [ ] No — los campos como `price`, `balance` o `status` están protegidos contra Mass Assignment  
- [ ] Sí — los atributos sensibles del negocio **pueden** ser modificados al añadirlos al cuerpo de la solicitud JSON o del formulario  

### ¿Revela la aplicación estructuras de objetos internos que faciliten la adivinación de parámetros?
- [ ] No — las respuestas solo incluyen los campos públicos previstos y la documentación está restringida  
- [ ] Sí — los campos de los objetos internos son visibles en las respuestas GET, lo que hace **posible** el descubrimiento de parámetros  
- [ ] Sí — la documentación de la API (Swagger/OpenAPI) enumera explícitamente propiedades internas que **pueden** ser asignadas

---

## WSTG-ERRH-01 — Pruebas de Manejo Inadecuado de Errores

El manejo inadecuado de errores ocurre cuando una aplicación web revela detalles técnicos sensibles —como stack traces, información del esquema de la base de datos o rutas de archivos internos— a través de sus respuestas de error. Los atacantes provocan estos errores enviando malformed input, accediendo a recursos inexistentes o induciendo server-side exceptions para mapear la arquitectura subyacente de la aplicación e identificar posibles vectores para una explotación posterior. Esta filtración de información a menudo sirve como precursor de ataques más graves, incluidos SQL Injection o Path Traversal, al proporcionar al atacante especificaciones precisas del entorno. Una implementación segura debe proporcionar mensajes genéricos y amigables para el usuario, capturando diagnósticos detallados únicamente en registros (logs) seguros del lado del servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### ¿Utiliza la aplicación un manejador de errores genérico y global para las excepciones no controladas?
- [ ] Sí — las páginas de error genéricas están **habilitadas** y **no** revelan detalles técnicos  
- [ ] Sí — se utilizan páginas de error personalizadas pero la filtración **es posible** a través de cabeceras (headers) específicas  
- [ ] No — las páginas de error por defecto del servidor (p. ej., Tomcat, IIS, Nginx) son **visibles**  

### ¿Se puede enumerar información técnica a través de malformed input o casos de borde (boundary cases)?
- [ ] No — los errores se manejan de manera segura con IDs de referencia únicos para el registro (logging)  
- [ ] Sí — los stack traces o las consultas a la base de datos **se revelan** en las respuestas  
- [ ] Sí — las rutas de archivos internos, variables de entorno o cadenas de versión del servidor **se revelan**  

### ¿Están las respuestas de la API filtrando objetos de error detallados (verbose) o información de depuración (debug)?
- [ ] No — las APIs devuelven códigos de error estandarizados y mensajes JSON saneados  
- [ ] Sí — las APIs devuelven objetos de depuración detallados o detalles completos de la excepción en el cuerpo de la respuesta  
- [ ] Sí — los stack traces se incluyen en los campos `details` o `exception` de la respuesta JSON  

### ¿Se comporta la aplicación de manera diferente (Time-based o Content-based) cuando ocurren errores?
- [ ] No — las firmas de respuesta (signatures) y los tiempos son consistentes independientemente del tipo de error  
- [ ] Sí — las respuestas diferenciales **pueden** usarse para enumerar estados válidos frente a inválidos (p. ej., User Enumeration)

---

## WSTG-ERRH-02 — Pruebas de Stack Traces

Las stack traces se generan cuando una aplicación no logra manejar una excepción de manera controlada, lo que resulta en la divulgación del estado de ejecución interno y la jerarquía de llamadas al usuario final. Para un consultor de pruebas de penetración (penetration tester), estas trazas son invaluables, ya que revelan el framework de la aplicación, versiones de librerías, rutas internas del sistema de archivos y el flujo lógico, lo que reduce significativamente el esfuerzo requerido para el reconocimiento (reconnaissance). La explotación implica provocar errores de manera intencionada mediante entradas malformadas, tipos de datos inesperados o violaciones de condiciones de contorno para forzar a la aplicación a un estado inestable y exfiltrar información sobre el stack tecnológico subyacente. Esta filtración a menudo proporciona el contexto necesario para identificar componentes de terceros vulnerables o para diseñar exploits específicos para fallos de lógica descubiertos dentro de las rutas de código divulgadas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Estado de la Prueba** | No realizada |
| **Severidad** | Baja / Media* |

> *La severidad aumenta a Media si la stack trace revela detalles arquitectónicos sensibles, consultas a bases de datos o parámetros de configuración interna.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### ¿Se devuelven stack traces completas al cliente ante errores de la aplicación?
- [ ] No — la aplicación devuelve páginas de error genéricas o mensajes de error personalizados  
- [ ] Sí — las stack traces están **habilitadas** pero solo para módulos específicos no sensibles  
- [ ] Sí — las stack traces completas están **habilitadas** y son visibles en el cuerpo de la respuesta HTTP  

### ¿Los mensajes de error divulgan información sensible del entorno?
- [ ] No — los mensajes de error no contienen detalles técnicos ni del entorno  
- [ ] Sí — los mensajes revelan **rutas de archivos internas** o **estructuras de directorios** del lado del servidor  
- [ ] Sí — los mensajes revelan **esquemas de base de datos**, **SQL queries** o **versiones de librerías de terceros**  

### ¿Se pueden provocar stack traces mediante la manipulación de parámetros de entrada?
- [ ] No — la entrada malformada se maneja de forma controlada o es rechazada por la validación  
- [ ] Sí — las trazas pueden provocarse mediante **tipos de datos inesperados** (p. ej., enviando un array donde se espera un string)  
- [ ] Sí — las trazas se provocan mediante **null bytes**, **caracteres especiales** o **valores límite** (boundary values)  

### ¿Se aplica de manera consistente un manejador de errores global en toda la aplicación?
- [ ] Sí — se aplica consistentemente un manejador de excepciones global en todos los endpoints probados  
- [ ] Sí — existe un manejador pero **no se aplica** a endpoints legados, API o recién añadidos  
- [ ] No — no se detecta ningún mecanismo global de manejo de errores, lo que resulta en errores por defecto del servidor

---

## WSTG-CRYP-01 — Pruebas de Seguridad Débil en la Capa de Transporte

Las pruebas de seguridad débil en la capa de transporte implican el análisis de la implementación y configuración de SSL/TLS para garantizar la confidencialidad e integridad de los datos durante el tránsito. Los atacantes aprovechan configuraciones incorrectas como protocolos obsoletos (SSLv2, TLS 1.0), suites de cifrado (cipher suites) débiles (NULL, RC4 o anónimas) y certificados inválidos o con firmas débiles para realizar ataques de Man-in-the-Middle (MitM). Esta prueba se dirige típicamente a los endpoints del servidor web o del balanceador de carga de la aplicación donde se establece la comunicación cifrada. Una explotación exitosa permite a un adversario interceptar datos sensibles, secuestrar sesiones de usuario o degradar las conexiones a estados inseguros mediante el Downgrade de protocolos o el descifrado de tráfico.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Media* |

> *La severidad es Alta si se transmiten datos sensibles (PII, credenciales, tokens de sesión); Media para tráfico de información general.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### ¿Se admiten protocolos TLS/SSL obsoletos o inseguros?
- [ ] No — solo TLS 1.2 y TLS 1.3 están **habilitados**  
- [ ] Sí — TLS 1.0 o TLS 1.1 están **habilitados**  
- [ ] Sí — SSLv2 o SSLv3 están **habilitados** *(Crítico)*  

### ¿El servidor acepta suites de cifrado (cipher suites) débiles o inseguras?
- [ ] No — solo se **soportan** cifrados fuertes y modernos (ej., AES-GCM, ChaCha20)  
- [ ] Sí — se **soportan** cifrados con encriptación débil (ej., 3DES, RC4) o longitud de bits baja  
- [ ] Sí — se **soportan** cifrados NULL, anónimos (ADH) o de exportación *(Crítico)*  

### ¿Es el certificado del servidor válido y confiable?
- [ ] Sí — el certificado es válido, coincide con el dominio y está firmado por una **CA de confianza**  
- [ ] No — el certificado está **expirado** o utiliza un **algoritmo de firma débil** (ej., SHA-1)  
- [ ] No — el certificado es **autofirmado** (self-signed) o existe una **discrepancia** (mismatch) en el nombre de dominio  

### ¿El servidor soporta Renegociación Segura y previene ataques de Downgrade?
- [ ] Sí — la Renegociación Segura (Secure Renegotiation) está **soportada** y TLS Fallback SCSV está **habilitado**  
- [ ] No — la Renegociación Segura **no está soportada**, lo que permite potencialmente una inyección MitM  
- [ ] No — el servidor es vulnerable a ataques **POODLE** o **ROBOT**  

### ¿Está HTTP Strict Transport Security (HSTS) correctamente implementado?

| Cabecera | Presente | Ausente |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Sí — la cabecera HSTS está presente con un `max-age` largo e `includeSubDomains` **habilitado**  
- [ ] Sí — la cabecera HSTS está presente pero falta `includeSubDomains` o tiene un **max-age corto**  
- [ ] No — la cabecera HSTS **no está presente**

---

## WSTG-CRYP-02 — Pruebas de Padding Oracle

Las vulnerabilidades de Padding Oracle ocurren cuando el proceso de descifrado de una aplicación filtra información sobre si el relleno (padding) de un texto cifrado descifrado es válido o inválido. Al observar estas respuestas de canal lateral (side-channel)—como códigos de estado HTTP distintos, mensajes de error específicos o variaciones en los tiempos de respuesta—un atacante puede descifrar datos sin conocer la clave de cifrado y, potencialmente, volver a cifrar texto en claro (plaintext) arbitrario. Esta vulnerabilidad se manifiesta típicamente en cookies de sesión cifradas, tokens de identidad o parámetros de URL que utilizan cifradores de bloque en modo Cipher Block Chaining (CBC) con relleno PKCS#7. La explotación permite la pérdida completa de confidencialidad y puede conducir al compromiso de la integridad mediante la construcción de bloques cifrados falsificados.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Crítica* |

> *La severidad es Crítica si la vulnerabilidad reside en la gestión de sesiones, tokens de identidad o cifrado de PII.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### ¿Se utiliza un cifrador de bloque en modo CBC para datos sensibles?
- [ ] No — la aplicación utiliza cifrado autenticado (AES-GCM/ChaCha20-Poly1305)  
- [ ] Sí — la aplicación utiliza cifradores de bloque pero **no** se requiere relleno (padding) (ej. cifradores de flujo o modo CTR)  
- [ ] Sí — la aplicación utiliza el modo CBC y se **aplica** relleno (padding)  

### ¿Revela el servidor la validez del relleno a través de canales laterales?
- [ ] No — el servidor devuelve mensajes de error uniformes y tiempos de respuesta consistentes  
- [ ] Sí — el servidor devuelve diferentes códigos de estado HTTP (ej. 200 vs 500) para errores de relleno  
- [ ] Sí — el servidor devuelve mensajes de error específicos (ej. "Invalid Padding", "Decryption failed")  
- [ ] Sí — el servidor presenta diferencias de tiempo medibles durante el fallo del descifrado  

### ¿Es posible el descifrado automatizado del texto cifrado?
- [ ] No — los canales laterales **no** son lo suficientemente estables para la explotación  
- [ ] Sí — el texto cifrado **puede** ser descifrado parcialmente (últimos bloques)  
- [ ] Sí — la recuperación completa del plaintext **es posible** utilizando herramientas como `PadBuster`  

### ¿Puede el atacante realizar bit-flipping o falsificar textos cifrados válidos?
- [ ] No — las verificaciones de integridad (HMAC) se comprueban **antes** del descifrado  
- [ ] Sí — no hay verificación de integridad, pero la construcción del Payload **no** es factible  
- [ ] Sí — la construcción de texto cifrado arbitrario **es posible**, permitiendo la escalada de privilegios o la inyección de datos

---

## WSTG-CRYP-03 — Pruebas de información sensible enviada a través de canales no cifrados

La transmisión de datos sensibles a través de canales no cifrados, como HTTP en texto claro, expone la información a la interceptación y modificación mediante ataques Man-in-the-Middle (MITM). Esta vulnerabilidad suele ocurrir cuando las aplicaciones web no logran imponer Transport Layer Security (TLS) en todos los endpoints, particularmente en componentes legados, formularios de inicio de sesión o durante la transición inicial de HTTP a HTTPS. Desde la perspectiva de un atacante, esto permite el sniffing pasivo de credenciales de autenticación, tokens de sesión e información de identificación personal (PII) en segmentos de red comprometidos o no confiables, como redes Wi-Fi públicas. Garantizar que todos los datos sensibles estén encapsulados dentro de un túnel TLS robusto es fundamental para mantener la confidencialidad e integridad de las interacciones del usuario y prevenir el secuestro de sesión (session hijacking).


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Media |

> *La severidad pasa a ser Alta si las credenciales de autenticación, los identificadores de sesión o la PII se transmiten en texto claro.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### ¿Permite la aplicación la comunicación a través de HTTP no cifrado para endpoints sensibles?
- [ ] No — todo el tráfico de la aplicación se fuerza estrictamente a través de HTTPS *(Más seguro)*  
- [ ] Sí — las páginas no sensibles son accesibles a través de HTTP, pero los endpoints sensibles **no lo son**  
- [ ] Sí — los endpoints sensibles (login, checkout, perfil) **son** accesibles a través de HTTP no cifrado *(Riesgo alto)*  

### ¿Existe una redirección global de HTTP a HTTPS?
- [ ] Sí — la aplicación realiza una redirección 301/302 a HTTPS para todas las peticiones HTTP  
- [ ] Sí — la redirección solo se realiza para endpoints sensibles específicos  
- [ ] No — **no se aplica** ninguna redirección de HTTP a HTTPS  

### ¿Está implementado HTTP Strict Transport Security (HSTS)?
- [ ] Sí — HSTS está **habilitado** con un `max-age` prolongado e incluye las directivas `includeSubDomains` y `preload`  
- [ ] Sí — HSTS está **habilitado** pero carece de las directivas `includeSubDomains` o `preload`  
- [ ] No — HSTS **no está habilitado** en el servidor de aplicaciones  

### ¿Están las cookies sensibles protegidas contra la transmisión en texto claro?

| Nombre de la cookie | Flag Secure presente | Flag Secure ausente |
|---------------------|:--------------------:|:-------------------:|
| `SessionID`         |                      |                     |
| `AuthToken`         |                      |                     |
| `CSRF-Token`        |                      |                     |

### ¿Transmite la aplicación datos sensibles a APIs de terceros a través de canales no cifrados?
- [ ] No — todas las llamadas a APIs externas se realizan a través de túneles cifrados (HTTPS)  
- [ ] Sí — se envía cierta telemetría no sensible a través de HTTP  
- [ ] Sí — **se envían** claves de API, PII o credenciales a terceros a través de HTTP no cifrado

---

## WSTG-CRYP-04 — Testing for Weak Cryptographic Primitives

Las primitivas criptográficas débiles implican el uso de algoritmos obsoletos o inseguros, tales como MD5, SHA-1, DES o RC4, los cuales son susceptibles a ataques de colisión, análisis de frecuencias o descifrado por Brute Force. Estas debilidades ocurren típicamente en el hashing de contraseñas, el cifrado de datos en reposo (encryption at rest), la generación de tokens de sesión o la verificación de firmas digitales dentro del backend de la aplicación. Desde la perspectiva de un atacante, la identificación de estas primitivas permite comprometer la integridad y confidencialidad de los datos, como realizar el craqueo de hashes interceptados para recuperar contraseñas en texto claro o la falsificación de tokens de autenticación. Las aplicaciones modernas deben, en su lugar, utilizar primitivas que sean estándares de la industria, resistentes a colisiones y de alta entropía, como Argon2, Bcrypt o AES-GCM, para garantizar una protección robusta contra el criptoanálisis.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### ¿Se utilizan algoritmos de hashing obsoletos como MD5 o SHA-1 para el almacenamiento de datos sensibles?
- [ ] No — la aplicación utiliza algoritmos modernos resistentes a colisiones como Argon2 o Bcrypt  
- [ ] Sí — se utilizan algoritmos obsoletos pero **solo** para verificaciones de integridad que no son sensibles para la seguridad  
- [ ] Sí — **se utilizan** algoritmos obsoletos para contraseñas o tokens sensibles *(Alta)*  

### ¿Se emplean actualmente cifrados simétricos débiles como DES, 3DES o RC4?
- [ ] No — se utiliza AES (de 128 bits o superior) en un modo seguro como GCM o CBC  
- [ ] Sí — los cifrados legados están **habilitados** por compatibilidad con versiones anteriores, pero **no** son los predeterminados  
- [ ] Sí — los cifrados legados son el método principal para proteger los datos en reposo o en tránsito  

### ¿La aplicación implementa lógica criptográfica personalizada ("roll-your-own") o no estándar?
- [ ] No — la aplicación se adhiere estrictamente a librerías criptográficas estándar y revisadas por pares  
- [ ] Sí — existen wrappers personalizados, pero las primitivas subyacentes **son** estándares de la industria  
- [ ] Sí — **se aplican** algoritmos criptográficos propietarios o lógica personalizada a datos sensibles *(Crítica)*  

### ¿Se gestionan las sales criptográficas (salts) y los vectores de inicialización (IVs) según las mejores prácticas?
- [ ] Sí — los salts son únicos por usuario y los IVs son impredecibles para cada operación  
- [ ] Sí — se utilizan salts, pero **no** son únicos para todos los registros  
- [ ] No — los salts son estáticos, están hardcoded o ausentes, y los IVs **se reutilizan** a través de múltiples sesiones  

### ¿Es la longitud de bits de las claves asimétricas (RSA, Diffie-Hellman) suficiente para los estándares modernos?
- [ ] Sí — las claves RSA/DH son de 2048 bits o superiores, o se utiliza Criptografía de Curva Elíptica (ECC)  
- [ ] No — las claves son inferiores a 2048 bits, pero el bypass **no** es trivial  
- [ ] No — las claves son de 1024 bits o menores, lo que las hace **vulnerables** a la factorización o ataques computacionales

---

## WSTG-BUSL-01 — Pruebas de Validación de Datos de la Lógica de Negocio

Las pruebas de validación de datos de la lógica de negocio se centran en la capacidad de la aplicación para identificar y rechazar datos que son sintácticamente correctos pero lógicamente inválidos dentro del contexto del dominio del negocio. Los atacantes explotan estos fallos enviando valores inesperados —como cantidades negativas en un carrito de compras, fechas pasadas para eventos futuros o enteros extremadamente grandes— para evadir restricciones y manipular el estado de la aplicación. Estas vulnerabilidades suelen residir en flujos de trabajo de varios pasos (multi-step workflows), procesos de pago y módulos de gestión de cuentas donde la lógica del lado del servidor (server-side logic) no logra verificar la integridad contextual de los datos proporcionados por el usuario. Una explotación exitosa puede derivar en pérdidas financieras, compromiso de la integridad y acceso no autorizado a funciones o datos restringidos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### ¿Aplica la aplicación límites de rango lógico en las entradas numéricas?
- [ ] Sí — la aplicación rechaza correctamente valores negativos, cero o excesivamente grandes cuando no son apropiados *(Más seguro)*  
- [ ] Sí — la aplicación rechaza algunos rangos inválidos pero **es** vulnerable a desbordamiento de enteros (integer overflow) o evasión de límites (boundary bypass)  
- [ ] No — la aplicación acepta cualquier valor numérico independientemente del contexto de negocio *(Crítico)*  

### ¿Son consistentes las comprobaciones de validación del lado del servidor en todas las capas de la aplicación?
- [ ] Sí — la validación se aplica de manera consistente tanto en las capas de API/servicio como en la base de datos  
- [ ] Sí — la validación **se aplica** en la interfaz de usuario (UI/Frontend), pero es posible realizar una evasión mediante una solicitud directa a la API  
- [ ] No — la validación **no se aplica** o es inconsistente, lo que permite la corrupción lógica de los datos  

### ¿Puede subvertirse la lógica de negocio manipulando datos en flujos de trabajo de varios pasos (multi-step workflows)?
- [ ] No — el estado se mantiene de forma segura en el servidor y los datos de los pasos anteriores **no pueden** ser alterados  
- [ ] Sí — la validación ocurre en los primeros pasos, pero el envío final **no vuelve a verificar** la integridad de los datos  
- [ ] Sí — la evasión del flujo de trabajo **es posible** saltando pasos o enviando datos fuera de orden  

### ¿Maneja la aplicación adecuadamente los datos de "casos extremos" (edge cases) que evaden las restricciones de negocio?
- [ ] Sí — las restricciones son robustas y manejan entradas inusuales pero sintácticamente válidas (p. ej., años bisiestos, cambios de zona horaria)  
- [ ] Sí — se manejan algunos casos extremos, pero combinaciones específicas **pueden** llevar a un comportamiento inesperado  
- [ ] No — los casos extremos conducen directamente a fallos de lógica, como compras gratuitas o cambios de estado no autorizados

---

## WSTG-BUSL-02 — Probar la capacidad de falsificar peticiones

La falsificación de peticiones (Request Forging) dentro de un contexto de lógica de negocio implica la manipulación de parámetros definidos por la aplicación para realizar acciones fuera del flujo de trabajo o nivel de autorización previstos. Los atacantes se dirigen a procesos de múltiples pasos, como sistemas de pago o registro de cuentas, donde el estado se mantiene a través de parámetros del lado del cliente que el servidor no vuelve a validar contra una fuente de confianza en el backend. Al alterar campos ocultos, valores JSON o parámetros REST como precios, cantidades o indicadores de estado internos, un atacante puede evadir requisitos de pago, escalar privilegios o desencadenar cambios de estado no deseados. Esta vulnerabilidad se manifiesta típicamente en formularios web con estado o APIs donde el servidor confía implícitamente en la representación del estado del negocio proporcionada por el cliente durante una transacción.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Crítica* |

> *La severidad se vuelve Crítica si la petición falsificada permite pérdidas financieras, acciones administrativas no autorizadas o la toma de control total de una cuenta (Account Takeover).

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### ¿Valida la aplicación los parámetros transaccionales contra una "fuente de verdad" en el lado del servidor?
- [ ] Sí — todos los parámetros sensibles (precio, rol, ID) se validan contra la base de datos y la evasión **no es posible**  
- [ ] Sí — se aplica validación, pero puede ser evadida mediante Parameter Pollution o codificación alternativa  
- [ ] No — la aplicación confía en los valores del lado del cliente para determinar el costo o el resultado de una transacción *(Crítica)*  

### ¿Pueden los valores críticos para el negocio (p. ej., cantidades, montos) modificarse a valores no autorizados?
- [ ] No — el servidor rechaza valores negativos, valores cero o valores excesivamente altos  
- [ ] Sí — el servidor acepta valores modificados pero solo dentro de un rango limitado  
- [ ] Sí — **se pueden** enviar valores negativos o cero, lo que conduce a evasiones financieras o de lógica  

### ¿Se utilizan campos ocultos o parámetros de API para mantener el estado a través de procesos de múltiples pasos?
- [ ] No — el estado se mantiene de forma segura a través de sesiones en el servidor o tokens firmados  
- [ ] Sí — el estado se pasa a través del cliente pero las verificaciones de integridad (HMAC/Signatures) están **activadas** y son robustas  
- [ ] Sí — el estado se pasa a través del cliente y las verificaciones de integridad están **desactivadas** o pueden ser eliminadas  

### ¿Es posible falsificar peticiones que omitan pasos intermedios obligatorios en un flujo de trabajo?
- [ ] No — el servidor impone una secuencia estricta de operaciones para cada transacción  
- [ ] Sí — los pasos **pueden** omitirse, pero la acción final aún requiere datos válidos de los pasos anteriores  
- [ ] Sí — la petición final de "enviar" o "completar" **puede** ser falsificada directamente para evadir los pasos de autorización o pago

---

## WSTG-BUSL-03 — Pruebas de comprobación de integridad

Las pruebas de comprobación de integridad se centran en verificar que la aplicación garantice que los datos permanezcan inalterados y sean confiables a medida que se mueven a través de diversos procesos de negocio o entre el cliente y el servidor. Los atacantes se dirigen a esta lógica manipulando parámetros críticos —como precios de productos, cantidades, privilegios de usuario o estados de transacciones— dentro de peticiones HTTP, campos ocultos o cookies. Si la aplicación carece de verificación en el lado del servidor o de controles de integridad robustos como Hashing o HMACs, un adversario puede manipular estos valores para cometer fraude, escalar sus propios privilegios o eludir las restricciones de negocio previstas. Esta prueba es vital para cualquier aplicación que maneje transacciones financieras, transiciones de estado sensibles o flujos de trabajo de varios pasos donde la salida de una etapa sirve como la entrada confiable para la siguiente.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si la falla de integridad permite el robo financiero, el escalamiento de privilegios no autorizado o la modificación de registros persistentes.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### ¿Están los parámetros sensibles del negocio expuestos a la manipulación en el lado del cliente?
- [ ] No — los valores sensibles se gestionan exclusivamente en el lado del servidor  
- [ ] Sí — valores como el precio, la cantidad o los indicadores de estado **están** expuestos pero cifrados  
- [ ] Sí — los valores **están** expuestos en texto plano dentro de campos ocultos, parámetros de URL o cookies  

### ¿Se aplica un mecanismo de integridad (p. ej., HMAC, Checksum) a los parámetros controlados por el cliente?
- [ ] Sí — se **aplica** y verifica un control de integridad robusto en cada petición  
- [ ] Sí — se **aplica** un control de integridad pero utiliza un algoritmo débil/predecible o un secreto embebido (hardcoded)  
- [ ] No — no se **aplican** controles de integridad a los parámetros sensibles  

### ¿Puede el control de integridad ser eludido o recalculado por un atacante?
- [ ] No — la verificación de la firma se aplica estrictamente y el bypass **no es posible**  
- [ ] Sí — la verificación de la firma puede ser eludida omitiendo el parámetro de la firma  
- [ ] Sí — la firma **puede** ser recalculada porque la clave secreta o la lógica está expuesta/es débil  

### ¿Realiza la aplicación una re-validación de los datos en el backend contra una fuente confiable?
- [ ] Sí — el servidor re-valida todos los valores contra la base de datos y el bypass **no es posible**  
- [ ] Sí — el servidor realiza una validación parcial pero el bypass **es posible** a través de casos de borde específicos  
- [ ] No — el servidor confía en los datos proporcionados por el cliente sin verificación en el backend *(Crítico)*  

### ¿Es posible manipular el estado de un proceso de varios pasos?
- [ ] No — el estado de la sesión se gestiona en el lado del servidor y no puede ser manipulado  
- [ ] Sí — la información de estado se transmite a través del cliente y la manipulación **es posible** para saltar pasos o repetir acciones

---

## WSTG-BUSL-04 — Pruebas de Tiempos de Procesamiento

Los ataques de temporización de procesos implican medir el tiempo que una aplicación tarda en procesar solicitudes específicas para inferir información sensible sobre estados internos o la existencia de datos. Al analizar discrepancias a nivel de milisegundos en los tiempos de respuesta —a menudo causadas por lógica condicional de salida anticipada, búsquedas en bases de datos o comparaciones criptográficas que no son de tiempo constante— los atacantes pueden realizar un análisis de canal lateral (side-channel analysis) para enumerar nombres de usuario válidos, verificar fragmentos de contraseñas o confirmar la existencia de registros específicos. Estas vulnerabilidades ocurren típicamente en endpoints de autenticación, formularios de restablecimiento de contraseña y filtros de API con uso intensivo de recursos donde la lógica del backend se ramifica según la validez de la entrada. Desde la perspectiva de un atacante, estas diferencias de tiempo sirven como un "oráculo booleano" que revela verdades sobre los datos del backend, incluso cuando la aplicación devuelve mensajes de error genéricos e idénticos al usuario.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Herramientas:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### ¿Muestra la aplicación tiempos de respuesta variables para solicitudes de recursos válidos frente a inválidos?
- [ ] No — los tiempos de respuesta son idénticos independientemente de la validez de la entrada  
- [ ] Sí — existen diferencias de tiempo insignificantes pero **no** son estadísticamente significativas  
- [ ] Sí — las diferencias de tiempo constantes son **observables** y proporcionan un oráculo confiable  

### ¿Se han implementado funciones de comparación de tiempo constante o retardos artificiales para normalizar los tiempos de respuesta?
- [ ] Sí — se **aplican** algoritmos de tiempo constante a todas las operaciones de comparación sensibles *(Más seguro)*  
- [ ] Sí — se han **habilitado** retardos artificiales o jitter, pero la lógica subyacente sigue sin ser de tiempo constante  
- [ ] No — no se **aplican** mitigaciones de temporización, lo que permite la medición directa de las ramas de la lógica  

### ¿Puede un atacante utilizar el análisis estadístico para aislar la señal de temporización del ruido de la red?
- [ ] No — el jitter de la red y el ruido de la aplicación hacen que el análisis de temporización **no sea posible**  
- [ ] Sí — con un tamaño de muestra suficiente, la señal de temporización **puede** ser aislada del ruido  
- [ ] Sí — el delta de tiempo es lo suficientemente grande como para que **no** se requiera normalización estadística  

### ¿Es posible la enumeración de cuentas a través de la funcionalidad de inicio de sesión o de restablecimiento de contraseña?
- [ ] No — la existencia de la cuenta no se puede determinar mediante la temporización  
- [ ] Sí — las discrepancias de temporización permiten a un atacante remoto **enumerar** nombres de usuario o correos electrónicos válidos  

### ¿Las operaciones con uso intensivo de recursos (p. ej., procesamiento de archivos, búsquedas complejas) filtran la existencia de datos a través de la temporización?
- [ ] No — el consumo de recursos es uniforme independientemente de si se encuentra un registro  
- [ ] Sí — la existencia de registros sensibles **puede** inferirse midiendo la duración del procesamiento en el backend

---

## WSTG-BUSL-05 — Pruebas de límites en el número de veces que se puede utilizar una función

Esta prueba evalúa si una aplicación impone restricciones de lógica de negocio sobre la frecuencia y el número total de veces que una acción o función específica puede ser ejecutada por un único usuario, sesión o dirección IP. El hecho de no implementar estos límites permite a los atacantes abusar de funciones de alto valor —como el envío de SMS OTP, la activación de correos electrónicos de restablecimiento de contraseña, la aplicación de códigos de descuento o la realización de exportaciones masivas de datos— lo que conlleva pérdidas financieras, agotamiento de recursos o daños a la reputación. Estas vulnerabilidades se encuentran típicamente en endpoints transaccionales o funciones de comunicación y se explotan mediante scripts automatizados que repiten la acción mucho más allá de los umbrales de negocio previstos. Desde la perspectiva de un atacante, este es un objetivo principal para el SMS pumping, mail bombing o flujos de trabajo de fuerza bruta (Brute Force) que carecen de un control de frecuencia (Rate Limiting) explícito o de limitadores basados en el conteo.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si la función involucra transacciones financieras, costes de SMS o impacta en la disponibilidad de todo el sistema.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### ¿Define y aplica la aplicación límites a las funciones de negocio sensibles?
- [ ] Sí — los límites se aplican estrictamente y **no es posible** su evasión  
- [ ] Sí — los límites se aplican pero son demasiado altos para prevenir el abuso  
- [ ] No — no se aplican límites a la ejecución de la función  

### ¿Se aplican los límites de velocidad (Rate Limiting) y los contadores de uso en el lado del servidor?
- [ ] Sí — la aplicación se realiza estrictamente en el lado del servidor (server-side) y **no puede** ser manipulada  
- [ ] No — la aplicación depende de lógica del lado del cliente (client-side) (ej. JavaScript o campos ocultos)  
- [ ] No — no existe aplicación en ningún nivel  

### ¿Pueden evadirse los límites de uso mediante la manipulación de encabezados o sesiones?
- [ ] No — la evasión mediante `X-Forwarded-For`, `Origin` o rotación de sesiones **no es posible**  
- [ ] Sí — los límites **pueden** evadirse falseando encabezados de IP (spoofing) o borrando cookies  
- [ ] Sí — los límites **pueden** evadirse creando múltiples cuentas o sesiones  

### ¿Provoca el exceso del límite una respuesta defensiva adecuada?
- [ ] Sí — la aplicación devuelve `429 Too Many Requests` y registra (log) el evento *(Más seguro)*  
- [ ] Sí — la aplicación devuelve un error pero **no** registra el posible abuso  
- [ ] No — la aplicación continúa procesando solicitudes pero ignora la salida  
- [ ] No — la aplicación no proporciona respuesta o falla (crash) bajo carga  

### ¿Es significativo para el negocio el impacto del abuso de la función?
- [ ] No — el abuso de la función tiene un coste o impacto en recursos insignificante  
- [ ] Sí — el abuso **puede** llevar a un consumo menor de recursos o molestias al usuario  
- [ ] Sí — el abuso **puede** conllevar costes financieros significativos (ej. tarifas de SMS) o denegación de servicio *(Crítico)*

---

## WSTG-BUSL-06 — Pruebas de evasión de flujos de trabajo

La evasión de flujos de trabajo (Workflow circumvention) implica la manipulación de la lógica de una aplicación para omitir secuencias previstas o requisitos previos dentro de procesos de múltiples pasos. Los atacantes identifican endpoints sensibles —como confirmaciones de pago, aprobaciones administrativas o pantallas de finalización de MFA— e intentan acceder a ellos directamente, saltándose los pasos intermedios obligatorios. Esta vulnerabilidad ocurre cuando la lógica del lado del servidor no mantiene una máquina de estados robusta, confiando en su lugar en la suposición de que un usuario sigue la ruta guiada por la interfaz de usuario (UI). Una explotación exitosa puede conducir a fallos críticos en la lógica de negocio, incluyendo transacciones no autorizadas, escalada de privilegios y la evasión de mecanismos de verificación de identidad.

| Campo | Valor |
|---|---|
| **ID WSTG** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si la evasión permite transacciones financieras no autorizadas, escalada de privilegios o acceso administrativo.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### ¿Contiene la aplicación procesos de negocio de múltiples etapas?
- [ ] No — la aplicación consiste únicamente en acciones de una sola solicitud  
- [ ] Sí — existen procesos de múltiples etapas (p. ej., carritos de compra, formularios de asistente, registro)  

### ¿El servidor impone estrictamente la secuencia de pasos?
- [ ] Sí — el seguimiento del estado en el lado del servidor garantiza que los pasos **no se puedan** omitir  
- [ ] Sí — los controles del lado del cliente sugieren una secuencia, pero el servidor **no valida** el progreso  
- [ ] No — la aplicación **no impone** ningún orden específico de operaciones  

### ¿Puede un atacante alcanzar la etapa final de un proceso solicitando directamente la URL o endpoint terminal?
- [ ] No — el acceso directo a los pasos terminales **no es posible** sin completarlos previamente  
- [ ] Sí — se puede acceder a los pasos terminales (p. ej., `/api/v1/checkout/complete`) mediante una solicitud directa  

### ¿Verifica la aplicación que los datos de los requisitos previos de los pasos anteriores estén presentes y sean válidos?
- [ ] Sí — el servidor valida todos los datos de los pasos anteriores antes de continuar  
- [ ] No — el servidor **es** vulnerable a la "omisión" de pasos donde se esperan datos pero no se verifican  

### ¿Se pueden evadir controles de seguridad como MFA o verificación de identidad mediante la manipulación del flujo de trabajo?
- [ ] No — los controles críticos **no pueden** ser evadidos mediante la manipulación del flujo  
- [ ] Sí — es posible evadir el MFA o las comprobaciones de identidad saltando al paso posterior a la verificación

---

## WSTG-BUSL-07 — Probar defensas contra el mal uso de la aplicación

Las defensas contra el mal uso de la aplicación evalúan la capacidad de la misma para identificar, registrar y responder a comportamientos anómalos del usuario que se desvían de la lógica de negocio prevista. A diferencia de la seguridad tradicional basada en firmas, estas defensas se centran en detectar patrones como ráfagas rápidas de solicitudes, Credential Stuffing o la enumeración secuencial de recursos que indican un abuso automatizado. Desde la perspectiva de un atacante, esta prueba determina la resiliencia de la aplicación contra la automatización y los ataques "low and slow" diseñados para exfiltrar datos o agotar recursos sin activar las alertas de seguridad estándar. El hecho de no implementar estas defensas suele dar lugar a un Scraping masivo de datos, Account Takeovers o denegación de servicio a través de funciones legítimas de la aplicación que consumen muchos recursos.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### ¿Se aplican mecanismos de Rate Limiting o Throttling en los endpoints sensibles?
- [ ] Sí — **se aplica** y se impone un Rate Limiting estricto en todos los endpoints sensibles *(Más seguro)*  
- [ ] Sí — **se aplica** Rate Limiting pero puede ser evadido mediante la rotación de IP o la manipulación de cabeceras  
- [ ] Sí — **se aplica** Rate Limiting solo a los endpoints de autenticación, dejando otros expuestos  
- [ ] No — **no se aplica** Rate Limiting ni Throttling a ninguna función de la aplicación  

### ¿Detecta y bloquea la aplicación los patrones de interacción automatizados?
- [ ] Sí — el análisis de comportamiento o los CAPTCHAs **están habilitados** y bloquean eficazmente a los bots  
- [ ] Sí — la anti-automatización **está habilitada** pero su evasión **es posible** utilizando navegadores headless o ciclos de sesión  
- [ ] No — la aplicación **no puede** distinguir entre un usuario humano y un script automatizado  

### ¿Se registra el comportamiento anómalo y se acompaña de una respuesta defensiva?
- [ ] Sí — el mal uso activa el bloqueo automatizado y alerta al equipo de seguridad  
- [ ] Sí — el mal uso se registra para auditoría pero **no** se toma ninguna acción defensiva automatizada  
- [ ] No — la aplicación **no registra** ni responde a patrones de actividad inusuales  

### ¿Pueden evadirse las defensas manipulando identificadores del lado del cliente o cabeceras?
- [ ] No — las defensas dependen del estado del lado del servidor y de telemetría verificada  
- [ ] Sí — los controles **pueden** ser evadidos borrando cookies o rotando cadenas de `User-Agent`  
- [ ] Sí — los controles **pueden** ser evadidos inyectando cabeceras como `X-Forwarded-For` o `X-Real-IP`

---

## WSTG-BUSL-08 — Prueba de Carga de Tipos de Archivos No Esperados

La carga de tipos de archivos no esperados permite a los atacantes omitir las restricciones de la lógica de negocio al enviar archivos que la aplicación no tiene la intención de procesar, lo que potencialmente conduce a la ejecución remota de código (Remote Code Execution), Cross-Site Scripting (XSS) o denegación de servicio (DoS). Los atacantes sondean estas vulnerabilidades modificando las extensiones de archivo, los tipos MIME y los magic bytes para engañar a la lógica de validación del lado del servidor y lograr que acepte contenido malicioso. Esto ocurre típicamente en la carga de fotos de perfil, sistemas de gestión documental o archivos adjuntos en tickets de soporte donde existe una validación insuficiente en el back-end. Una explotación exitosa puede comprometer el servidor host si el archivo cargado se ejecuta, o derivar en ataques del lado del cliente si el archivo se sirve a otros usuarios con un Content-Type incorrecto.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### ¿Restringe la aplicación la carga de archivos a un conjunto específico de extensiones?
- [ ] Sí — se aplica una lista blanca (whitelist) estricta de extensiones y el bypass **no es posible**  
- [ ] Sí — la lista blanca de extensiones está presente pero el bypass **es posible** mediante extensiones dobles o null bytes  
- [ ] No — **se puede** cargar cualquier extensión de archivo  

### ¿Se valida el contenido del archivo más allá de la extensión de archivo?
- [ ] Sí — la lógica del lado del servidor verifica los Magic Bytes y realiza una inspección profunda del contenido  
- [ ] Sí — la lógica del lado del servidor verifica el tipo MIME únicamente a través de la cabecera `Content-Type`  
- [ ] No — **no se aplica** ninguna validación de contenido después de la comprobación de la extensión  

### ¿Puede un atacante ejecutar código mediante la carga de una web shell o script?
- [ ] No — los archivos cargados se almacenan en un directorio no ejecutable o en un bucket de almacenamiento (S3/Azure Blob)  
- [ ] Sí — los archivos se almacenan en un directorio accesible vía web pero la ejecución **está deshabilitada** mediante la configuración del servidor  
- [ ] Sí — los scripts maliciosos cargados **pueden** ejecutarse directamente a través de una ruta URL predecible *(Crítica)*  

### ¿Son posibles ataques del lado del cliente como XSS a través de los tipos de archivos cargados?
- [ ] No — los archivos se sirven con `Content-Disposition: attachment` y `X-Content-Type-Options: nosniff`  
- [ ] Sí — los archivos SVG o HTML **pueden** cargarse y renderizarse en el navegador, lo que deriva en XSS  
- [ ] Sí — los metadatos de la imagen (EXIF) **se procesan** y se reflejan en la interfaz de usuario sin sanitización

---

## WSTG-BUSL-09 — Prueba de subida de archivos maliciosos

La funcionalidad de subida de archivos permite a los usuarios enviar datos al servidor, proporcionando un vector directo para introducir contenido malicioso en el entorno de la aplicación. Los atacantes explotan lógicas de validación débiles para subir web shells, malware o archivos especialmente diseñados —como SVG o HTML— para lograr la ejecución remota de comandos (Remote Code Execution (RCE)) o Cross-Site Scripting (XSS). Esta vulnerabilidad se encuentra típicamente en funciones como la configuración de perfiles, sistemas de gestión documental y gestores de archivos adjuntos, donde el servidor no verifica correctamente los tipos de archivo, el contenido y los permisos de ejecución. Una explotación exitosa a menudo conduce al compromiso total del sistema, la exfiltración de datos o el uso del servidor como punto de pivote para ataques a la red interna.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### ¿Proporciona la aplicación funcionalidad para la subida de archivos?
- [ ] No — no existe funcionalidad de subida de archivos  
- [ ] Sí — existe funcionalidad para usuarios autenticados  
- [ ] Sí — la funcionalidad está disponible para usuarios **no autenticados** *(Crítico)*  

### ¿Se aplica la validación de extensiones de archivo en el lado del servidor?
- [ ] Sí — se **aplica** una lista blanca (allowlist) estricta de extensiones y el bypass **no es posible**  
- [ ] Sí — se utiliza una allowlist pero el bypass **es posible** mediante distinción entre mayúsculas y minúsculas o extensiones dobles  
- [ ] Sí — se utiliza una lista negra (blocklist), la cual **puede** ser evadida con extensiones alternativas (ej. `.phtml`, `.asa`)  
- [ ] No — no se **aplica** ninguna validación de extensión en el lado del servidor  

### ¿Se valida el contenido del archivo más allá de la extensión o el tipo MIME?
- [ ] Sí — la aplicación verifica los magic bytes y realiza una inspección profunda del contenido  
- [ ] Sí — la aplicación solo comprueba la cabecera `Content-Type`, la cual **es** fácilmente falsificada (spoofed)  
- [ ] No — la aplicación confía totalmente en la extensión del archivo para la validación  

### ¿Se almacenan los archivos subidos en un directorio con permisos de ejecución?
- [ ] No — los archivos se almacenan en un servicio de almacenamiento dedicado (ej. S3) o en un directorio no ejecutable  
- [ ] Sí — los archivos se almacenan en el servidor web pero la ejecución está **deshabilitada** mediante configuración  
- [ ] Sí — los archivos se almacenan en un directorio accesible vía web y la ejecución **está habilitada** *(Crítico)*  

### ¿Se pueden evadir los filtros de seguridad mediante la manipulación del nombre del archivo?
- [ ] No — los nombres de archivo son saneados o regenerados aleatoriamente por el servidor  
- [ ] Sí — los filtros **pueden** ser evadidos utilizando bytes nulos (null bytes) (ej. `shell.php%00.jpg`)  
- [ ] Sí — se **pueden** utilizar caracteres de path traversal (ej. `../../`) para subir archivos a directorios arbitrarios

---

## WSTG-BUSL-10 — Pruebas de la Funcionalidad de Pago

Las pruebas de la funcionalidad de pago consisten en identificar fallos de lógica de negocio que permitan a un atacante omitir pasarelas de pago, manipular los totales de los pedidos o falsificar señales de transacciones exitosas. Estas vulnerabilidades residen típicamente en la comunicación entre la aplicación, el navegador del cliente y los procesadores de pago de terceros como Stripe, PayPal o sistemas de contabilidad internos. Un atacante puede intentar modificar los parámetros de precio en tránsito, enviar cantidades negativas para reducir el costo total o realizar un replay de los callbacks de transacciones exitosas para completar pedidos sin un pago real. Una explotación exitosa conlleva pérdidas financieras directas, discrepancias en el inventario y el compromiso potencial de la integridad del comercio electrónico de la aplicación.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### ¿Se valida el monto de la transacción en el lado del servidor antes del procesamiento?
- [ ] Sí — el monto se valida estrictamente contra la base de datos de productos del lado del servidor *(Más seguro)*  
- [ ] Sí — el monto se valida pero es posible realizar un bypass de la lógica mediante Parameter Pollution o cambio de moneda  
- [ ] No — el monto de la transacción se puede modificar en la solicitud del lado del cliente y es aceptado por el backend  

### ¿Pueden manipularse las cantidades de los artículos para dar como resultado un total de cero o negativo?
- [ ] No — las cantidades negativas o en cero son rechazadas por la lógica de validación de la aplicación  
- [ ] Sí — se aceptan cantidades negativas en el carrito pero el total se corrige en el checkout  
- [ ] Sí — se pueden utilizar cantidades negativas para reducir el total general del pedido u obtener un crédito *(Crítico)*  

### ¿Gestiona la aplicación de forma segura los callbacks de la pasarela de pago o los webhooks?
- [ ] Sí — los callbacks requieren una firma criptográfica válida que no puede ser falsificada  
- [ ] Sí — se verifican las firmas pero el secreto es débil o la verificación se puede omitir  
- [ ] No — la aplicación depende de callbacks no autenticados o no verificados para confirmar el estado del pago  

### ¿Es la aplicación vulnerable a Race Conditions durante la fase de checkout o pago?
- [ ] No — la integridad transaccional y los mecanismos de bloqueo de la base de datos están habilitados  
- [ ] Sí — las Race Conditions son posibles pero no impactan el precio final o el cumplimiento del pedido  
- [ ] Sí — las solicitudes concurrentes pueden dar lugar a múltiples cumplimientos para un solo pago o al "doble gasto" de tarjetas de regalo  

### ¿Están los códigos de descuento o puntos de fidelidad sujetos a manipulación o reutilización?
- [ ] No — los códigos se validan para un solo uso y se aplican las restricciones lógicas  
- [ ] Sí — los códigos pueden reutilizarse o aplicarse a artículos no elegibles, pero el impacto es limitado  
- [ ] Sí — los atacantes pueden aplicar múltiples códigos en conflicto o eludir los límites de expiración y uso

---

## WSTG-CLNT-01 — Testing for DOM Based Cross Site Scripting

El Cross-Site Scripting basado en DOM (DOM XSS) ocurre cuando una aplicación contiene JavaScript del lado del cliente que procesa datos de una fuente no confiable de manera insegura, generalmente escribiendo los datos de vuelta en el Document Object Model (DOM) a través de un sink peligroso. A diferencia del XSS reflejado o almacenado, la vulnerabilidad existe completamente dentro del código del lado del cliente; el Payload a menudo no se envía al servidor en absoluto, ya que es procesado por sinks como `innerHTML`, `eval()` o `document.write()`. Los atacantes explotan esto creando URLs con fragmentos o parámetros maliciosos que, al ser cargados por el navegador de la víctima, ejecutan JavaScript arbitrario en el contexto de la sesión del usuario. Esto puede llevar a la exfiltración de tokens de sesión, el robo de datos sensibles y acciones no autorizadas realizadas en nombre del usuario autenticado.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Herramientas:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### ¿Se utilizan fuentes no confiables para capturar datos controlados por el usuario en JavaScript?
- [ ] No — JavaScript **no** utiliza `location.hash`, `location.search` o `document.referrer`  
- [ ] Sí — se utilizan fuentes no confiables pero los datos **no** se pasan a ningún sink de ejecución  
- [ ] Sí — se utilizan fuentes no confiables y los datos fluyen directamente hacia la lógica del lado del cliente  

### ¿Se pasan datos controlados por el usuario a sinks peligrosos del DOM?
- [ ] No — **no** se utilizan sinks tales como `innerHTML`, `outerHTML`, `document.write()` o `eval()`  
- [ ] Sí — existen sinks peligrosos pero los datos están estrictamente **sanitizados** utilizando una librería segura como DOMPurify  
- [ ] Sí — existen sinks peligrosos y los datos se procesan con un filtrado basado en regex **insuficiente** o personalizado  
- [ ] Sí — existen sinks peligrosos y los datos se pasan **sin** ninguna sanitización o codificación *(Crítico)*  

### ¿Es posible alcanzar el sink de ejecución a través de un payload basado en URL?
- [ ] No — el flujo de la fuente al sink (source-to-sink) **no** puede activarse mediante una entrada externa  
- [ ] Sí — el flujo es **posible** pero requiere una interacción compleja del usuario  
- [ ] Sí — el flujo es **posible** y puede activarse a través de un fragmento de URL o un parámetro de consulta simple  

### ¿Los encabezados de seguridad modernos o las mitigaciones basadas en el navegador están neutralizando el riesgo?
- [ ] Sí — una `Content-Security-Policy` (CSP) estricta con `script-src` y `trusted-types` está **habilitada** y previene la ejecución  
- [ ] Sí — existe una CSP pero `unsafe-inline` o hashes débiles hacen que un Exploit sea **posible**  
- [ ] No — no existe una CSP para mitigar la ejecución basada en DOM  

### ¿Utiliza la aplicación frameworks del lado del cliente con protecciones integradas?
- [ ] Sí — el framework (ej. Angular, React) codifica automáticamente los datos y el bypass **no es posible**  
- [ ] Sí — se utiliza el framework pero las "vías de escape" como `dangerouslySetInnerHTML` están **habilitadas**  
- [ ] No — la aplicación utiliza JavaScript puro (vanilla) o librerías legadas (ej. versiones antiguas de jQuery) sin auto-escaping

---

## WSTG-CLNT-02 — Pruebas de ejecución de JavaScript

Las pruebas de ejecución de JavaScript se centran en identificar instancias en las que una aplicación maneja de forma inadecuada los datos suministrados por el usuario, lo que conduce a la ejecución de scripts no autorizados dentro del contexto del navegador de la víctima. Esta vulnerabilidad suele dar como resultado Cross-Site Scripting (XSS), lo que permite a los atacantes exfiltrar cookies de sesión, manipular el Document Object Model (DOM) o realizar acciones en nombre del usuario. Los evaluadores de penetración examinan receptores (sinks) como `innerHTML`, `document.write()` y `eval()` donde pueden procesarse datos no saneados provenientes de fuentes (sources) como fragmentos de URL, `localStorage` o `window.name`. Una explotación exitosa compromete la integridad y confidencialidad de la sesión del usuario y puede servir como pivote para ataques del lado del cliente (client-side) más complejos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Herramientas:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### ¿Procesa la aplicación datos suministrados por el usuario en receptores (sinks) de JavaScript peligrosos?
- [ ] No — todos los datos están saneados o se utilizan en receptores seguros como `textContent`  
- [ ] Sí — los datos se procesan en receptores peligrosos pero **se aplica** saneamiento/codificación  
- [ ] Sí — los datos se procesan en receptores peligrosos y **no se aplica** saneamiento  

### ¿Existe una Content Security Policy (CSP) para mitigar la ejecución de scripts?
- [ ] Sí — una CSP restrictiva está **habilitada** y el bypass **no es posible**  
- [ ] Sí — una CSP está **habilitada** pero el bypass **es posible** a través de `unsafe-inline` o CDNs inseguros  
- [ ] No — no hay ninguna CSP **habilitada**  

### ¿Se puede ejecutar JavaScript a través de parámetros o fragmentos de URL (DOM-based XSS)?
- [ ] No — la entrada está codificada correctamente y la ejecución **no es posible**  
- [ ] Sí — la entrada se refleja en el DOM pero la ejecución **es prevenida** por las protecciones de frameworks modernos  
- [ ] Sí — la entrada se ejecuta directamente en el navegador y la explotación **es posible**  

### ¿Son los manejadores de eventos o los esquemas URI vulnerables a la inyección de scripts?
- [ ] No — la entrada se sanea para eliminar atributos de eventos y esquemas `javascript:`  
- [ ] Sí — los manejadores de eventos **pueden** ser inyectados pero los filtros limitan la complejidad del Payload  
- [ ] Sí — la ejecución de JavaScript arbitrario a través de los atributos `onerror`, `onload` o `href` **es posible**

---

## WSTG-CLNT-03 — HTML Injection

La Inyección HTML (HTML Injection) ocurre cuando una aplicación no logra sanitizar las entradas proporcionadas por el usuario antes de renderizarlas en el navegador, permitiendo que un atacante inyecte etiquetas HTML arbitrarias en el Document Object Model (DOM) de la página web. Aunque es similar al Cross-Site Scripting (XSS), el objetivo principal de la Inyección HTML es manipular la presentación visual de la página o facilitar ataques de Phishing mediante la inyección de formularios maliciosos y contenido engañoso. Los atacantes apuntan a parámetros que se reflejan en la interfaz de usuario (UI), como términos de búsqueda, campos de perfil de usuario o mensajes de error, para engañar a las víctimas y lograr que revelen credenciales sensibles o interactúen con enlaces de terceros no autorizados. Esta vulnerabilidad representa un riesgo significativo para la integridad de la interfaz de usuario de la aplicación y puede ser un precursor de ataques más complejos de ingeniería social o relacionados con la sesión.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Baja / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### ¿Se reflejan las entradas controladas por el usuario en el DOM sin una codificación HTML adecuada?
- [ ] No — toda la entrada reflejada es estrictamente codificada en HTML antes de ser renderizada
- [ ] Sí — algunos caracteres **están** codificados, pero **es posible** realizar bypasses para ciertas etiquetas
- [ ] Sí — la entrada se refleja en bruto y la Inyección HTML **es posible**

### ¿Se pueden inyectar elementos HTML estructurales para alterar el diseño de la página?
- [ ] No — la aplicación filtra o elimina las etiquetas estructurales como `<div>`, `<table>` o `<iframe>`
- [ ] Sí — los elementos estructurales **pueden** ser inyectados, pero **no** impactan significativamente en la UI
- [ ] Sí — **es posible** una alteración visual (defacement) significativa de la UI mediante etiquetas inyectadas

### ¿Es posible inyectar elementos funcionales de phishing como formularios o enlaces maliciosos?
- [ ] No — las etiquetas `<form>`, `<input>` y `<a>` son bloqueadas o sanitizadas eficazmente
- [ ] Sí — se **pueden** inyectar enlaces maliciosos para redirigir a los usuarios a sitios externos
- [ ] Sí — se **pueden** inyectar elementos `<form>` funcionales para la recolección de credenciales *(Media)*

### ¿Mitigan el impacto de la inyección las cabeceras de seguridad o las Content Security Policies (CSP)?
- [ ] Sí — existe una CSP restrictiva y se **aplica** `form-action` o `frame-src`
- [ ] No — las cabeceras de seguridad están presentes, pero la CSP **está deshabilitada** o contiene unsafe-inline/comodines
- [ ] No — no hay CSP ni cabeceras de seguridad pertinentes **habilitadas**

---

## WSTG-CLNT-04 — Pruebas de Redirección de URL en el Lado del Cliente

La redirección de URL en el lado del cliente (Client-side URL redirection) ocurre cuando una aplicación web acepta una entrada controlada por el usuario—normalmente a través de parámetros de URL o fragmentos hash—y la utiliza dentro de un sumidero (sink) de redirección en el lado del cliente como `window.location` o `document.location.href`. Esta vulnerabilidad es aprovechada principalmente por los atacantes para realizar campañas de phishing sofisticadas, ya que la víctima confía inicialmente en el dominio legítimo antes de ser redirigida silenciosamente a un sitio malicioso. Más allá del phishing, las redirecciones en el lado del cliente a menudo pueden encadenarse para la exfiltración de datos sensibles, tales como tokens de acceso OAuth o identificadores de sesión, si la redirección ocurre mientras esos valores están presentes en la URL o en la cabecera `Referer`. En las Aplicaciones de Página Única (SPA) modernas, esta vulnerabilidad aparece frecuentemente en la lógica de enrutamiento, en parámetros "return to" tras la autenticación o en funcionalidades de cambio de idioma.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media* |

> *La severidad pasa a ser Alta si la redirección puede utilizarse para la exfiltración de tokens OAuth, IDs de sesión o para evadir controles de seguridad en un flujo de autenticación.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Herramientas:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### ¿Utiliza la aplicación scripts en el lado del cliente para gestionar redirecciones basadas en la entrada del usuario?
- [ ] No — la redirección se gestiona exclusivamente en el lado del servidor mediante códigos de estado HTTP `3xx`  
- [ ] Sí — la aplicación utiliza sinks de JavaScript como `window.location` para navegar basándose en parámetros de URL  
- [ ] Sí — la aplicación utiliza un enrutador de framework de cliente (ej. React Router, Vue Router) que procesa rutas proporcionadas por el usuario  

### ¿Se han implementado controles de validación de entrada o listas blancas (whitelisting) para la URL de destino?
- [ ] Sí — se aplica una **whitelist** estricta de dominios/rutas permitidos y el bypass **no es posible**  
- [ ] Sí — existe validación pero depende de regex débiles o listas negras (blacklists), y el bypass **es posible**  
- [ ] No — la aplicación acepta cualquier cadena arbitraria como destino de redirección  

### ¿Es posible evadir la lógica de redirección utilizando técnicas comunes de ofuscación?
- [ ] No — los controles manejan correctamente los caracteres codificados, las URLs relativas al protocolo (`//`) y las barras invertidas  
- [ ] Sí — el bypass **es posible** mediante URL encoding o doble codificación  
- [ ] Sí — el bypass **es posible** mediante URLs relativas al protocolo o caracteres `@` (ej. `https://expected.com@attacker.com`)  
- [ ] Sí — el bypass **es posible** utilizando comportamientos específicos del navegador (quirks) o inyección de bytes nulos (null byte injection)  

### ¿Expone el proceso de redirección datos sensibles al sitio de destino?
- [ ] No — no hay datos sensibles presentes en la URL ni se transmiten mediante cabeceras durante la redirección  
- [ ] Sí — los datos sensibles (ej. tokens de sesión, claves API) **se filtran** a través de la cabecera `Referer` al sitio externo  
- [ ] Sí — los datos sensibles presentes en el fragmento de URL (`#`) **son accesibles** para los scripts del sitio de destino  

### ¿Es posible ejecutar JavaScript a través del sink de redirección (DOM-based XSS)?
- [ ] No — el sink solo permite la navegación a protocolos `http` o `https`  
- [ ] Sí — el sink de redirección permite URIs `javascript:` o `data:`, lo que hace que el DOM-based XSS **sea posible**

---

## WSTG-CLNT-05 — Testing for CSS Injection

La inyección CSS (CSS Injection) ocurre cuando una aplicación permite que la entrada proporcionada por el usuario influya en las hojas de estilo en cascada (Cascading Style Sheets - CSS) de una página web sin un saneamiento o escape adecuado. Aunque a menudo se percibe como un problema cosmético, los atacantes pueden aprovechar la inyección CSS para exfiltrar datos sensibles, como tokens CSRF o IDs de sesión, mediante el uso de selectores de atributos y propiedades de imagen de fondo (`background-image`) para activar solicitudes externas. Esta vulnerabilidad ocurre típicamente en puntos finales (endpoints) donde las preferencias del usuario (por ejemplo, temas personalizados, colores de fuente) se reflejan dentro de bloques `<style>` o atributos de estilo en línea (`style`). Desde la perspectiva de un atacante, una explotación exitosa puede conducir a la redistribución de la interfaz de usuario (UI Redressing), phishing mediante la modificación no autorizada del diseño, o la exfiltración sigilosa de datos en entornos donde las políticas de seguridad de contenido (Content Security Policies - CSP) estrictas podrían bloquear ataques basados en JavaScript.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta* |

> *La severidad aumenta a Alta si el punto de inyección permite la exfiltración de atributos sensibles como tokens CSRF o si puede utilizarse para UI Redressing en flujos de trabajo sensibles.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### ¿Refleja la aplicación la entrada controlada por el usuario dentro de contextos CSS?
- [ ] No — la entrada **no** se refleja en etiquetas de estilo, atributos o archivos CSS  
- [ ] Sí — la entrada se refleja pero está estrictamente limitada a valores alfanuméricos seguros  
- [ ] Sí — la entrada se refleja dentro de un atributo `style` o un bloque `<style>`  

### ¿Están correctamente saneados los metacaracteres y funciones específicos de CSS?
- [ ] Sí — los caracteres como `{`, `}`, `:` y funciones como `url()` están **correctamente escapados**  
- [ ] Sí — el saneamiento está implementado pero es posible **evadirlo (bypass)** mediante codificación o comentarios CSS  
- [ ] No — no se aplica ningún saneamiento a los metacaracteres CSS  

### ¿Es posible la exfiltración de datos utilizando selectores CSS?
- [ ] No — los selectores **no pueden** activar solicitudes externas ni filtrar datos  
- [ ] Sí — la exfiltración parcial de datos **es posible** mediante selectores de atributos y `background-image`  
- [ ] Sí — la exfiltración completa de tokens sensibles **es posible** mediante técnicas de fuerza bruta (Brute Force) automatizadas  

### ¿Mitiga una Política de Seguridad de Contenido (CSP) el riesgo de inyección CSS?
- [ ] Sí — `style-src` está restringido a 'self' e `img-src` o `connect-src` **está restringido**  
- [ ] Sí — la CSP está presente pero utiliza 'unsafe-inline' o permite dominios externos con comodines (wildcards)  
- [ ] No — no se ha implementado ninguna CSP para evitar la carga de recursos externos a través de CSS  

### ¿Puede utilizarse la vulnerabilidad para realizar UI Redressing o phishing?
- [ ] No — el alcance de la inyección es demasiado limitado para modificar el diseño de manera significativa  
- [ ] Sí — la modificación de la interfaz de usuario (UI modification) **es posible**, permitiendo la superposición de elementos maliciosos  
- [ ] Sí — el control total del diseño de la página **es posible**, facilitando ataques de phishing altamente convincentes

---

## WSTG-CLNT-06 — Pruebas de Manipulación de Recursos del Lado del Cliente

La manipulación de recursos del lado del cliente ocurre cuando una aplicación permite que la entrada controlada por el usuario influya en la fuente o ruta de los recursos cargados por el navegador, tales como scripts, hojas de estilo o iframes. Al manipular fragmentos de URI, parámetros de consulta u otras fuentes del DOM, un atacante puede obligar a la aplicación a cargar contenido externo malicioso o ejecutar código no autorizado. Esta vulnerabilidad se encuentra típicamente en la lógica de JavaScript que puebla dinámicamente los atributos `src` o `href` de los elementos HTML sin una validación suficiente contra una lista de permitidos (allow-list) confiable. Desde la perspectiva de un atacante, esto se explota mediante la construcción de una URL que redirecciona la carga de recursos a un servidor controlado por el atacante, lo que potencialmente resulta en Cross-Site Scripting (XSS) basado en DOM, exfiltración de datos sensibles o UI redressing.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Media / Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Herramientas:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### ¿Utiliza la aplicación sinks del lado del cliente para cargar o incrustar recursos dinámicamente?
- [ ] No — los recursos se cargan únicamente a través de HTML estático o lógica del lado del servidor  
- [ ] Sí — el JavaScript del lado del cliente puebla dinámicamente atributos de recursos como `src`, `href` o `data`  

### ¿Se han implementado controles de validación o listas de permitidos para las fuentes URI de los recursos?
- [ ] Sí — **se aplican** listas de permitidos estrictas y validación de origen a todos los recursos dinámicos  
- [ ] Sí — la validación está presente pero depende de listas negras (blacklists) débiles o expresiones regulares fáciles de evadir  
- [ ] No — no se aplican controles de validación a las rutas de recursos controladas por el usuario  

### ¿Es posible evadir la validación de URI utilizando protocolos alternativos o codificación?
- [ ] No — **no es posible** evadir los filtros de recursos  
- [ ] Sí — la evasión **es posible** utilizando diferentes protocolos como `data:`, `vbscript:` o `javascript:`  
- [ ] Sí — la evasión **es posible** utilizando caracteres codificados o bytes nulos para eludir coincidencias de cadenas simples  

### ¿Puede un atacante redireccionar la carga de recursos a un dominio externo no confiable?
- [ ] No — las rutas de los recursos están restringidas al mismo origen o a un subdirectorio fijo  
- [ ] Sí — la aplicación **puede** ser forzada a cargar scripts o estilos desde un dominio externo arbitrario  

### ¿Cuál es el impacto máximo de la manipulación de recursos?
- [ ] Bajo — la manipulación solo afecta a elementos no ejecutables como imágenes o texto no sensible  
- [ ] Medio — la manipulación permite la inyección de CSS o un UI redressing significativo  
- [ ] Alto — la manipulación conduce a XSS basado en DOM o a la ejecución de JavaScript arbitrario *(Crítico)*

---

## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) es un mecanismo basado en el navegador que permite a un servidor autorizar explícitamente el acceso a sus recursos desde diferentes orígenes, relajando efectivamente la Same-Origin Policy (SOP). Las configuraciones erróneas ocurren cuando una aplicación confía excesivamente en orígenes arbitrarios, a menudo reflejando el encabezado `Origin` en el encabezado de respuesta `Access-Control-Allow-Origin` o utilizando una whitelist de regex mal implementada. Desde la perspectiva de un atacante, una política de CORS permisiva puede ser explotada alojando un script malicioso en un dominio controlado que realice solicitudes autenticadas a la aplicación vulnerable, permitiendo al atacante exfiltrar datos sensibles, como tokens CSRF o PII, directamente desde el cuerpo de la respuesta. Esta vulnerabilidad es más crítica en los endpoints de API que procesan sesiones autenticadas y devuelven información no pública.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Estado de la Prueba** | No Realizado |
| **Severidad** | Media / Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Herramientas:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### ¿Implementa la aplicación encabezados CORS en los endpoints autenticados?
- [ ] No — CORS **no está habilitado**; el navegador utiliza por defecto la política estricta Same-Origin Policy  
- [ ] Sí — Los encabezados CORS están presentes y restringidos a una **whitelist** estricta  
- [ ] Sí — Los encabezados CORS están presentes pero configurados de forma **permisiva**  

### ¿Cómo maneja el servidor el encabezado `Access-Control-Allow-Origin` (ACAO)?
- [ ] ACAO está configurado para un dominio estático específico y **confiable**  
- [ ] ACAO está configurado como `*` (comodín) y las credenciales **no pueden** enviarse  
- [ ] ACAO está configurado como `*` (comodín) y las credenciales **pueden** enviarse *(Nota: La mayoría de los navegadores bloquean esto)*  
- [ ] ACAO **refleja dinámicamente** el valor del encabezado `Origin` de la solicitud  

### ¿Está el encabezado `Access-Control-Allow-Credentials` (ACAC) configurado como `true`?
- [ ] No — ACAC **no está presente** o está configurado como `false`  
- [ ] Sí — ACAC está configurado como `true`, pero ACAO está **restringido** a orígenes confiables  
- [ ] Sí — ACAC está configurado como `true` y ACAO **refleja** orígenes arbitrarios *(Crítico)*  

### ¿Puede omitirse la whitelist de origen mediante la manipulación de subdominios o el origen null?
- [ ] No — La whitelist se aplica estrictamente y **no puede** ser omitida  
- [ ] Sí — La whitelist acepta **cualquier** subdominio del objetivo (ej. `attacker.target.com`)  
- [ ] Sí — La whitelist acepta el origen `null` a través de iframes en sandbox  
- [ ] Sí — La whitelist es vulnerable a omisiones de regex (ej. `target.com.attacker.com` o `target.com-safe.com`)  

### ¿Permite la configuración de CORS la exfiltración de datos sensibles?
- [ ] No — Los endpoints expuestos **no** contienen datos sensibles o específicos de la sesión  
- [ ] Sí — Los endpoints autenticados devuelven PII, credenciales o tokens CSRF que **pueden** ser leídos por un origen no autorizado

---

## WSTG-CLNT-08 — Testing for Cross Site Flashing

El Cross-Site Flashing (XSF) es una vulnerabilidad de lado del cliente que ocurre cuando un archivo Flash (SWF) maneja incorrectamente la entrada controlada por el usuario, permitiendo que un atacante ejecute código ActionScript malicioso o establezca un puente hacia el entorno JavaScript del navegador. Al manipular parámetros como `FlashVars` o cadenas de consulta de URL (query strings) pasados a sumideros (sinks) como `ExternalInterface.call`, `getURL` o `loadMovie`, un atacante puede realizar acciones en el contexto del dominio vulnerable, incluyendo el secuestro de sesión (session hijacking) y la exfiltración de datos. Esta vulnerabilidad se encuentra principalmente en aplicaciones empresariales heredadas (legacy) que aún alojan archivos SWF, donde la validación de entrada insuficiente permite que la película Flash sea reutilizada como un vector de Cross-Site Scripting (XSS) o para evadir la Same-Origin Policy (SOP) a través de configuraciones de dominio cruzado permisivas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### ¿Se alojan archivos heredados (legacy) de Adobe Flash (.swf) en la aplicación?
- [ ] No — no se alojan ni se referencian archivos Flash dentro del alcance de la aplicación  
- [ ] Sí — hay archivos SWF presentes pero solo sirven contenido estático  
- [ ] Sí — hay archivos SWF presentes y aceptan parámetros dinámicos a través de `FlashVars` o cadenas de URL  

### ¿Se sanitiza la entrada pasada a los sumideros (sinks) de ActionScript?
- [ ] Sí — todas las entradas son validadas estrictamente y los sumideros de ActionScript **no pueden** ser manipulados  
- [ ] Sí — se aplica validación pero la evasión (bypass) **es posible** mediante técnicas de codificación específicas  
- [ ] No — la entrada se pasa directamente a sumideros sensibles como `ExternalInterface.call` o `getURL` *(Crítico)*  

### ¿Puede el archivo Flash ser utilizado para ejecutar JavaScript arbitrario (XSS)?
- [ ] No — `ExternalInterface` está **deshabilitado** o el parámetro `allowScriptAccess` está establecido en `never`  
- [ ] Sí — `allowScriptAccess` está establecido en `sameDomain` pero el SWF está alojado en el dominio de destino  
- [ ] Sí — `allowScriptAccess` está establecido en `always`, permitiendo XSS desde cualquier dominio  

### ¿Previene la política `crossdomain.xml` las solicitudes de origen cruzado no autorizadas?
- [ ] Sí — `crossdomain.xml` es restrictivo y solo permite orígenes específicos y confiables  
- [ ] No — el archivo `crossdomain.xml` **no existe**  
- [ ] Sí — la política es excesivamente permisiva (ej., `<allow-access-from domain="*" />`)  

### ¿Se puede forzar al archivo SWF a cargar películas externas controladas por un atacante?
- [ ] No — la aplicación **no puede** ser forzada a cargar archivos SWF externos  
- [ ] Sí — las funciones `loadMovie` o `loadMovieNum` aceptan URLs externas no validadas

---

## WSTG-CLNT-09 — Pruebas de Clickjacking

El Clickjacking, también conocido como UI redressing, ocurre cuando un atacante utiliza capas transparentes u opacas para engañar a un usuario para que haga clic en un botón o enlace en otra página cuando su intención era hacer clic en la página de nivel superior. Esta vulnerabilidad compromete la integridad de las acciones del usuario, lo que potencialmente conduce a la modificación de datos no autorizada, cambios en la cuenta o transferencias financieras realizadas en nombre de la víctima. Los atacantes suelen explotar esto incrustando el sitio web objetivo dentro de un `<iframe>` en un sitio malicioso controlado y superponiéndolo con contenido engañoso o señuelos atractivos. Ocurre con mayor frecuencia en páginas que realizan acciones sensibles que cambian el estado, como botones de "Eliminar cuenta", formularios de restablecimiento de contraseña o paneles de configuración administrativa.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Estado de la Prueba** | No Realizado |
| **Severidad** | Media / Alta* |

> *La severidad se convierte en Alta si se pueden activar acciones sensibles (p. ej., transferencias de fondos, eliminación de cuentas o escalada de privilegios) a través de clickjacking.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Herramientas:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### ¿Implementa la aplicación cabeceras del lado del servidor para prevenir el framing no autorizado?
- [ ] Sí — Se aplican `X-Frame-Options` o `Content-Security-Policy` y previenen el framing *(Más seguro)*  
- [ ] Sí — Las cabeceras están presentes pero **mal configuradas** (p. ej., `X-Frame-Options: ALLOW-FROM` que carece de un amplio soporte en los navegadores)  
- [ ] No — No se aplican cabeceras de protección contra framing  

### ¿Está implementada la directiva `frame-ancestors` de `Content-Security-Policy` (CSP)?
- [ ] Sí — Se aplica `frame-ancestors 'none'` o `'self'`  
- [ ] Sí — `frame-ancestors` está presente pero **permite** orígenes no confiables o excesivamente amplios  
- [ ] No — La directiva **no existe** o no se ha implementado CSP  

### ¿Está la cabecera `X-Frame-Options` (XFO) configurada correctamente para la compatibilidad con navegadores antiguos?
- [ ] Sí — Se aplica `DENY` o `SAMEORIGIN`  
- [ ] Sí — XFO está presente pero es **ignorada** por los navegadores modernos porque existe una directiva `frame-ancestors` de CSP  
- [ ] No — La cabecera XFO **no existe** o se ha establecido en un valor inseguro  

### ¿Pueden omitirse las protecciones de framing utilizando técnicas conocidas?
- [ ] No — La omisión (bypass) **no es posible** mediante técnicas estándar (double-framing, etc.)  
- [ ] Sí — La protección **puede** omitirse debido a una expresión regular (regex) o lógica débil en la lista blanca de `frame-ancestors`  
- [ ] Sí — La protección **puede** omitirse porque la aplicación depende exclusivamente de técnicas de frame-busting basadas en JavaScript  

### ¿Es explotable una acción sensible a través de una prueba de concepto de clickjacking?
- [ ] No — El framing está bloqueado o no se puede acceder a acciones sensibles  
- [ ] Sí — El UI redressing **es posible** pero requiere una interacción compleja del usuario  
- [ ] Sí — El UI redressing **es posible** y permite acciones inmediatas que cambian el estado *(Crítico)*

---

## WSTG-CLNT-10 — Pruebas de WebSockets

Las pruebas de WebSocket se centran en el canal de comunicación full-duplex establecido entre el cliente y el servidor, el cual omite los patrones tradicionales de solicitud-respuesta de HTTP. Los pentesters evalúan la seguridad del proceso de handshake, la presencia de protecciones contra Cross-Site WebSocket Hijacking (CSWSH) y el rigor de la validación de entradas aplicada a los payloads de los mensajes. La explotación suele implicar el secuestro de una sesión activa para la exfiltración de datos en tiempo real o la inyección de payloads maliciosos que activen vulnerabilidades en el lado del servidor, como Command Injection o SQLi, a través del socket persistente. Dado que muchos Web Application Firewalls (WAF) tradicionales no inspeccionan el tráfico WebSocket de manera efectiva, este protocolo a menudo proporciona un vector sigiloso para evadir las defensas perimetrales.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Herramientas:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### ¿Se establece la conexión WebSocket a través de un canal seguro?
- [ ] Sí — se obliga el uso de `wss://` (WebSocket Secure) para todas las conexiones  
- [ ] No — se utiliza `ws://`, lo que permite la interceptación por parte de un person-in-the-middle (PITM)  

### ¿Está la aplicación protegida contra Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] No — el servidor valida estrictamente el encabezado `Origin` durante el handshake  
- [ ] Sí — la validación del encabezado `Origin` es débil o **puede** ser evadida mediante orígenes nulos o suplantados  
- [ ] Sí — no se realiza ninguna validación del encabezado `Origin`, lo que permite a atacantes cross-site iniciar una conexión *(Crítico)*  

### ¿Se aplica validación y saneamiento de entradas a los payloads de los mensajes WebSocket?
- [ ] Sí — todos los mensajes entrantes son saneados y validados contra un esquema  
- [ ] Sí — existe cierta validación, pero la evasión **es posible** utilizando payloads codificados o no estandarizados  
- [ ] No — los mensajes se procesan directamente, lo que permite la inyección (XSS, SQLi, RCE) o la manipulación de la lógica  

### ¿Se realizan comprobaciones de autenticación y autorización en cada mensaje WebSocket?
- [ ] Sí — se verifica la sesión para cada mensaje o el protocolo es sin estado (stateless) mediante tokens  
- [ ] Sí — la autenticación ocurre en el handshake, pero la autorización para acciones específicas **no se aplica** por mensaje  
- [ ] No — la autenticación solo se comprueba en el handshake, lo que permite que el secuestro de sesión otorgue acceso completo  

### ¿Implementa el servidor limitación de tasa (rate limiting) o restricciones de recursos en el WebSocket?
- [ ] Sí — el Rate Limiting y los tamaños máximos de mensaje están **habilitados** y se aplican  
- [ ] No — no existen límites, lo que permite la Denegación de Servicio (DoS) mediante la inundación de mensajes o tamaños de payload elevados

---

## WSTG-CLNT-11 — Prueba de Web Messaging

Web Messaging, o `postMessage`, permite la comunicación entre orígenes (cross-origin) entre objetos de ventana, como una página principal y una ventana emergente (popup) o un iframe embebido. Este mecanismo es fundamental para la funcionalidad web moderna, pero introduce un riesgo significativo si la aplicación receptora no verifica la identidad del remitente o gestiona incorrectamente el Payload del mensaje. Los atacantes explotan estas vulnerabilidades alojando un sitio malicioso que incrusta la aplicación objetivo y envía mensajes manipulados para activar acciones no deseadas, exfiltrar datos sensibles o ejecutar Cross-Site Scripting (XSS) basado en DOM. La explotación exitosa ocurre cuando los listeners de mensajes no validan estrictamente la propiedad `origin` o pasan `message.data` directamente a sumideros de ejecución (sinks) peligrosos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Herramientas:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### ¿Utiliza la aplicación la API `postMessage` para la comunicación entre documentos?
- [ ] No — los listeners de `postMessage` **no** están presentes en el código del lado del cliente  
- [ ] Sí — se utiliza `postMessage` para la comunicación interna de componentes  
- [ ] Sí — se utiliza `postMessage` para comunicarse con dominios o widgets de terceros  

### ¿Se valida estrictamente el origen de los mensajes entrantes?
- [ ] Sí — el origen se verifica contra una lista blanca (whitelist) estricta utilizando `===` o una comparación idéntica *(Más seguro)*  
- [ ] Sí — el origen se valida mediante una expresión regular (regex), pero la lógica **es** susceptible de derivación (bypass) (ej. `^https://trusted.com`)  
- [ ] No — la propiedad `origin` **no** se verifica, permitiendo que cualquier dominio envíe mensajes  
- [ ] No — se utiliza un comodín `*` en el `targetOrigin` del remitente, lo que podría filtrar datos a listeners maliciosos  

### ¿Se desinfecta (sanitize) el payload del mensaje antes de ser procesado por la aplicación?
- [ ] Sí — los datos se tratan como texto plano y no se utilizan sumideros (sinks) peligrosos  
- [ ] Sí — los datos se analizan (ej. `JSON.parse`) y se validan antes de su uso, y el bypass **no es posible**  
- [ ] No — los datos del mensaje se pasan directamente a sumideros peligrosos como `innerHTML`, `eval()` o `setTimeout()`  
- [ ] No — los datos del mensaje se utilizan para navegar por la ventana o cambiar el `location.href` sin validación  

### ¿Puede un atacante activar acciones no autorizadas o exfiltrar datos mediante un mensaje malicioso?
- [ ] No — incluso con un mensaje suplantado (spoofed), no se puede acceder a acciones ni datos sensibles  
- [ ] Sí — un mensaje manipulado **puede** activar cambios de estado sensibles (ej. cambio de contraseña, actualización de perfil)  
- [ ] Sí — un mensaje manipulado **puede** dar lugar a un XSS basado en DOM o a la exfiltración de tokens CSRF/datos de sesión

---

## WSTG-CLNT-12 — Prueba de Almacenamiento en el Navegador

La prueba del almacenamiento en el navegador implica analizar cómo una aplicación utiliza mecanismos del lado del cliente, tales como LocalStorage, SessionStorage, IndexedDB y WebSQL, para persistir datos. La información sensible almacenada de forma insegura —incluyendo PII, tokens de autenticación o identificadores de sesión— puede verse comprometida si un atacante logra un Cross-Site Scripting (XSS) u obtiene acceso físico al dispositivo del usuario. Los pentesters deben determinar si los datos se almacenan en texto claro, si permanecen accesibles tras el cierre de sesión y si el ciclo de vida del almacenamiento se gestiona adecuadamente para evitar fugas de datos. Desde la perspectiva de un atacante, estos mecanismos de almacenamiento son objetivos valiosos para la exfiltración de información de mantenimiento de estado que permite el secuestro de sesiones (session hijacking) o la persistencia de cuentas a largo plazo.

| Campo | Valor |
|---|---|
| **ID WSTG** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Media / Alta* |

> *La severidad se considera Alta si se almacenan tokens de sesión o PII sensible en LocalStorage y son accesibles mediante XSS.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### ¿Almacena la aplicación información sensible en el almacenamiento del navegador?
- [ ] No — la aplicación **no** utiliza el almacenamiento del navegador o solo almacena estados de la interfaz de usuario (UI) no sensibles.
- [ ] Sí — los datos sensibles se almacenan pero **están cifrados** utilizando criptografía robusta del lado del cliente.
- [ ] Sí — los datos sensibles (tokens, PII, secretos) se almacenan en **texto claro** *(Media)*.

### ¿Son los datos almacenados accesibles para scripts no autorizados (riesgo de XSS)?
- [ ] No — los datos se almacenan en cookies HttpOnly (no en el almacenamiento del navegador) o el almacenamiento **no se utiliza**.
- [ ] Sí — se utiliza LocalStorage/SessionStorage, lo que hace que los datos sean **completamente accesibles** a través de cualquier Payload de XSS *(Alta)*.

### ¿Se limpia adecuadamente el almacenamiento del navegador al cerrar la sesión del usuario?
- [ ] Sí — todo el almacenamiento relacionado con la aplicación se **borra explícitamente** durante el proceso de cierre de sesión (logout).
- [ ] No — el almacenamiento persiste después del cierre de sesión, pero **no** contiene datos de sesión sensibles.
- [ ] No — los tokens de autenticación o PII **persisten** en el almacenamiento después de que la sesión ha finalizado *(Media)*.

### ¿Utiliza la aplicación IndexedDB o WebSQL para conjuntos de datos grandes o sensibles?
- [ ] No — estos mecanismos de almacenamiento **no se utilizan**.
- [ ] Sí — los datos se almacenan y son **saneados adecuadamente** antes de ser renderizados en el DOM.
- [ ] Sí — se almacenan conjuntos de datos sensibles en **texto claro** dentro de las estructuras de IndexedDB/WebSQL.

### ¿Puede manipularse la integridad de los datos almacenados para afectar la lógica de la aplicación?
- [ ] No — la aplicación valida o firma los datos antes de procesarlos desde el almacenamiento.
- [ ] Sí — la modificación de los valores de almacenamiento (p. ej., roles de usuario, flags) **es posible** y resulta en un comportamiento alterado de la aplicación.

---

## WSTG-CLNT-13 — Pruebas de Cross Site Script Inclusion

La inclusión de scripts entre sitios (Cross-Site Script Inclusion - XSSI) ocurre cuando una aplicación web exporta datos sensibles dentro de un archivo JavaScript generado dinámicamente o una respuesta JSONP, que un sitio controlado por un atacante puede incluir mediante una etiqueta `<script>`. Dado que la inclusión de scripts omite la Same-Origin Policy (SOP), un atacante puede ejecutar el script en su propio contexto y sobrescribir objetos o variables globales para exfiltrar la información sensible. Esta vulnerabilidad se manifiesta típicamente en endpoints autenticados que devuelven datos específicos del usuario, como direcciones de correo electrónico, API keys o metadatos de sesión en formato de script. Desde la perspectiva de un atacante, la explotación implica crear una página maliciosa que haga referencia al script vulnerable y capture los cambios resultantes en las variables globales o las llamadas a funciones.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta |

> *La severidad se vuelve Alta si los datos filtrados incluyen tokens CSRF, identificadores de sesión o PII altamente sensible.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### ¿Existen endpoints autenticados que devuelvan JavaScript dinámico o JSONP con información sensible?
- [ ] No — todos los scripts son estáticos y **no pueden** contener datos específicos del usuario  
- [ ] Sí — existen scripts dinámicos pero **no contienen** información sensible  
- [ ] Sí — los scripts dinámicos contienen PII o tokens y **son** accesibles entre distintos orígenes  

### ¿Está implementado el encabezado `X-Content-Type-Options: nosniff` en las respuestas de scripts sensibles?
- [ ] Sí — el encabezado está **aplicado** y evita el sniffing de tipos MIME  
- [ ] No — el encabezado está **ausente**, lo que permite potencialmente que contenido que no es script se ejecute como tal  

### ¿Están los scripts sensibles protegidos por mecanismos anti-XSSI, como prefijos no ejecutables o encabezados personalizados?
- [ ] Sí — los scripts requieren un encabezado personalizado o un token que **no puede** ser enviado mediante una etiqueta `<script>` estándar  
- [ ] Sí — los scripts utilizan prefijos no ejecutables (ej. `)]}'`) que **no pueden** ser omitidos fácilmente  
- [ ] No — los scripts son accesibles directamente mediante peticiones GET utilizando credenciales de ambiente *(Crítico)*  

### ¿Es posible la exfiltración de datos sensibles mediante la sobrescritura de variables globales o prototipos?
- [ ] No — los datos sensibles tienen un alcance (scope) local o están protegidos mediante funciones modernas del navegador  
- [ ] Sí — los datos sensibles se asignan a variables globales y **pueden** ser capturados  
- [ ] Sí — los datos se filtran a través de callbacks de JSONP que **pueden** ser interceptados

---

## WSTG-CLNT-14 — Testing for Reverse Tabnabbing

Reverse Tabnabbing es una vulnerabilidad de lado del cliente en la que una página abierta mediante `target="_blank"` conserva una referencia funcional a la página de origen (página padre) a través del objeto `window.opener`. Esto permite que un sitio de destino controlado por un atacante redirija mediante programación la pestaña original y de confianza a una URL maliciosa, típicamente una página de Phishing diseñada para recolectar credenciales. El ataque explota la confianza inherente del usuario en la pestaña original, ya que es poco probable que note que la página que acaba de dejar ha navegado hacia un dominio diferente. Las prácticas de seguridad modernas requieren el uso de los atributos `rel="noopener"` o `rel="noreferrer"` en las etiquetas de anclaje, o establecer la propiedad `opener` como null en JavaScript, para prevenir esta comunicación entre ventanas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Bajo / Medio* |

> *La severidad es Baja en la mayoría de los navegadores modernos, ya que ahora establecen por defecto `target="_blank"` como `rel="noopener"`. La severidad pasa a ser Media si la aplicación se dirige a navegadores antiguos o utiliza `window.open()` sin establecer la propiedad `opener` como null.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### ¿Existen enlaces que se abran en una nueva ventana o pestaña?
- [ ] No — ningún enlace utiliza `target="_blank"` o una redirección equivalente basada en JavaScript  
- [ ] Sí — `target="_blank"` se utiliza exclusivamente en enlaces internos y de confianza  
- [ ] Sí — `target="_blank"` se utiliza en enlaces externos o en URL proporcionadas por el usuario  

### ¿Está restringida la relación `window.opener` en las etiquetas de anclaje?
- [ ] Sí — `rel="noopener"` o `rel="noreferrer"` se **aplican de forma consistente** en todos los enlaces con `target="_blank"` *(Más seguro)*  
- [ ] Sí — los atributos se aplican pero **faltan** en enlaces de alto riesgo o generados por el usuario  
- [ ] No — los atributos **no se aplican**, lo que permite que la página hija acceda a `window.opener`  

### ¿Es la aplicación vulnerable a Reverse Tabnabbing a través de `window.open()` de JavaScript?
- [ ] No — no se utiliza `window.open()` o se establece explícitamente la propiedad `opener` como null  
- [ ] Sí — se utiliza `window.open()` y la referencia `opener` **permanece accesible** para la ventana hija  

### ¿Puede un atacante controlar el destino de un enlace que se abre en una nueva pestaña?
- [ ] No — todos los destinos de los enlaces están predefinidos (hardcoded) y apuntan a dominios de confianza  
- [ ] Sí — los destinos de los enlaces son proporcionados por el usuario (por ejemplo, perfiles de redes sociales, enlaces de biografía) y **no** están saneados con protecciones contra Tabnabbing

---

## WSTG-APIT-01 — API Reconnaissance

El reconocimiento de API es el proceso sistemático de identificar y mapear la superficie de ataque de la API para descubrir endpoints, métodos soportados y estructuras de datos subyacentes. Los atacantes realizan esta fase para descubrir APIs no documentadas (shadow APIs), versiones obsoletas con vulnerabilidades heredadas (legacy vulnerabilities) y archivos de documentación expuestos, como definiciones de Swagger o OpenAPI, que revelan la lógica de negocio interna. Mediante el análisis de patrones de URL predecibles, archivos JavaScript del lado del cliente y resultados de fuerza bruta de directorios (directory brute-force), un adversario puede construir un mapa completo de la API para identificar objetivos para ataques de Broken Object Level Authorization (BOLA) o Mass Assignment. Este reconocimiento suele dirigirse a rutas comunes como `/api/v1/`, `/swagger.json` o `/graphql` para obtener una hoja de ruta de la funcionalidad del backend de la aplicación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Informativa / Baja |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Herramientas:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### ¿Son accesibles públicamente los archivos de documentación de la API (Swagger, OpenAPI, WSDL)?
- [ ] No — La documentación de la API **no** es accesible o está restringida por autenticación  
- [ ] Sí — La documentación es accesible pero requiere credenciales válidas  
- [ ] Sí — La documentación de la API (p. ej., `/swagger-ui.html`) es **accesible públicamente** sin autenticación  

### ¿Se pueden descubrir endpoints de API no documentados o "shadow" mediante fuzzing?
- [ ] No — El fuzzing de directorios y endpoints no arrojó rutas no documentadas  
- [ ] Sí — Se encontraron endpoints no documentados, pero **no** se puede acceder a ellos sin autorización  
- [ ] Sí — Se encontraron endpoints no documentados y **se puede** acceder a ellos sin una autorización válida  

### ¿Revela la aplicación múltiples versiones de la API (p. ej., /v1/, /v2/, /beta/)?
- [ ] No — Solo es accesible la versión actual y robustecida (hardened) de la API  
- [ ] Sí — Existen versiones heredadas (legacy) pero implementan los **mismos** controles de seguridad que la versión actual  
- [ ] Sí — Las versiones heredadas (p. ej., `/v1/`) son accesibles y **carecen** de los controles de seguridad de las versiones más recientes  

### ¿Los recursos del lado del cliente (JavaScript/Apps móviles) filtran estructuras de endpoints o claves de API?
- [ ] No — El código del lado del cliente **no** contiene rutas de API embebidas (hardcoded) ni claves sensibles  
- [ ] Sí — El código del lado del cliente contiene mapas de endpoints de API pero **no** claves sensibles  
- [ ] Sí — Las claves de API, secretos o URLs de endpoints de uso interno **están** embebidos (hardcoded) en los recursos del lado del cliente  

### ¿Se pueden descubrir parámetros o encabezados ocultos en endpoints conocidos?
- [ ] No — El fuzzing de parámetros (p. ej., mediante `Arjun`) **no** reveló entradas ocultas  
- [ ] Sí — Se descubrieron parámetros ocultos (p. ej., `debug=true`, `admin=1`) pero **no** son funcionales  
- [ ] Sí — Se descubrieron parámetros ocultos y **pueden** utilizarse para alterar el comportamiento de la aplicación

---

## WSTG-APIT-02 — API Broken Object Level Authorization

La Autorización de Objetos a Nivel de Objeto Rota (BOLA), también conocida como Referencia Directa Insegura a Objetos (IDOR), ocurre cuando una API no valida si un usuario tiene los permisos adecuados para acceder o manipular un recurso específico identificado por un ID. Los atacantes explotan esto enumerando sistemáticamente o adivinando identificadores —como IDs numéricos o UUIDs— en las rutas de las solicitudes, parámetros de consulta (query parameters) o cuerpos JSON para acceder a datos pertenecientes a otros usuarios. Esta vulnerabilidad es el problema más común e impactante en la seguridad de las API modernas, lo que puede conducir a una exfiltración masiva de datos, modificaciones no autorizadas o la toma de control total de la cuenta (Account Takeover). Desde la perspectiva de un atacante, el objetivo es identificar endpoints que acepten identificadores de objetos y probar si la lógica de autorización en el lado del servidor está ausente o puede ser evadida manipulando dichos IDs.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Estado de la Prueba** | No realizado |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Herramientas:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### ¿Son los identificadores de objetos predecibles o enumerables dentro de las solicitudes de API?
- [ ] No — los identificadores son largos, aleatorios y criptográficamente seguros (p. ej., UUIDv4)  
- [ ] Sí — los identificadores son enteros secuenciales (p. ej., `101`, `102`)  
- [ ] Sí — los identificadores siguen un patrón predecible o se derivan de información pública  

### ¿Realiza la API una validación en el lado del servidor de la propiedad del objeto para cada solicitud?
- [ ] Sí — se aplican controles de autorización para cada solicitud y la omisión **no es posible**  
- [ ] Sí — se aplican controles pero la omisión **es posible** a través de Parameter Pollution o Method Tunneling  
- [ ] No — la aplicación confía únicamente en que el usuario esté autenticado sin verificar la propiedad del recurso específico  

### ¿Es posible acceder o modificar el recurso de otro usuario cambiando el identificador?
- [ ] No — el acceso no autorizado a los recursos de otros usuarios **no es posible**  
- [ ] Sí — el acceso de **lectura** no autorizado (Horizontal BOLA) **es posible**  
- [ ] Sí — la **modificación** o **eliminación** no autorizada (Horizontal BOLA) **es posible**  

### ¿Permite la API acceder a objetos administrativos o de nivel de sistema sustituyendo los IDs?
- [ ] No — los recursos administrativos están protegidos por capas de autorización secundarias  
- [ ] Sí — el acceso o la modificación de recursos a nivel de sistema (Vertical BOLA) **es posible**  

### ¿Se puede omitir la verificación de autorización moviendo el identificador a una parte diferente de la solicitud?
- [ ] No — la lógica de autorización es consistente independientemente de la ubicación del parámetro  
- [ ] Sí — la omisión **es posible** cuando el ID se mueve de la ruta de la URL al cuerpo JSON o a los encabezados  
- [ ] Sí — la omisión **es posible** cuando se proporcionan múltiples IDs y el servidor procesa el que no está autorizado

---

## WSTG-APIT-99 — Testing GraphQL

GraphQL es un lenguaje de consulta para API que permite a los clientes solicitar estructuras de datos específicas; sin embargo, las malas configuraciones suelen derivar en riesgos de seguridad significativos, incluyendo la divulgación de información y el agotamiento de recursos. Las vulnerabilidades surgen típicamente de la introspección habilitada, la falta de límites de profundidad/complejidad en las consultas y fallos de autorización a nivel de objeto (BOLA) dentro de las funciones de resolución (*resolvers*). Los atacantes explotan estos puntos finales (*endpoints*) utilizando la introspección para mapear todo el esquema (*schema*), aprovechando fragmentos circulares para provocar una Denegación de Servicio (DoS) o evadiendo la autorización al acceder a campos no autorizados mediante consultas anidadas. Las pruebas se centran en los *endpoints* `/graphql`, `/v1/graphql` o `/graphiql` para asegurar que la implementación aplique controles de acceso estrictos y una gestión de recursos adecuada.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica* |

> *La severidad se convierte en Crítica si el Broken Object-Level Authorization (BOLA) permite el acceso no autorizado a Información de Identificación Personal (PII) o mutaciones administrativas.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Herramientas:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### ¿Está habilitado el sistema de introspección de GraphQL?
- [ ] No — la introspección está **deshabilitada** y el esquema no puede ser mapeado  
- [ ] Sí — la introspección está **habilitada** pero requiere autenticación administrativa  
- [ ] Sí — la introspección está **habilitada** y es accesible públicamente *(Alta)*  

### ¿Se aplican límites de recursos (profundidad y complejidad) en las consultas?
- [ ] Sí — se **aplican** y exigen límites estrictos de profundidad y complejidad en las consultas  
- [ ] Sí — se **aplican** límites pero pueden ser evadidos utilizando alias o fragmentos  
- [ ] No — no se **aplican** límites, permitiendo consultas recursivas y DoS  

### ¿Existe Broken Object-Level Authorization (BOLA) en los resolvers?
- [ ] No — la autorización se **aplica** a nivel de campo y objeto para cada consulta  
- [ ] Sí — la autorización se **aplica** a algunos campos pero se exponen objetos sensibles  
- [ ] Sí — la autorización **no se aplica**, permitiendo el acceso a cualquier objeto mediante la manipulación de ID  

### ¿Están las mutaciones de GraphQL debidamente protegidas contra el uso no autorizado?
- [ ] No — las mutaciones **no** están presentes en el esquema  
- [ ] Sí — las mutaciones requieren una autenticación válida y controles de autorización estrictos  
- [ ] Sí — las mutaciones son **posibles** para usuarios no autenticados o carecen de autorización  

### ¿Filtran los mensajes de error detalles sensibles de la implementación o del esquema?
- [ ] No — los mensajes de error son genéricos y **no** revelan la lógica interna  
- [ ] Sí — los mensajes de error revelan tipos de base de datos subyacentes o sugerencias a través de "¿Quiso decir...?" (*Did you mean...?*)  
- [ ] Sí — se **revelan** trazas de pila (*stack traces*) completas y datos de configuración sensibles en las respuestas