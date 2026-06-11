## WSTG-SESS-01 — Pruebas del Esquema de Gestión de Sesiones

Las pruebas del esquema de gestión de sesiones se centran en analizar cómo la aplicación maneja el ciclo de vida y la estructura de los identificadores de sesión para garantizar que sean robustos frente a la predicción y el secuestro (hijacking). Los atacantes capturan múltiples tokens de sesión para realizar un análisis estadístico, buscando patrones, baja entropía o incrementos predecibles que permitan falsificar sesiones válidas para otros usuarios. Las debilidades en el esquema a menudo se manifiestan como vulnerabilidades de Session Fixation o el uso de valores insuficientemente aleatorios, que normalmente se encuentran en cookies HTTP, parámetros URL o implementaciones de encabezados personalizados. Comprometer el esquema de sesión es un ataque de alto impacto que otorga al adversario acceso completo al estado autenticado de una víctima sin necesidad de sus credenciales principales.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Herramientas:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### ¿Es el identificador de sesión suficientemente complejo y aleatorio?
- [ ] Sí — el token supera las pruebas estadísticas de aleatoriedad (alta entropía) y el bypass **no es posible**  
- [ ] Sí — el token es largo pero contiene patrones predecibles o segmentos estáticos  
- [ ] No — el token es secuencial, basado en marcas de tiempo (timestamps) predecibles, o tiene una entropía **críticamente baja** *(Crítico)*  

### ¿Dónde se transmite y almacena el identificador de sesión?
- [ ] El identificador se almacena en una cookie `Secure` y `HttpOnly` *(Más seguro)*  
- [ ] El identificador se almacena en una cookie pero carece de los flags `Secure` o `HttpOnly`  
- [ ] El identificador se transmite **únicamente** a través de parámetros URL o peticiones `GET` *(Riesgo Alto)*  

### ¿Emite la aplicación un nuevo identificador de sesión tras una autenticación exitosa?
- [ ] Sí — **se genera** un nuevo ID de sesión único inmediatamente después del login  
- [ ] No — el ID de sesión sigue siendo el mismo antes y después del login *(Posible Session Fixation)*  

### ¿Está el identificador de sesión vinculado a una dirección IP o User-Agent específicos para evitar la reutilización?
- [ ] Sí — la sesión se invalida si la huella digital (fingerprint) del cliente cambia significativamente  
- [ ] No — la sesión **puede** ser reutilizada en diferentes direcciones IP o dispositivos sin verificación adicional  

### ¿Se invalidan correctamente los identificadores de sesión en el lado del servidor tras el cierre de sesión o el tiempo de espera (timeout)?
- [ ] Sí — el estado de la sesión en el lado del servidor se **destruye completamente** al cerrar la sesión  
- [ ] No — la sesión permanece válida en el servidor incluso si se elimina la cookie del lado del cliente  
- [ ] No — la sesión **nunca expira** o tiene un periodo de timeout excesivamente largo  

---