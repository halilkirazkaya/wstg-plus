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