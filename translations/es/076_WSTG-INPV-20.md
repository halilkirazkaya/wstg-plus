## WSTG-INPV-20 — Mass Assignment

Mass Assignment, también conocido como Overposting o Insecure Deserialization de parámetros, ocurre cuando una aplicación web vincula automáticamente los parámetros de una solicitud HTTP entrante con las propiedades de objetos internos sin un filtrado de atributos adecuado. Esta vulnerabilidad es prevalente en los frameworks MVC modernos y APIs RESTful donde los datos en formato JSON, XML o codificados en formularios se deserializan directamente en los modelos de datos del lado de la aplicación. Los atacantes explotan este comportamiento inyectando parámetros adicionales no listados en una solicitud —tales como `isAdmin`, `role` o `account_balance`— para modificar campos sensibles que los desarrolladores no pretendían exponer al control del usuario. Una explotación exitosa generalmente resulta en una escalada de privilegios, modificación no autorizada de datos o el bypass de flujos críticos de lógica de negocio.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta / Media* |

> *La severidad aumenta a Alta si se pueden modificar atributos administrativos, niveles de permisos o campos de facturación.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### ¿Utiliza la aplicación la vinculación automática para los parámetros de las solicitudes entrantes?
- [ ] No — los parámetros se mapean manualmente a variables específicas u objetos seguros  
- [ ] Sí — se utiliza la vinculación automática pero se aplican estrictas **listas blancas** (white-lists) o DTOs (Data Transfer Objects)  
- [ ] Sí — se utiliza la vinculación automática y los atributos internos sensibles **pueden** ser modificados  

### ¿Es posible inyectar con éxito atributos administrativos o relacionados con permisos?
- [ ] No — los parámetros inyectados son ignorados, eliminados o provocan un error de validación  
- [ ] Sí — se aceptan parámetros adicionales pero **no** afectan al estado del objeto en el backend  
- [ ] Sí — la inyección de parámetros como `is_admin`, `role` o `permissions` escala privilegios **exitosamente** *(Crítico)*  

### ¿Es posible modificar la lógica de negocio sensible o campos financieros mediante overposting?
- [ ] No — los campos como `price`, `balance` o `status` están protegidos contra Mass Assignment  
- [ ] Sí — los atributos sensibles del negocio **pueden** ser modificados al añadirlos al cuerpo de la solicitud JSON o del formulario  

### ¿Revela la aplicación estructuras de objetos internos que faciliten la adivinación de parámetros?
- [ ] No — las respuestas solo incluyen los campos públicos previstos y la documentación está restringida  
- [ ] Sí — los campos de los objetos internos son visibles en las respuestas GET, lo que hace **posible** el descubrimiento de parámetros  
- [ ] Sí — la documentación de la API (Swagger/OpenAPI) enumera explícitamente propiedades internas que **pueden** ser asignadas  

---