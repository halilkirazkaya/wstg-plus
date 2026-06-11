## WSTG-INPV-11 — Code Injection

A injeção de código (Code Injection) ocorre quando uma aplicação avalia trechos de código fornecidos pelo usuário dentro de seu ambiente de execução, geralmente por meio de funções inseguras específicas da linguagem, como `eval()`, `exec()` ou `system()`. Esta vulnerabilidade difere da injeção de comando (Command Injection) porque o alvo é o contexto de execução da linguagem de programação (ex: PHP, Python, Node.js) em vez do shell do sistema operacional subjacente. Atacantes exploram esses pontos para executar lógica arbitrária, exfiltrar variáveis de ambiente ou obter a execução remota de código (RCE) completa no servidor. Pentesters devem investigar funcionalidades que processam expressões matemáticas, templates do lado do servidor (Server-Side Templates) ou parâmetros de configuração dinâmica onde a entrada pode ser interpretada como código executável.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Alguma funcionalidade da aplicação avalia entradas dinâmicas por meio de funções de avaliação específicas da linguagem?
- [ ] Não — funções de avaliação dinâmica **não estão** presentes na base de código  
- [ ] Sim — funções como `eval()`, `exec()` ou `include()` são utilizadas, mas **não** aceitam entrada do usuário  
- [ ] Sim — as funções são utilizadas e processam entradas fornecidas pelo usuário  

### Mecanismos de validação ou sanitização de entrada são aplicados antes da avaliação?
- [ ] Sim — a entrada é rigorosamente validada contra uma **whitelist** estática  
- [ ] Sim — a entrada é sanitizada via blacklisting, mas o **bypass** é possível  
- [ ] Não — a entrada é passada diretamente para a função de avaliação e **nenhum controle** é aplicado  

### O ambiente de execução está isolado (sandboxed) ou restrito para impedir o acesso ao nível do sistema?
- [ ] Sim — a execução ocorre em um sandbox altamente restrito onde chamadas de sistema **não podem** ser feitas  
- [ ] Sim — existem algumas restrições, mas o **sandbox escape** é possível  
- [ ] Não — o ambiente permite acesso total às APIs da linguagem e funções do sistema operacional subjacentes *(Crítica)*  

### A injeção de código pode ser usada para exfiltrar informações sensíveis?
- [ ] Não — a execução é cega (blind) e nenhuma comunicação fora de banda (out-of-band) **é possível**  
- [ ] Sim — variáveis de ambiente ou arquivos locais **podem** ser exfiltrados via requisições HTTP/DNS  
- [ ] Sim — os resultados do código executado são **refletidos diretamente** na resposta da aplicação  

### A aplicação permite a inclusão remota de arquivos (Remote File Inclusion) ou o carregamento dinâmico de bibliotecas?
- [ ] Não — apenas caminhos de código locais e predefinidos podem ser executados  
- [ ] Sim — a aplicação **pode** ser forçada a carregar e executar código de uma fonte remota ou controlada pelo atacante  

---