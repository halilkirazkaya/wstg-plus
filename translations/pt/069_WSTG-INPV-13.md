## WSTG-INPV-13 — Format String Injection

A injeção de string de formato (Format String Injection) ocorre quando uma aplicação web passa entrada controlada pelo usuário diretamente para o argumento de string de formato de uma função variádica, como a família `printf` da linguagem C ou wrappers de log de baixo nível. Embora seja menos comum em frameworks web modernos de código gerenciado, esta vulnerabilidade permanece crítica em aplicações CGI legadas, extensões nativas ou sistemas de log em casos específicos que interagem com bibliotecas de sistema de baixo nível. Atacantes exploram isso fornecendo especificadores de formato como `%x` ou `%p` para vazar endereços de memória sensíveis e dados da stack, ou o especificador `%n` para realizar escritas arbitrárias na memória. A exploração bem-sucedida geralmente resulta no comprometimento completo do sistema via Remote Code Execution (RCE) ou, no mínimo, em uma condição de Denial-of-Service (DoS) impactante através de falhas (crashes) nos processos.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### A aplicação processa entrada de usuário por meio de código nativo ou interfaces CGI legadas?
- [ ] Não — a aplicação é construída inteiramente em frameworks de código gerenciado (ex: JVM, CLR)  
- [ ] Sim — a aplicação utiliza JNI, extensões nativas C/C++, ou CGIs binários legados onde vulnerabilidades de format string **podem** existir  

### As entradas fornecidas pelo usuário são passadas como o argumento principal para funções sensíveis a formato?
- [ ] Não — as entradas são passadas como argumentos de dados, e as strings de formato são **estáticas** e **hardcoded**  
- [ ] Sim — a entrada do usuário é concatenada diretamente no argumento da string de formato, mas a sanitização **é aplicada**  
- [ ] Sim — a entrada do usuário é utilizada diretamente como a string de formato e nenhuma sanitização **é aplicada**  

### É possível vazar conteúdos da memória utilizando especificadores de formato?
- [ ] Não — a entrada é validada e especificadores como `%p`, `%x` ou `%s` **não são processados**  
- [ ] Sim — o fornecimento dos especificadores `%p` ou `%x` resulta na reflexão de endereços de memória da stack ou heap  
- [ ] Sim — dados sensíveis (ex: ponteiros, stack cookies) **podem** ser exfiltrados via especificadores de formato  

### A aplicação pode sofrer crash ou ser manipulada utilizando o especificador `%n`?
- [ ] Não — especificadores que permitem escritas na memória estão **desativados** ou bloqueados pelo compilador/ambiente  
- [ ] Sim — a aplicação sofre crash (DoS) quando `%s%s%s%s` ou strings similares são fornecidas  
- [ ] Sim — escritas arbitrárias na memória via `%n` são **possíveis**, levando potencialmente a um Remote Code Execution (RCE)  

### As proteções binárias modernas (ASLR, DEP, Stack Canaries) são contornáveis através desta injeção?
- [ ] Não — o ambiente é endurecido (hardened) e o vazamento de memória **não pode** ser usado para burlar as proteções  
- [ ] Sim — a injeção de format string **pode** ser usada para vazar endereços base, tornando o bypass de ASLR/DEP **possível**  

---