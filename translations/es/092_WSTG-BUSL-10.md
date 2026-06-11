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