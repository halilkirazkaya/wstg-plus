## WSTG-INPV-13 — Format String Injection

La inyección de cadenas de formato (Format String Injection) ocurre cuando una aplicación web pasa una entrada controlada por el usuario directamente al argumento de cadena de formato de una función variádica, como la familia `printf` de C o envoltorios (wrappers) de registro de bajo nivel. Aunque es menos común en los frameworks web modernos de código administrado, esta vulnerabilidad sigue siendo crítica en aplicaciones CGI heredadas (legacy), extensiones nativas o sistemas de registro en casos de borde que interactúan con librerías de sistema de bajo nivel. Los atacantes explotan (exploit) esto proporcionando especificadores de formato como `%x` o `%p` para filtrar direcciones de memoria sensibles y datos de la pila (stack), o el especificador `%n` para realizar escrituras arbitrarias en memoria. Una explotación exitosa generalmente resulta en el compromiso completo del sistema a través de la ejecución remota de código (Remote Code Execution - RCE) o, como mínimo, una condición de denegación de servicio (Denial-of-Service - DoS) impactante mediante la caída de procesos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Estado de la Prueba** | No Realizado |
| **Severidad** | Alta / Crítica |

**Referencias:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Herramientas:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### ¿Procesa la aplicación entradas de usuario a través de código nativo o interfaces CGI heredadas?
- [ ] No — la aplicación está construida íntegramente en frameworks de código administrado (p. ej., JVM, CLR)  
- [ ] Sí — la aplicación utiliza JNI, extensiones nativas C/C++ o CGIs binarios heredados donde **pueden** existir vulnerabilidades de Format String Injection  

### ¿Se pasan las entradas proporcionadas por el usuario como el argumento principal de funciones sensibles al formato?
- [ ] No — las entradas se pasan como argumentos de datos, y las cadenas de formato son **estáticas** y están **hardcoded**  
- [ ] Sí — la entrada del usuario se concatena directamente en el argumento de la cadena de formato, pero **se aplica** sanitización  
- [ ] Sí — la entrada del usuario se utiliza directamente como cadena de formato y **no se aplica** sanitización  

### ¿Es posible filtrar contenidos de la memoria utilizando especificadores de formato?
- [ ] No — la entrada es validada y los especificadores como `%p`, `%x` o `%s` **no son procesados**  
- [ ] Sí — el envío de especificadores `%p` o `%x` resulta en el reflejo de direcciones de memoria del stack o heap  
- [ ] Sí — los datos sensibles (p. ej., punteros, stack cookies) **pueden** ser exfiltrados a través de especificadores de formato  

### ¿Se puede provocar la caída de la aplicación o manipularla utilizando el especificador `%n`?
- [ ] No — los especificadores que permiten escrituras en memoria están **deshabilitados** o bloqueados por el compilador/entorno  
- [ ] Sí — la aplicación se cae (DoS) cuando se proporcionan `%s%s%s%s` o cadenas similares  
- [ ] Sí — las escrituras arbitrarias en memoria a través de `%n` son **posibles**, lo que potencialmente conduce a una Ejecución Remota de Código (RCE)  

### ¿Son eludibles las protecciones binarias modernas (ASLR, DEP, Stack Canaries) a través de esta inyección?
- [ ] No — el entorno está endurecido (hardened) y la fuga de memoria **no puede** utilizarse para eludir las protecciones  
- [ ] Sí — la inyección de cadenas de formato **puede** utilizarse para filtrar direcciones base, haciendo que la elusión de ASLR/DEP sea **posible**  

---