## WSTG-INPV-08 — Teste de SSI Injection

A Injeção de Server-Side Includes (SSI Injection) ocorre quando uma aplicação falha ao sanitizar a entrada do usuário antes de incorporá-la em arquivos HTML processados pelo motor de SSI do servidor. Atacantes exploram isso injetando diretivas SSI, tais como `<!--#exec cmd="id" -->`, para executar comandos arbitrários de shell, ler arquivos locais ou exfiltrar variáveis de ambiente. Esta vulnerabilidade é normalmente encontrada em aplicações que utilizam extensões de arquivo legadas como `.shtml`, `.shtm` ou `.stm`, ou onde o servidor web está explicitamente configurado para processar SSI dentro de arquivos HTML padrão. A exploração bem-sucedida concede ao atacante as mesmas permissões do processo do servidor web, frequentemente levando ao comprometimento total do sistema ou à exposição de dados sensíveis.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### O servidor web suporta e processa diretivas SSI?
- [ ] Não — o servidor **não suporta** SSI ou extensões de arquivo como `.shtml` estão **desativadas**  
- [ ] Sim — o SSI **está habilitado**, mas a entrada do usuário **não é** refletida nas páginas processadas  
- [ ] Sim — o SSI **está habilitado** e a entrada do usuário **é** refletida nas páginas processadas  

### Comandos arbitrários do sistema podem ser executados através da diretiva `#exec`?
- [ ] Não — a diretiva `#exec` está **desativada** ou restrita pela configuração do servidor  
- [ ] Sim — a execução de comandos **é possível** através de payloads `#exec cmd` ou `#exec cgi`  

### A exfiltração de arquivos sensíveis ou variáveis de ambiente é possível?
- [ ] Não — diretivas como `#include` ou `#config` estão **desativadas**  
- [ ] Sim — a leitura de arquivos locais (ex: `/etc/passwd`) **é possível** através de `#include virtual`  
- [ ] Sim — variáveis de ambiente do servidor **podem** ser exfiltradas via `#printenv` ou `#echo`  

### A validação de entrada ou controles de WAF são eficazes contra payloads de SSI?
- [ ] Sim — a validação rigorosa de entrada **é aplicada** e o bypass **não é possível**  
- [ ] Sim — a validação/WAF **é aplicada**, mas o bypass **é possível** utilizando codificação de caracteres ou sintaxe SSI diferente  
- [ ] Não — nenhuma validação ou proteção WAF está presente para caracteres de SSI (`<`, `!`, `#`, `-`, `"`)  

---