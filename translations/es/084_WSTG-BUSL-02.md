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