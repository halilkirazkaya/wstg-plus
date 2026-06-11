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