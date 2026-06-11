## WSTG-INPV-12 — Command Injection

A injeção de comando (Command Injection) ocorre quando uma aplicação passa dados inseguros fornecidos pelo usuário, como entradas de formulário, cabeçalhos HTTP ou cookies, para um shell do sistema. Esta vulnerabilidade permite que um atacante execute comandos arbitrários do sistema operacional no servidor, tipicamente com os privilégios do processo da aplicação web. Do ponto de vista de um atacante, isso é explorado utilizando metacaracteres de shell como ponto e vírgula, pipes ou crases (backticks) para encadear comandos maliciosos ao fluxo de execução pretendido. O impacto é frequentemente catastrófico, levando ao comprometimento total do sistema, exfiltração não autorizada de dados ou movimentação lateral dentro da rede interna.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Ferramentas:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### A aplicação invoca comandos de nível de sistema ou executáveis de shell?
- [ ] Não — a aplicação **não** interage com o shell do SO  
- [ ] Sim — a aplicação utiliza APIs internas ou bibliotecas de alto nível para tarefas do sistema  
- [ ] Sim — a aplicação invoca comandos de shell diretamente via chamadas de sistema como `system()`, `exec()` ou `popen()`  

### A entrada do usuário é validada ou sanitizada antes de ser passada para chamadas de sistema?
- [ ] Sim — lista de permissões (allow-listing) estrita e validação de entrada **são aplicadas**  
- [ ] Sim — sanitização ou escape (escaping) são utilizados, mas o bypass **é possível**  
- [ ] Não — a entrada do usuário é passada diretamente para o shell **sem** validação  

### Metacaracteres de shell podem ser usados para manipular o fluxo de execução de comandos?
- [ ] Não — os metacaracteres são corretamente filtrados ou escapados  
- [ ] Sim — a execução em banda (in-band), onde a saída do comando é refletida na resposta, **é possível**  
- [ ] Sim — a execução cega (blind), onde nenhuma saída é refletida, **é possível**  

### Exfiltração fora de banda (OOB) é possível a partir do host?
- [ ] Não — o servidor não possui tráfego de saída (egress) ou sinais OOB (DNS/HTTP) falham  
- [ ] Sim — a exfiltração de dados via sinais OOB baseados em DNS ou HTTP **é possível**  

### O processo da aplicação é executado com privilégios de sistema elevados?
- [ ] Não — a aplicação é executada com uma conta de serviço dedicada de baixo privilégio  
- [ ] Sim — a aplicação é executada como `root`, `Administrator` ou um usuário de alto privilégio *(Crítico)*  

---