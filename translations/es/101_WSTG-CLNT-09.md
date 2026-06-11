## WSTG-CLNT-09 — Pruebas de Clickjacking

El Clickjacking, también conocido como UI redressing, ocurre cuando un atacante utiliza capas transparentes u opacas para engañar a un usuario para que haga clic en un botón o enlace en otra página cuando su intención era hacer clic en la página de nivel superior. Esta vulnerabilidad compromete la integridad de las acciones del usuario, lo que potencialmente conduce a la modificación de datos no autorizada, cambios en la cuenta o transferencias financieras realizadas en nombre de la víctima. Los atacantes suelen explotar esto incrustando el sitio web objetivo dentro de un `<iframe>` en un sitio malicioso controlado y superponiéndolo con contenido engañoso o señuelos atractivos. Ocurre con mayor frecuencia en páginas que realizan acciones sensibles que cambian el estado, como botones de "Eliminar cuenta", formularios de restablecimiento de contraseña o paneles de configuración administrativa.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Estado de la Prueba** | No Realizado |
| **Severidad** | Media / Alta* |

> *La severidad se convierte en Alta si se pueden activar acciones sensibles (p. ej., transferencias de fondos, eliminación de cuentas o escalada de privilegios) a través de clickjacking.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Herramientas:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### ¿Implementa la aplicación cabeceras del lado del servidor para prevenir el framing no autorizado?
- [ ] Sí — Se aplican `X-Frame-Options` o `Content-Security-Policy` y previenen el framing *(Más seguro)*  
- [ ] Sí — Las cabeceras están presentes pero **mal configuradas** (p. ej., `X-Frame-Options: ALLOW-FROM` que carece de un amplio soporte en los navegadores)  
- [ ] No — No se aplican cabeceras de protección contra framing  

### ¿Está implementada la directiva `frame-ancestors` de `Content-Security-Policy` (CSP)?
- [ ] Sí — Se aplica `frame-ancestors 'none'` o `'self'`  
- [ ] Sí — `frame-ancestors` está presente pero **permite** orígenes no confiables o excesivamente amplios  
- [ ] No — La directiva **no existe** o no se ha implementado CSP  

### ¿Está la cabecera `X-Frame-Options` (XFO) configurada correctamente para la compatibilidad con navegadores antiguos?
- [ ] Sí — Se aplica `DENY` o `SAMEORIGIN`  
- [ ] Sí — XFO está presente pero es **ignorada** por los navegadores modernos porque existe una directiva `frame-ancestors` de CSP  
- [ ] No — La cabecera XFO **no existe** o se ha establecido en un valor inseguro  

### ¿Pueden omitirse las protecciones de framing utilizando técnicas conocidas?
- [ ] No — La omisión (bypass) **no es posible** mediante técnicas estándar (double-framing, etc.)  
- [ ] Sí — La protección **puede** omitirse debido a una expresión regular (regex) o lógica débil en la lista blanca de `frame-ancestors`  
- [ ] Sí — La protección **puede** omitirse porque la aplicación depende exclusivamente de técnicas de frame-busting basadas en JavaScript  

### ¿Es explotable una acción sensible a través de una prueba de concepto de clickjacking?
- [ ] No — El framing está bloqueado o no se puede acceder a acciones sensibles  
- [ ] Sí — El UI redressing **es posible** pero requiere una interacción compleja del usuario  
- [ ] Sí — El UI redressing **es posible** y permite acciones inmediatas que cambian el estado *(Crítico)*  

---