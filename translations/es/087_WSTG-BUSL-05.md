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