## WSTG-INPV-05 — SQL Injection

La SQL Injection ocurre cuando la entrada suministrada por el usuario se incorpora en consultas SQL sin la debida parametrización o sanitización, lo que permite a los atacantes manipular la lógica de la consulta. Una explotación exitosa puede derivar en la exfiltración no autorizada de datos, bypass de autenticación, modificación de datos y, en algunos casos, ejecución remota de código a través de funciones de la base de datos como `xp_cmdshell` o `LOAD_FILE()`. Esta vulnerabilidad afecta comúnmente a formularios de inicio de sesión, funcionalidades de búsqueda, parámetros de REST API, cabeceras HTTP y cualquier endpoint que construya consultas SQL dinámicas a partir de entradas controladas por el usuario. Desde la perspectiva de un atacante, la identificación de un punto de inyección es la puerta de entrada principal para el compromiso total de la base de datos y el potencial movimiento lateral dentro de la infraestructura.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Herramientas:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### ¿Se procesan los parámetros controlables por el usuario mediante métodos seguros de acceso a la base de datos?
- [ ] Sí — la aplicación utiliza exclusivamente ORM o consultas parametrizadas y el bypass **no es posible**  
- [ ] Sí — se utilizan consultas parametrizadas pero el bypass **es posible** a través de casos de borde (ej., cláusulas `ORDER BY`)  
- [ ] No — se utiliza concatenación directa de cadenas para construir consultas SQL **sin** parametrización  

### ¿Puede omitirse el mecanismo de autenticación a través de SQL Injection?
- [ ] No — los parámetros de inicio de sesión **no** son vulnerables a la inyección  
- [ ] Sí — el bypass de autenticación **es posible** utilizando payloads basados en tautologías (ej., `' OR 1=1 --`)  

### ¿Es posible la exfiltración de datos mediante técnicas in-band o out-of-band?
- [ ] No — no se ha identificado exfiltración de datos  
- [ ] Sí — la exfiltración basada en UNION o en errores **es posible**  
- [ ] Sí — la exfiltración blind (basada en booleanos o tiempo) **es posible**  
- [ ] Sí — la exfiltración out-of-band (DNS/HTTP) **es posible**  

### ¿Presentan las funciones de la aplicación vulnerabilidades de SQL Injection de segundo orden?
- [ ] No — los datos almacenados se manejan de forma segura cuando se recuperan para consultas posteriores  
- [ ] Sí — la entrada del usuario se almacena y se utiliza posteriormente en una consulta SQL insegura **sin** validación  

### ¿Están los privilegios de la cuenta de servicio de la base de datos restringidos al mínimo necesario?
- [ ] Sí — la cuenta de servicio tiene el **mínimo privilegio** (ej., limitada a tablas/acciones específicas)  
- [ ] No — la cuenta de servicio tiene privilegios elevados (ej., privilegios de `DROP TABLE`, `FILE`, o `xp_cmdshell` **habilitado**)  

---