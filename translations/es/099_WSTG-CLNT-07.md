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