## WSTG-INPV-06 — Pruebas de LDAP Injection

La LDAP Injection ocurre cuando una aplicación incorpora datos proporcionados por el usuario en filtros LDAP (Lightweight Directory Access Protocol) sin la debida sanitización o escapado, lo que permite a un atacante manipular la lógica de la consulta. Al inyectar caracteres especiales como asteriscos, paréntesis y operadores lógicos, los atacantes pueden modificar el filtro de búsqueda para evadir los mecanismos de autenticación o exfiltrar información sensible del directorio, incluyendo nombres de usuario, membresías de grupos y atributos organizacionales. Esta vulnerabilidad se manifiesta típicamente en funcionalidades como búsquedas en directorios corporativos, portales de empleados o sistemas de Single Sign-On (SSO) que interactúan con Active Directory u OpenLDAP. Desde la perspectiva de un atacante, la explotación exitosa a menudo implica técnicas ciegas (Blind) para enumerar la estructura del directorio o los valores de los atributos bit a bit cuando la salida directa de la consulta es suprimida por la aplicación.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Estado de la prueba** | No realizado |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Herramientas:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### ¿Utiliza la aplicación LDAP para la autenticación o búsquedas en el directorio?
- [ ] No — LDAP **no se utiliza** en la arquitectura de la aplicación  
- [ ] Sí — LDAP se utiliza y todas las entradas están estrictamente sanitizadas o parametrizadas  
- [ ] Sí — LDAP se utiliza y la entrada del usuario se concatena en los filtros **sin** el escapado adecuado  

### ¿Se escapan o filtran adecuadamente los metacaracteres LDAP en la entrada del usuario?
- [ ] Sí — los caracteres como `(`, `)`, `&`, `|`, `*` y `\` **no están permitidos** o se escapan correctamente  
- [ ] No — los metacaracteres **pueden** ser inyectados para alterar la estructura del filtro LDAP  

### ¿Puede el mecanismo de autenticación ser evadido mediante inyección?
- [ ] No — la lógica de inicio de sesión **no es susceptible** a la manipulación de consultas  
- [ ] Sí — la autenticación **puede** ser evadida utilizando la inyección de un OR lógico (`|`) o comodines (`*`) en los campos de nombre de usuario o contraseña  

### ¿Es posible la exfiltración ciega de datos desde el servicio de directorio?
- [ ] No — los resultados de búsqueda son limitados y no se observan diferencias temporales (Timing) o booleanas  
- [ ] Sí — los atributos del directorio **pueden** ser enumerados bit a bit mediante técnicas de Blind Injection basadas en booleanos  
- [ ] Sí — los registros completos del directorio **se ven reflejados** en la respuesta de la aplicación debido a la manipulación de la consulta  

### ¿Son vulnerables los componentes secundarios de la aplicación (ej. sistemas de correo, SSO) a la inyección de atributos basada en LDAP?
- [ ] No — los atributos LDAP se manejan de forma segura en todos los componentes  
- [ ] Sí — un atacante **puede** modificar sus propios atributos (ej. correo electrónico, membresía de grupos) a través de la inyección para escalar privilegios o redireccionar comunicaciones  

---