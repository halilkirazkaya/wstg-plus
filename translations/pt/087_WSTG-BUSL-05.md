## WSTG-BUSL-05 — Teste de Limites do Número de Vezes que uma Função Pode Ser Utilizada

Este teste avalia se uma aplicação impõe restrições de lógica de negócio sobre a frequência e o número total de vezes que uma ação ou função específica pode ser executada por um único usuário, sessão ou endereço IP. A falha na implementação desses limites permite que atacantes abusem de funções de alto valor — como o envio de SMS OTPs, disparo de e-mails de redefinição de senha, aplicação de códigos de desconto ou execução de exportações de dados pesadas — resultando em perdas financeiras, exaustão de recursos ou danos à reputação. Essas vulnerabilidades são normalmente encontradas em endpoints transacionais ou funcionalidades de comunicação e são exploradas através de scripts automatizados que repetem a ação muito além dos limites de negócio pretendidos. Do ponto de vista do atacante, este é um alvo principal para SMS pumping, mail bombing ou workflows de Brute Force (Força Bruta) que carecem de Rate Limiting (Limitação de Taxa) explícito ou controles baseados em contagem.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Média / Alta* |

> *A gravidade torna-se Alta se a função envolver transações financeiras, custos de SMS ou impactar a disponibilidade global do sistema.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### A aplicação define e impõe limites em funções de negócio sensíveis?
- [ ] Sim — os limites são rigorosamente aplicados e o bypass **não é possível**  
- [ ] Sim — os limites são aplicados, mas são excessivamente altos para prevenir abusos  
- [ ] Não — nenhum limite é aplicado à execução da função  

### Os limites de taxa (Rate Limiting) e contadores de uso são aplicados no lado do servidor (Server-side)?
- [ ] Sim — a imposição é estritamente no server-side e **não pode** ser manipulada  
- [ ] Não — a imposição depende de lógica no client-side (ex: JavaScript ou campos ocultos)  
- [ ] Não — não existe imposição em nenhum nível  

### Os limites de uso podem ser contornados através de manipulação de cabeçalhos (headers) ou de sessão?
- [ ] Não — bypass via `X-Forwarded-For`, `Origin` ou rotação de sessão **não é possível**  
- [ ] Sim — os limites **podem** ser contornados através de spoofing de cabeçalhos de IP ou limpeza de cookies  
- [ ] Sim — os limites **podem** ser contornados através da criação de múltiplas contas ou sessões  

### Exceder o limite aciona uma resposta defensiva apropriada?
- [ ] Sim — a aplicação retorna `429 Too Many Requests` e registra (logs) o evento *(Mais seguro)*  
- [ ] Sim — a aplicação retorna um erro, mas **não** registra o potencial abuso  
- [ ] Não — a aplicação continua a processar requisições, mas ignora a saída  
- [ ] Não — a aplicação não fornece resposta ou falha (crashes) sob carga  

### O impacto do abuso da função é significativo para o negócio?
- [ ] Não — o abuso da função tem custo ou impacto de recursos insignificantes  
- [ ] Sim — o abuso **pode** levar ao consumo menor de recursos ou incômodo ao usuário  
- [ ] Sim — o abuso **pode** levar a custos financeiros significativos (ex: taxas de SMS) ou negação de serviço *(Crítico)*  

---