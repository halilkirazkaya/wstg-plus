## WSTG-INPV-17 — Testing for Host Header Injection

La inyección de cabecera Host (Host Header Injection) ocurre cuando una aplicación confía en la cabecera `Host` proporcionada por el usuario e la incorpora en la lógica del lado del servidor, como en la generación de enlaces o redirecciones, sin una validación suficiente. Los atacantes manipulan esta cabecera para facilitar el envenenamiento de caché web (Web Cache Poisoning), el envenenamiento del restablecimiento de contraseña (Password Reset Poisoning) mediante la redirección de tokens sensibles a un dominio externo, o para evadir controles de seguridad que dependen del enrutamiento basado en cabeceras. Una explotación exitosa permite a un atacante exfiltrar datos de sesión, realizar tomas de control de cuentas (Account Takeover) o redireccionar a usuarios desprevenidos hacia una infraestructura maliciosa. Esta vulnerabilidad se encuentra comúnmente en entornos con balanceo de carga o en aplicaciones que generan dinámicamente URLs absolutas basadas en el contexto de la solicitud entrante.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Estado de la prueba** | No realizada |
| **Severidad** | Media / Alta* |

> *La severidad aumenta a Alta si la cabecera Host se utiliza para generar enlaces sensibles en correos electrónicos, como en el caso del restablecimiento de contraseñas.

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Herramientas:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### ¿Refleja la aplicación el valor de la cabecera `Host` en enlaces, redirecciones o scripts?
- [ ] No — la aplicación utiliza URLs base estáticas (hardcoded) o una configuración estrictamente validada.
- [ ] Sí — la cabecera `Host` se refleja pero se valida estrictamente contra una lista blanca (Whitelist).
- [ ] Sí — la cabecera `Host` se refleja y es posible realizar un Exploit o bypass mediante valores malformados (ej. `expected.com:attacker.com`).
- [ ] Sí — la cabecera `Host` se refleja **sin** ningún tipo de validación.

### ¿Se procesan cabeceras alternativas relacionadas con el host por parte de la aplicación o la infraestructura?
- [ ] No — las cabeceras como `X-Forwarded-Host`, `X-Host` o `Forwarded` son ignoradas.
- [ ] Sí — se procesan cabeceras alternativas pero **no** anulan la lógica interna.
- [ ] Sí — las cabeceras alternativas **pueden** utilizarse para anular el valor de la cabecera `Host` primaria.

### ¿Puede manipularse la cabecera `Host` para envenenar correos electrónicos generados por el sistema (ej. restablecimiento de contraseña)?
- [ ] No — los enlaces de correo electrónico utilizan un dominio estático y confiable de la configuración del servidor.
- [ ] Sí — la cabecera `Host` influye en los enlaces de correo electrónico pero la validación **no** puede ser evadida.
- [ ] Sí — la cabecera `Host` **puede** manipularse para redireccionar tokens de restablecimiento de contraseña a un dominio controlado por el atacante *(Crítico)*.

### ¿Es la aplicación susceptible a Web Cache Poisoning a través de la cabecera `Host`?
- [ ] No — no hay un mecanismo de caché presente o la cabecera `Host` es parte de la clave de caché (Cache Key).
- [ ] Sí — existe una caché pero valida estrictamente o ignora la cabecera `Host` para entradas sin clave (unkeyed inputs).
- [ ] Sí — existe una caché y **puede** ser envenenada mediante una cabecera `Host` inyectada para servir contenido malicioso a otros usuarios.

### ¿Acepta el servidor cabeceras `Host` arbitrarias para el enrutamiento de hosts virtuales (Virtual Host)?
- [ ] No — el servidor devuelve un error o una página por defecto para cabeceras `Host` no reconocidas.
- [ ] Sí — el servidor acepta cualquier cabecera `Host`, lo cual **es** útil para el reconocimiento o la enumeración de servicios internos.

---