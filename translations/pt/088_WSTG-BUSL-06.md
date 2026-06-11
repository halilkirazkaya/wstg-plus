## WSTG-BUSL-06 — Teste para a Circunvenção de Fluxos de Trabalho

A circunvenção de fluxos de trabalho envolve a manipulação da lógica de uma aplicação para ignorar sequências pretendidas ou pré-requisitos dentro de processos de múltiplas etapas. Atacantes identificam endpoints sensíveis — como confirmações de pagamento, aprovações administrativas ou telas de conclusão de MFA — e tentam acessá-los diretamente, pulando etapas intermediárias obrigatórias. Esta vulnerabilidade ocorre quando a lógica do lado do servidor falha em manter uma máquina de estados robusta, confiando, em vez disso, na suposição de que o usuário segue o caminho orientado pela UI. A exploração bem-sucedida pode levar a falhas críticas de lógica de negócio, incluindo transações não autorizadas, escalonamento de privilégios e a burlagem de mecanismos de verificação de identidade.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a circunvenção permitir transações financeiras não autorizadas, escalonamento de privilégios ou acesso administrativo.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### A aplicação contém processos de negócio em múltiplas etapas?
- [ ] Não — a aplicação consiste apenas em ações de requisição única  
- [ ] Sim — processos de múltiplas etapas (ex: carrinhos de compras, formulários assistidos/wizards, registro) **estão** presentes  

### A sequência de etapas é estritamente imposta pelo servidor?
- [ ] Sim — o rastreamento de estado no lado do servidor garante que as etapas **não possam** ser ignoradas  
- [ ] Sim — controles no lado do cliente sugerem uma sequência, mas o servidor **não** valida o progresso  
- [ ] Não — a aplicação **não** impõe nenhuma ordem específica de operações  

### Um atacante pode alcançar a etapa final de um processo solicitando diretamente a URL ou endpoint terminal?
- [ ] Não — o acesso direto às etapas terminais **não é possível** sem a conclusão prévia  
- [ ] Sim — etapas terminais (ex: `/api/v1/checkout/complete`) **podem** ser alcançadas por requisição direta  

### A aplicação verifica se os dados pré-requisitos das etapas anteriores estão presentes e são válidos?
- [ ] Sim — o servidor valida todos os dados das etapas anteriores antes de prosseguir  
- [ ] Não — o servidor **é** vulnerável a "pular" etapas onde os dados são esperados, mas não verificados  

### Controles de segurança como MFA ou verificação de identidade podem ser burlados através da manipulação do fluxo de trabalho?
- [ ] Não — controles críticos **não podem** ser contornados via manipulação de fluxo  
- [ ] Sim — a burlagem de MFA ou verificações de identidade **é possível** ao saltar para a etapa pós-verificação  

---