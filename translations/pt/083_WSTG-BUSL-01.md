## WSTG-BUSL-01 — Teste de Validação de Dados da Lógica de Negócio

O teste de validação de dados da lógica de negócio foca na capacidade da aplicação de identificar e rejeitar dados que são sintaticamente corretos, mas logicamente inválidos dentro do contexto do domínio de negócio. Atacantes exploram essas falhas enviando valores inesperados — como quantidades negativas em um carrinho de compras, datas no passado para eventos futuros ou inteiros extremamente grandes — para contornar restrições e manipular o estado da aplicação. Essas vulnerabilidades residem frequentemente em workflows de múltiplas etapas, processos de checkout e módulos de gerenciamento de contas, onde a lógica do lado do servidor (server-side logic) falha em verificar a integridade contextual dos dados fornecidos pelo usuário. A exploração bem-sucedida pode levar a perdas financeiras, comprometimento da integridade e acesso não autorizado a recursos ou dados restritos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### A aplicação impõe limites de intervalo lógico em entradas numéricas?
- [ ] Sim — a aplicação rejeita corretamente valores negativos, zero ou excessivamente grandes onde for inapropriado *(Mais seguro)*  
- [ ] Sim — a aplicação rejeita alguns intervalos inválidos, mas **é** vulnerável a integer overflow ou boundary bypass  
- [ ] Não — a aplicação aceita qualquer valor numérico, independentemente do contexto de negócio *(Crítico)*  

### As verificações de validação do lado do servidor são consistentes em todas as camadas da aplicação?
- [ ] Sim — a validação é aplicada consistentemente tanto nas camadas de API/serviço quanto no banco de dados  
- [ ] Sim — a validação **é aplicada** na UI/Frontend, mas o bypass via requisição direta à API **é possível**  
- [ ] Não — a validação **não é aplicada** ou é inconsistente, permitindo a corrupção lógica de dados  

### A lógica de negócio pode ser subvertida pela manipulação de dados em workflows de múltiplas etapas?
- [ ] Não — o estado é mantido de forma segura no servidor e os dados de etapas anteriores **não podem** ser adulterados  
- [ ] Sim — a validação ocorre nas etapas iniciais, mas a submissão final **não** verifica novamente a integridade dos dados  
- [ ] Sim — o bypass de workflow **é possível** ao pular etapas ou enviar dados fora de ordem  

### A aplicação lida adequadamente com dados de "casos extremos" (edge cases) que contornam as restrições de negócio?
- [ ] Sim — as restrições são robustas e lidam com entradas incomuns, mas sintaticamente válidas (ex: anos bissextos, mudanças de fuso horário)  
- [ ] Sim — alguns casos extremos são tratados, mas combinações específicas **podem** levar a comportamentos inesperados  
- [ ] Não — casos extremos levam diretamente a falhas de lógica, como compras gratuitas ou alterações de status não autorizadas  

---