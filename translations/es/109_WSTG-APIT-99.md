## WSTG-APIT-99 — Testing GraphQL

GraphQL es un lenguaje de consulta para API que permite a los clientes solicitar estructuras de datos específicas; sin embargo, las malas configuraciones suelen derivar en riesgos de seguridad significativos, incluyendo la divulgación de información y el agotamiento de recursos. Las vulnerabilidades surgen típicamente de la introspección habilitada, la falta de límites de profundidad/complejidad en las consultas y fallos de autorización a nivel de objeto (BOLA) dentro de las funciones de resolución (*resolvers*). Los atacantes explotan estos puntos finales (*endpoints*) utilizando la introspección para mapear todo el esquema (*schema*), aprovechando fragmentos circulares para provocar una Denegación de Servicio (DoS) o evadiendo la autorización al acceder a campos no autorizados mediante consultas anidadas. Las pruebas se centran en los *endpoints* `/graphql`, `/v1/graphql` o `/graphiql` para asegurar que la implementación aplique controles de acceso estrictos y una gestión de recursos adecuada.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Alta / Crítica* |

> *La severidad se convierte en Crítica si el Broken Object-Level Authorization (BOLA) permite el acceso no autorizado a Información de Identificación Personal (PII) o mutaciones administrativas.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Herramientas:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### ¿Está habilitado el sistema de introspección de GraphQL?
- [ ] No — la introspección está **deshabilitada** y el esquema no puede ser mapeado  
- [ ] Sí — la introspección está **habilitada** pero requiere autenticación administrativa  
- [ ] Sí — la introspección está **habilitada** y es accesible públicamente *(Alta)*  

### ¿Se aplican límites de recursos (profundidad y complejidad) en las consultas?
- [ ] Sí — se **aplican** y exigen límites estrictos de profundidad y complejidad en las consultas  
- [ ] Sí — se **aplican** límites pero pueden ser evadidos utilizando alias o fragmentos  
- [ ] No — no se **aplican** límites, permitiendo consultas recursivas y DoS  

### ¿Existe Broken Object-Level Authorization (BOLA) en los resolvers?
- [ ] No — la autorización se **aplica** a nivel de campo y objeto para cada consulta  
- [ ] Sí — la autorización se **aplica** a algunos campos pero se exponen objetos sensibles  
- [ ] Sí — la autorización **no se aplica**, permitiendo el acceso a cualquier objeto mediante la manipulación de ID  

### ¿Están las mutaciones de GraphQL debidamente protegidas contra el uso no autorizado?
- [ ] No — las mutaciones **no** están presentes en el esquema  
- [ ] Sí — las mutaciones requieren una autenticación válida y controles de autorización estrictos  
- [ ] Sí — las mutaciones son **posibles** para usuarios no autenticados o carecen de autorización  

### ¿Filtran los mensajes de error detalles sensibles de la implementación o del esquema?
- [ ] No — los mensajes de error son genéricos y **no** revelan la lógica interna  
- [ ] Sí — los mensajes de error revelan tipos de base de datos subyacentes o sugerencias a través de "¿Quiso decir...?" (*Did you mean...?*)  
- [ ] Sí — se **revelan** trazas de pila (*stack traces*) completas y datos de configuración sensibles en las respuestas  

---