## WSTG-CLNT-05 — Testing for CSS Injection

La inyección CSS (CSS Injection) ocurre cuando una aplicación permite que la entrada proporcionada por el usuario influya en las hojas de estilo en cascada (Cascading Style Sheets - CSS) de una página web sin un saneamiento o escape adecuado. Aunque a menudo se percibe como un problema cosmético, los atacantes pueden aprovechar la inyección CSS para exfiltrar datos sensibles, como tokens CSRF o IDs de sesión, mediante el uso de selectores de atributos y propiedades de imagen de fondo (`background-image`) para activar solicitudes externas. Esta vulnerabilidad ocurre típicamente en puntos finales (endpoints) donde las preferencias del usuario (por ejemplo, temas personalizados, colores de fuente) se reflejan dentro de bloques `<style>` o atributos de estilo en línea (`style`). Desde la perspectiva de un atacante, una explotación exitosa puede conducir a la redistribución de la interfaz de usuario (UI Redressing), phishing mediante la modificación no autorizada del diseño, o la exfiltración sigilosa de datos en entornos donde las políticas de seguridad de contenido (Content Security Policies - CSP) estrictas podrían bloquear ataques basados en JavaScript.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Estado de la Prueba** | No Realizada |
| **Severidad** | Media / Alta* |

> *La severidad aumenta a Alta si el punto de inyección permite la exfiltración de atributos sensibles como tokens CSRF o si puede utilizarse para UI Redressing en flujos de trabajo sensibles.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### ¿Refleja la aplicación la entrada controlada por el usuario dentro de contextos CSS?
- [ ] No — la entrada **no** se refleja en etiquetas de estilo, atributos o archivos CSS  
- [ ] Sí — la entrada se refleja pero está estrictamente limitada a valores alfanuméricos seguros  
- [ ] Sí — la entrada se refleja dentro de un atributo `style` o un bloque `<style>`  

### ¿Están correctamente saneados los metacaracteres y funciones específicos de CSS?
- [ ] Sí — los caracteres como `{`, `}`, `:` y funciones como `url()` están **correctamente escapados**  
- [ ] Sí — el saneamiento está implementado pero es posible **evadirlo (bypass)** mediante codificación o comentarios CSS  
- [ ] No — no se aplica ningún saneamiento a los metacaracteres CSS  

### ¿Es posible la exfiltración de datos utilizando selectores CSS?
- [ ] No — los selectores **no pueden** activar solicitudes externas ni filtrar datos  
- [ ] Sí — la exfiltración parcial de datos **es posible** mediante selectores de atributos y `background-image`  
- [ ] Sí — la exfiltración completa de tokens sensibles **es posible** mediante técnicas de fuerza bruta (Brute Force) automatizadas  

### ¿Mitiga una Política de Seguridad de Contenido (CSP) el riesgo de inyección CSS?
- [ ] Sí — `style-src` está restringido a 'self' e `img-src` o `connect-src` **está restringido**  
- [ ] Sí — la CSP está presente pero utiliza 'unsafe-inline' o permite dominios externos con comodines (wildcards)  
- [ ] No — no se ha implementado ninguna CSP para evitar la carga de recursos externos a través de CSS  

### ¿Puede utilizarse la vulnerabilidad para realizar UI Redressing o phishing?
- [ ] No — el alcance de la inyección es demasiado limitado para modificar el diseño de manera significativa  
- [ ] Sí — la modificación de la interfaz de usuario (UI modification) **es posible**, permitiendo la superposición de elementos maliciosos  
- [ ] Sí — el control total del diseño de la página **es posible**, facilitando ataques de phishing altamente convincentes  

---