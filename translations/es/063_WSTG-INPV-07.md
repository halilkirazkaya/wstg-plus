## WSTG-INPV-07 — XML Injection

La XML Injection ocurre cuando una aplicación incorpora datos proporcionados por el usuario en un documento o flujo XML sin una validación, sanitización o escape de metacaracteres XML adecuados. Al inyectar etiquetas XML específicas o elementos estructurales, un atacante puede manipular la lógica del documento, evadir mecanismos de autenticación o interferir con la integridad de los datos procesados por el backend. Esta vulnerabilidad se encuentra comúnmente en servicios web basados en SOAP, API REST que aceptan XML, aserciones SAML y aplicaciones que generan archivos de configuración XML o reportes de forma dinámica. Desde la perspectiva de un atacante, el objetivo es salir del contexto de datos previsto para alterar la estructura del documento, lo que potencialmente conduce a una escalada de privilegios no autorizada o a la ejecución de una lógica de negocio no deseada.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Estado de la Prueba** | No realizado |
| **Severidad** | Alta / Media |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Herramientas:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### ¿La aplicación acepta y procesa entradas en formato XML provenientes del usuario?
- [ ] No — la aplicación **no** procesa entradas XML  
- [ ] Sí — las entradas XML se procesan a través de APIs (SOAP/REST)  
- [ ] Sí — el XML se procesa mediante la carga de archivos (SVG, DOCX, etc.)  

### ¿Se escapan o sanitizan adecuadamente los metacaracteres XML (ej. `<`, `>`, `&`, `'`, `"`)?
- [ ] Sí — todos los metacaracteres se escapan o sanitizan **adecuadamente** *(Más seguro)*  
- [ ] Sí — se aplica algo de filtrado, pero es **posible** realizar un bypass mediante codificación  
- [ ] No — la entrada sin procesar se incrusta directamente en las estructuras XML **sin** sanitización  

### ¿Se aplica la validación de esquema del lado del servidor (XSD/DTD) para todas las entradas XML?
- [ ] Sí — la validación estricta de esquema está **habilitada** y evita cambios estructurales  
- [ ] Sí — la validación está **habilitada**, pero **no** evita la inyección de etiquetas dentro de los campos de datos  
- [ ] No — la validación de esquema está **deshabilitada** o no se ha implementado  

### ¿Puede un atacante inyectar nuevas etiquetas XML para alterar la estructura o lógica del documento?
- [ ] No — la estructura permanece intacta independientemente de la entrada  
- [ ] Sí — se pueden inyectar etiquetas, pero **no** pueden alterar la lógica de negocio  
- [ ] Sí — la inyección de etiquetas **es posible** y permite la evasión de lógica o la modificación de datos *(Crítico)*  

### ¿Puede el analizador (parser) XML ser manipulado utilizando secciones CDATA o comentarios XML?
- [ ] No — el analizador maneja o rechaza correctamente los marcadores CDATA/comentarios  
- [ ] Sí — la inyección de `<![CDATA[...]]>` o `<!-- ... -->` **es posible** para ocultar o evadir filtros  

---