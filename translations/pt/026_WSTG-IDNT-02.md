## WSTG-IDNT-02 — Testar o Processo de Registro de Usuário

Testar o processo de registro de usuário envolve avaliar o fluxo de trabalho por meio do qual novas identidades são criadas para garantir que ele não possa ser explorado para acesso não autorizado ou exploração automatizada. As vulnerabilidades nesse processo manifestam-se frequentemente como falta de verificação de identidade (e-mail/SMS), suscetibilidade à criação massiva de contas via bots ou exposição de informações por meio de mensagens de erro detalhadas que revelam nomes de usuários existentes. Atacantes exploram essas falhas para realizar a enumeração de usuários (User Enumeration), conduzir campanhas de spam em larga escala ou manipular parâmetros de registro para obter privilégios elevados durante a fase de criação da conta. Este teste é vital para proteger a integridade da aplicação e evitar que atacantes povoem o sistema com contas fraudulentas ou maliciosas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### A verificação de identidade é exigida antes que uma nova conta se torne ativa?
- [ ] Sim — o registro requer um token único e com limite de tempo enviado por e-mail ou SMS  
- [ ] Sim — a verificação é exigida, mas os tokens são **fracos** ou **previsíveis**  
- [ ] Não — as contas são ativadas **imediatamente** sem qualquer etapa de verificação  

### O processo de registro impede a enumeração de usuários?
- [ ] Sim — a aplicação retorna respostas **idênticas** tanto para e-mails/usuários novos quanto existentes  
- [ ] Não — mensagens de erro (ex: "E-mail já em uso") **permitem** que um atacante mapeie usuários registrados  
- [ ] Não — diferenças de tempo (timing differences) nas respostas do servidor **revelam** se um usuário existe  

### Controles anti-automação estão implementados para evitar o registro em massa?
- [ ] Sim — mecanismos de CAPTCHA ou proof-of-work estão **presentes** e são **eficazes**  
- [ ] Sim — existem controles, mas o bypass **é possível** através de endpoints de API ou manipulação de cabeçalhos  
- [ ] Não — nenhum Rate Limiting ou CAPTCHA é **aplicado** ao endpoint de registro  

### Os parâmetros de registro podem ser manipulados para escalada de privilégios?
- [ ] Não — as funções de usuário (roles) são atribuídas **estritamente** pela lógica do lado do servidor  
- [ ] Sim — parâmetros como `role`, `admin` ou `group_id` **podem** ser modificados na requisição para obter acesso elevado  

### A política de senhas é aplicada durante a fase de registro?
- [ ] Sim — requisitos de senha forte são **aplicados** no lado do servidor  
- [ ] Não — senhas fracas (ex: "password123") são **aceitas** pelo sistema  

---