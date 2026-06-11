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