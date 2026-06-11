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