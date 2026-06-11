## WSTG-APIT-02 — API Broken Object Level Authorization

La Autorización de Objetos a Nivel de Objeto Rota (BOLA), también conocida como Referencia Directa Insegura a Objetos (IDOR), ocurre cuando una API no valida si un usuario tiene los permisos adecuados para acceder o manipular un recurso específico identificado por un ID. Los atacantes explotan esto enumerando sistemáticamente o adivinando identificadores —como IDs numéricos o UUIDs— en las rutas de las solicitudes, parámetros de consulta (query parameters) o cuerpos JSON para acceder a datos pertenecientes a otros usuarios. Esta vulnerabilidad es el problema más común e impactante en la seguridad de las API modernas, lo que puede conducir a una exfiltración masiva de datos, modificaciones no autorizadas o la toma de control total de la cuenta (Account Takeover). Desde la perspectiva de un atacante, el objetivo es identificar endpoints que acepten identificadores de objetos y probar si la lógica de autorización en el lado del servidor está ausente o puede ser evadida manipulando dichos IDs.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Estado de la Prueba** | No realizado |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Herramientas:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### ¿Son los identificadores de objetos predecibles o enumerables dentro de las solicitudes de API?
- [ ] No — los identificadores son largos, aleatorios y criptográficamente seguros (p. ej., UUIDv4)  
- [ ] Sí — los identificadores son enteros secuenciales (p. ej., `101`, `102`)  
- [ ] Sí — los identificadores siguen un patrón predecible o se derivan de información pública  

### ¿Realiza la API una validación en el lado del servidor de la propiedad del objeto para cada solicitud?
- [ ] Sí — se aplican controles de autorización para cada solicitud y la omisión **no es posible**  
- [ ] Sí — se aplican controles pero la omisión **es posible** a través de Parameter Pollution o Method Tunneling  
- [ ] No — la aplicación confía únicamente en que el usuario esté autenticado sin verificar la propiedad del recurso específico  

### ¿Es posible acceder o modificar el recurso de otro usuario cambiando el identificador?
- [ ] No — el acceso no autorizado a los recursos de otros usuarios **no es posible**  
- [ ] Sí — el acceso de **lectura** no autorizado (Horizontal BOLA) **es posible**  
- [ ] Sí — la **modificación** o **eliminación** no autorizada (Horizontal BOLA) **es posible**  

### ¿Permite la API acceder a objetos administrativos o de nivel de sistema sustituyendo los IDs?
- [ ] No — los recursos administrativos están protegidos por capas de autorización secundarias  
- [ ] Sí — el acceso o la modificación de recursos a nivel de sistema (Vertical BOLA) **es posible**  

### ¿Se puede omitir la verificación de autorización moviendo el identificador a una parte diferente de la solicitud?
- [ ] No — la lógica de autorización es consistente independientemente de la ubicación del parámetro  
- [ ] Sí — la omisión **es posible** cuando el ID se mueve de la ruta de la URL al cuerpo JSON o a los encabezados  
- [ ] Sí — la omisión **es posible** cuando se proporcionan múltiples IDs y el servidor procesa el que no está autorizado  

---