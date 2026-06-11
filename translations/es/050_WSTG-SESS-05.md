## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) es una vulnerabilidad en la que un atacante engaña al navegador de una víctima para que realice una acción no deseada en un sitio web diferente en el que la víctima está autenticada actualmente. Este exploit aprovecha el comportamiento del navegador de adjuntar automáticamente credenciales "ambientales", como cookies de sesión o encabezados de Authorization, a las solicitudes salientes. Los atacantes suelen dirigirse a operaciones que cambian el estado, como el cambio de contraseñas, la actualización de direcciones de correo electrónico o la ejecución de transferencias financieras, alojando scripts maliciosos o formularios ocultos en un sitio de terceros. Una explotación exitosa puede llevar al secuestro total de la cuenta (account takeover) o a la modificación de datos no autorizada sin el conocimiento o consentimiento del usuario.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Estado de la Prueba** | No Realizada |
| **Gravedad** | Media / Alta* |

> *La gravedad pasa a ser Alta si la acción vulnerable permite el secuestro de la cuenta, la escalada de privilegios o transacciones financieras no autorizadas.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Herramientas:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### ¿Se han implementado tokens anti-CSRF para las solicitudes que cambian el estado?
- [ ] Sí — se requieren tokens únicos y criptográficamente fuertes para todas las acciones que cambian el estado  
- [ ] Sí — los tokens están presentes pero **no** son únicos por sesión o son predecibles  
- [ ] No — **no** se han implementado tokens anti-CSRF  

### ¿Es robusta la validación del token anti-CSRF en el lado del servidor?
- [ ] Sí — el servidor valida estrictamente el token y **no es posible** realizar un bypass  
- [ ] Sí — la validación se realiza pero **se puede** evadir eliminando el parámetro del token  
- [ ] Sí — la validación se realiza pero **se puede** evadir proporcionando un token en blanco o ficticio  
- [ ] Sí — la validación se realiza pero el token **no** está vinculado a la sesión del usuario  

### ¿Depende la aplicación de métodos fácilmente evadibles para la protección contra CSRF?
- [ ] No — la aplicación no depende únicamente de encabezados débiles o comprobaciones de origen  
- [ ] Sí — la protección depende únicamente del encabezado `Referer` u `Origin`, el cual **puede** ser falsificado o eliminado  
- [ ] Sí — la protección depende de la comprobación del encabezado `X-Requested-With`, el cual **puede** ser evadido mediante configuraciones incorrectas de CORS  

### ¿Están configuradas las cookies de sesión con el atributo `SameSite`?
- [ ] Sí — `SameSite` está configurado como `Strict` o `Lax` en todas las cookies relacionadas con la sesión  
- [ ] No — el atributo `SameSite` **está ausente**, delegando en el comportamiento predeterminado del navegador  
- [ ] No — `SameSite` está configurado explícitamente como `None` sin el flag `Secure` o mitigaciones adicionales  

### ¿Se puede evadir la protección CSRF cambiando el método de solicitud HTTP?
- [ ] No — la protección se aplica independientemente del método HTTP utilizado  
- [ ] Sí — los tokens solo se validan en solicitudes `POST`, pero la acción **se puede** realizar a través de `GET`  
- [ ] Sí — cambiar el método (por ejemplo, a `PUT` o `DELETE`) evade la comprobación del token  

---