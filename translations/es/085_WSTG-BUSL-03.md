## WSTG-BUSL-03 — Pruebas de comprobación de integridad

Las pruebas de comprobación de integridad se centran en verificar que la aplicación garantice que los datos permanezcan inalterados y sean confiables a medida que se mueven a través de diversos procesos de negocio o entre el cliente y el servidor. Los atacantes se dirigen a esta lógica manipulando parámetros críticos —como precios de productos, cantidades, privilegios de usuario o estados de transacciones— dentro de peticiones HTTP, campos ocultos o cookies. Si la aplicación carece de verificación en el lado del servidor o de controles de integridad robustos como Hashing o HMACs, un adversario puede manipular estos valores para cometer fraude, escalar sus propios privilegios o eludir las restricciones de negocio previstas. Esta prueba es vital para cualquier aplicación que maneje transacciones financieras, transiciones de estado sensibles o flujos de trabajo de varios pasos donde la salida de una etapa sirve como la entrada confiable para la siguiente.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad pasa a ser Alta si la falla de integridad permite el robo financiero, el escalamiento de privilegios no autorizado o la modificación de registros persistentes.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### ¿Están los parámetros sensibles del negocio expuestos a la manipulación en el lado del cliente?
- [ ] No — los valores sensibles se gestionan exclusivamente en el lado del servidor  
- [ ] Sí — valores como el precio, la cantidad o los indicadores de estado **están** expuestos pero cifrados  
- [ ] Sí — los valores **están** expuestos en texto plano dentro de campos ocultos, parámetros de URL o cookies  

### ¿Se aplica un mecanismo de integridad (p. ej., HMAC, Checksum) a los parámetros controlados por el cliente?
- [ ] Sí — se **aplica** y verifica un control de integridad robusto en cada petición  
- [ ] Sí — se **aplica** un control de integridad pero utiliza un algoritmo débil/predecible o un secreto embebido (hardcoded)  
- [ ] No — no se **aplican** controles de integridad a los parámetros sensibles  

### ¿Puede el control de integridad ser eludido o recalculado por un atacante?
- [ ] No — la verificación de la firma se aplica estrictamente y el bypass **no es posible**  
- [ ] Sí — la verificación de la firma puede ser eludida omitiendo el parámetro de la firma  
- [ ] Sí — la firma **puede** ser recalculada porque la clave secreta o la lógica está expuesta/es débil  

### ¿Realiza la aplicación una re-validación de los datos en el backend contra una fuente confiable?
- [ ] Sí — el servidor re-valida todos los valores contra la base de datos y el bypass **no es posible**  
- [ ] Sí — el servidor realiza una validación parcial pero el bypass **es posible** a través de casos de borde específicos  
- [ ] No — el servidor confía en los datos proporcionados por el cliente sin verificación en el backend *(Crítico)*  

### ¿Es posible manipular el estado de un proceso de varios pasos?
- [ ] No — el estado de la sesión se gestiona en el lado del servidor y no puede ser manipulado  
- [ ] Sí — la información de estado se transmite a través del cliente y la manipulación **es posible** para saltar pasos o repetir acciones  

---