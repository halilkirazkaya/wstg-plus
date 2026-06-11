## WSTG-BUSL-01 — Pruebas de Validación de Datos de la Lógica de Negocio

Las pruebas de validación de datos de la lógica de negocio se centran en la capacidad de la aplicación para identificar y rechazar datos que son sintácticamente correctos pero lógicamente inválidos dentro del contexto del dominio del negocio. Los atacantes explotan estos fallos enviando valores inesperados —como cantidades negativas en un carrito de compras, fechas pasadas para eventos futuros o enteros extremadamente grandes— para evadir restricciones y manipular el estado de la aplicación. Estas vulnerabilidades suelen residir en flujos de trabajo de varios pasos (multi-step workflows), procesos de pago y módulos de gestión de cuentas donde la lógica del lado del servidor (server-side logic) no logra verificar la integridad contextual de los datos proporcionados por el usuario. Una explotación exitosa puede derivar en pérdidas financieras, compromiso de la integridad y acceso no autorizado a funciones o datos restringidos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Herramientas:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### ¿Aplica la aplicación límites de rango lógico en las entradas numéricas?
- [ ] Sí — la aplicación rechaza correctamente valores negativos, cero o excesivamente grandes cuando no son apropiados *(Más seguro)*  
- [ ] Sí — la aplicación rechaza algunos rangos inválidos pero **es** vulnerable a desbordamiento de enteros (integer overflow) o evasión de límites (boundary bypass)  
- [ ] No — la aplicación acepta cualquier valor numérico independientemente del contexto de negocio *(Crítico)*  

### ¿Son consistentes las comprobaciones de validación del lado del servidor en todas las capas de la aplicación?
- [ ] Sí — la validación se aplica de manera consistente tanto en las capas de API/servicio como en la base de datos  
- [ ] Sí — la validación **se aplica** en la interfaz de usuario (UI/Frontend), pero es posible realizar una evasión mediante una solicitud directa a la API  
- [ ] No — la validación **no se aplica** o es inconsistente, lo que permite la corrupción lógica de los datos  

### ¿Puede subvertirse la lógica de negocio manipulando datos en flujos de trabajo de varios pasos (multi-step workflows)?
- [ ] No — el estado se mantiene de forma segura en el servidor y los datos de los pasos anteriores **no pueden** ser alterados  
- [ ] Sí — la validación ocurre en los primeros pasos, pero el envío final **no vuelve a verificar** la integridad de los datos  
- [ ] Sí — la evasión del flujo de trabajo **es posible** saltando pasos o enviando datos fuera de orden  

### ¿Maneja la aplicación adecuadamente los datos de "casos extremos" (edge cases) que evaden las restricciones de negocio?
- [ ] Sí — las restricciones son robustas y manejan entradas inusuales pero sintácticamente válidas (p. ej., años bisiestos, cambios de zona horaria)  
- [ ] Sí — se manejan algunos casos extremos, pero combinaciones específicas **pueden** llevar a un comportamiento inesperado  
- [ ] No — los casos extremos conducen directamente a fallos de lógica, como compras gratuitas o cambios de estado no autorizados  

---