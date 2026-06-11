## WSTG-ATHZ-04 — Pruebas de Referencias Directas Inseguras a Objetos (Insecure Direct Object References)

Las Referencias Directas Inseguras a Objetos (IDOR) ocurren cuando una aplicación proporciona acceso directo a objetos basándose en la entrada suministrada por el usuario sin realizar una comprobación de autorización para verificar los permisos del solicitante. Esta vulnerabilidad impacta significativamente la confidencialidad e integridad, ya que permite a los atacantes visualizar, modificar o eliminar datos pertenecientes a otros usuarios o al sistema. Normalmente se manifiesta en parámetros de URL, datos del cuerpo POST o claves JSON que hacen referencia a claves internas de la base de datos, nombres de archivos o identificadores de cuenta. Desde la perspectiva de un atacante, la explotación implica manipular estos identificadores —a menudo mediante el incremento de enteros o fuzzing de UUID— para acceder a recursos fuera de su ámbito autorizado.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Alta |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Herramientas:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### ¿Utiliza la aplicación identificadores directos de objetos en los parámetros de la solicitud?
- [ ] No — la aplicación utiliza referencias indirectas o mapas cifrados *(Más seguro)*  
- [ ] Sí — la aplicación utiliza identificadores no predecibles (ej. UUID largos/hashes)  
- [ ] Sí — la aplicación utiliza identificadores predecibles (ej. enteros incrementales o nombres de usuario)  

### ¿Se realiza la autorización en el lado del servidor para cada solicitud que involucre un identificador de objeto?
- [ ] Sí — los controles en el lado del servidor **no pueden** ser omitidos para ningún identificador  
- [ ] Sí — existen controles en el lado del servidor, pero su omisión (bypass) **es posible** mediante Parameter Pollution o manipulación de encabezados  
- [ ] No — no **se aplica** ninguna comprobación de autorización para verificar la propiedad del objeto solicitado  

### ¿Puede un atacante acceder o modificar objetos pertenecientes a otros usuarios (Horizontal IDOR)?
- [ ] No — el acceso a los datos de otros usuarios **no es posible**  
- [ ] Sí — la visualización de los datos de otros usuarios **es posible**, pero la modificación **no es posible**  
- [ ] Sí — tanto la visualización como la modificación de los datos de otros usuarios **son posibles** *(Crítico)*  

### ¿Es posible acceder a objetos de nivel administrativo o de sistema (Vertical IDOR)?
- [ ] No — el acceso a objetos con privilegios superiores **no es posible**  
- [ ] Sí — el acceso a configuraciones administrativas o archivos del sistema **es posible**  

### ¿Filtra la aplicación identificadores de objetos a través de búsquedas, metadatos u otros endpoints?
- [ ] No — los identificadores **no se exponen** involuntariamente  
- [ ] Sí — los identificadores de otros usuarios/objetos **pueden** ser enumerados a través de endpoints secundarios o perfiles públicos  

---