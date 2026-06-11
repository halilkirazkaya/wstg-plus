## WSTG-INPV-17 — Teste de Host Header Injection

A Host Header Injection ocorre quando uma aplicação confia no cabeçalho `Host` fornecido pelo usuário e o incorpora na lógica do lado do servidor, como na geração de links ou redirecionamentos, sem validação suficiente. Atacantes manipulam este cabeçalho para facilitar o envenenamento de cache web (Web Cache Poisoning), facilitar o envenenamento de redefinição de senha ao redirecionar tokens sensíveis para um domínio externo, ou burlar controles de segurança que dependem de roteamento baseado em cabeçalhos. A exploração bem-sucedida permite que um atacante realize a exfiltração de dados de sessão, execute o comprometimento de contas (Account Takeover) ou redirecione usuários para uma infraestrutura maliciosa. Esta vulnerabilidade é comumente encontrada em ambientes com balanceamento de carga ou aplicações que geram dinamicamente URLs absolutas baseadas no contexto da requisição recebida.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Médio / Alto* |

> *A severidade torna-se Alta se o cabeçalho Host for utilizado para gerar links sensíveis em e-mails, como redefinições de senha.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Ferramentas:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### A aplicação reflete o valor do cabeçalho `Host` em links, redirecionamentos ou scripts?
- [ ] Não — a aplicação utiliza URLs base estáticas (hardcoded) ou configurações estritamente validadas  
- [ ] Sim — o cabeçalho `Host` é refletido, mas validado rigorosamente contra uma whitelist  
- [ ] Sim — o cabeçalho `Host` é refletido e o bypass **é possível** através de valores malformados (ex: `expected.com:attacker.com`)  
- [ ] Sim — o cabeçalho `Host` é refletido **sem** qualquer validação  

### Cabeçalhos alternativos relacionados ao host são processados pela aplicação ou infraestrutura?
- [ ] Não — cabeçalhos como `X-Forwarded-Host`, `X-Host` ou `Forwarded` são ignorados  
- [ ] Sim — cabeçalhos alternativos são processados, mas **não** sobrescrevem a lógica interna  
- [ ] Sim — cabeçalhos alternativos **podem** ser usados para sobrescrever o valor do cabeçalho `Host` principal  

### O cabeçalho `Host` pode ser manipulado para envenenar e-mails gerados pelo sistema (ex: redefinições de senha)?
- [ ] Não — os links de e-mail utilizam um domínio estático e confiável da configuração do servidor  
- [ ] Sim — o cabeçalho `Host` influencia os links de e-mail, mas a validação **não** pode ser burlada  
- [ ] Sim — o cabeçalho `Host` **pode** ser manipulado para redirecionar tokens de redefinição de senha para um domínio controlado pelo atacante *(Crítico)*  

### A aplicação é suscetível a Web Cache Poisoning via cabeçalho `Host`?
- [ ] Não — não há mecanismo de cache presente ou o cabeçalho `Host` faz parte da chave de cache (cache key)  
- [ ] Sim — um cache está presente, mas ele valida rigorosamente ou ignora o cabeçalho `Host` para entradas não indexadas (unkeyed inputs)  
- [ ] Sim — um cache existe e **pode** ser envenenado por um cabeçalho `Host` injetado para servir conteúdo malicioso a outros usuários  

### O servidor aceita cabeçalhos `Host` arbitrários para roteamento de hosts virtuais (vhosts)?
- [ ] Não — o servidor retorna um erro ou uma página padrão para cabeçalhos `Host` não reconhecidos  
- [ ] Sim — o servidor aceita qualquer cabeçalho `Host`, o que **é** útil para reconhecimento (reconnaissance) ou enumeração de serviços internos  

---