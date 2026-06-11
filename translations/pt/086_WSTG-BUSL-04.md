## WSTG-BUSL-04 — Test for Process Timing

Ataques de tempo de processamento (*process timing attacks*) envolvem a medição do tempo que uma aplicação leva para processar solicitações específicas para inferir informações sensíveis sobre estados internos ou a existência de dados. Ao analisar discrepâncias de milissegundos nos tempos de resposta — frequentemente causadas por lógica condicional de saída antecipada (*early-exit*), consultas ao banco de dados ou comparações criptográficas de tempo não constante — atacantes podem realizar análise de canal lateral (*side-channel analysis*) para enumerar nomes de usuário válidos, verificar fragmentos de senhas ou confirmar a existência de registros específicos. Essas vulnerabilidades ocorrem tipicamente em *authentication endpoints*, formulários de redefinição de senha e filtros de API que consomem muitos recursos, onde a lógica de *backend* se ramifica com base na validade da entrada. Da perspectiva de um atacante, essas diferenças de tempo servem como um "oráculo booleano" (*boolean oracle*) que revela verdades sobre os dados do *backend*, mesmo quando a aplicação retorna mensagens de erro genéricas e idênticas ao usuário.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Médio |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Ferramentas:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### A aplicação apresenta tempos de resposta variáveis para solicitações de recursos válidos vs. inválidos?
- [ ] Não — os tempos de resposta são idênticos, independentemente da validade da entrada  
- [ ] Sim — existem diferenças de tempo insignificantes, mas **não** são estatisticamente significativas  
- [ ] Sim — diferenças de tempo consistentes são **observáveis** e fornecem um oráculo confiável  

### Funções de comparação de tempo constante ou atrasos artificiais foram implementados para normalizar os tempos de resposta?
- [ ] Sim — algoritmos de tempo constante são **aplicados** a todas as operações de comparação sensíveis *(Mais seguro)*  
- [ ] Sim — atrasos artificiais ou *jitter* estão **habilitados**, mas a lógica subjacente permanece de tempo não constante  
- [ ] Não — nenhuma mitigação de tempo foi **aplicada**, permitindo a medição direta de ramos lógicos  

### Um atacante pode usar análise estatística para isolar o sinal de tempo do ruído de rede?
- [ ] Não — o *network jitter* e o ruído da aplicação tornam a análise de tempo **impossível**  
- [ ] Sim — com um tamanho de amostra suficiente, o sinal de tempo **pode** ser isolado do ruído  
- [ ] Sim — o delta de tempo é grande o suficiente para que a normalização estatística **não** seja necessária  

### A enumeração de contas é possível através da funcionalidade de login ou redefinição de senha?
- [ ] Não — a existência da conta não pode ser determinada através do tempo  
- [ ] Sim — as discrepâncias de tempo permitem que um atacante remoto **enumere** nomes de usuário ou e-mails válidos  

### Operações intensivas em recursos (ex: processamento de arquivos, buscas complexas) vazam a existência de dados via tempo?
- [ ] Não — o consumo de recursos é uniforme, independentemente de um registro ser encontrado ou não  
- [ ] Sim — a existência de registros sensíveis **pode** ser inferida medindo a duração do processamento no *backend*  

---