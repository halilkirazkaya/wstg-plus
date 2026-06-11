## WSTG-BUSL-04 — Pruebas de Tiempos de Procesamiento

Los ataques de temporización de procesos implican medir el tiempo que una aplicación tarda en procesar solicitudes específicas para inferir información sensible sobre estados internos o la existencia de datos. Al analizar discrepancias a nivel de milisegundos en los tiempos de respuesta —a menudo causadas por lógica condicional de salida anticipada, búsquedas en bases de datos o comparaciones criptográficas que no son de tiempo constante— los atacantes pueden realizar un análisis de canal lateral (side-channel analysis) para enumerar nombres de usuario válidos, verificar fragmentos de contraseñas o confirmar la existencia de registros específicos. Estas vulnerabilidades ocurren típicamente en endpoints de autenticación, formularios de restablecimiento de contraseña y filtros de API con uso intensivo de recursos donde la lógica del backend se ramifica según la validez de la entrada. Desde la perspectiva de un atacante, estas diferencias de tiempo sirven como un "oráculo booleano" que revela verdades sobre los datos del backend, incluso cuando la aplicación devuelve mensajes de error genéricos e idénticos al usuario.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Herramientas:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### ¿Muestra la aplicación tiempos de respuesta variables para solicitudes de recursos válidos frente a inválidos?
- [ ] No — los tiempos de respuesta son idénticos independientemente de la validez de la entrada  
- [ ] Sí — existen diferencias de tiempo insignificantes pero **no** son estadísticamente significativas  
- [ ] Sí — las diferencias de tiempo constantes son **observables** y proporcionan un oráculo confiable  

### ¿Se han implementado funciones de comparación de tiempo constante o retardos artificiales para normalizar los tiempos de respuesta?
- [ ] Sí — se **aplican** algoritmos de tiempo constante a todas las operaciones de comparación sensibles *(Más seguro)*  
- [ ] Sí — se han **habilitado** retardos artificiales o jitter, pero la lógica subyacente sigue sin ser de tiempo constante  
- [ ] No — no se **aplican** mitigaciones de temporización, lo que permite la medición directa de las ramas de la lógica  

### ¿Puede un atacante utilizar el análisis estadístico para aislar la señal de temporización del ruido de la red?
- [ ] No — el jitter de la red y el ruido de la aplicación hacen que el análisis de temporización **no sea posible**  
- [ ] Sí — con un tamaño de muestra suficiente, la señal de temporización **puede** ser aislada del ruido  
- [ ] Sí — el delta de tiempo es lo suficientemente grande como para que **no** se requiera normalización estadística  

### ¿Es posible la enumeración de cuentas a través de la funcionalidad de inicio de sesión o de restablecimiento de contraseña?
- [ ] No — la existencia de la cuenta no se puede determinar mediante la temporización  
- [ ] Sí — las discrepancias de temporización permiten a un atacante remoto **enumerar** nombres de usuario o correos electrónicos válidos  

### ¿Las operaciones con uso intensivo de recursos (p. ej., procesamiento de archivos, búsquedas complejas) filtran la existencia de datos a través de la temporización?
- [ ] No — el consumo de recursos es uniforme independientemente de si se encuentra un registro  
- [ ] Sí — la existencia de registros sensibles **puede** inferirse midiendo la duración del procesamiento en el backend  

---