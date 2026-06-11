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