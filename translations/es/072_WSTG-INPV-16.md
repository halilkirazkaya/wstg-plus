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