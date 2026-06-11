## WSTG-IDNT-04 — Teste de Enumeração de Contas e Contas de Usuário Adivinháveis

A enumeração de contas ocorre quando uma aplicação revela a existência ou não de uma conta de usuário por meio de variações nas respostas HTTP, mensagens de erro ou diferenças de tempo mensuráveis. Atacantes exploram esse comportamento enviando sistematicamente listas de nomes de usuário para identificar contas válidas, que são posteriormente visadas para credential stuffing, ataques de Brute Force ou engenharia social. Esta vulnerabilidade manifesta-se comumente em portais de login, funcionalidades de redefinição de senha, formulários de registro e endpoints de API que manipulam identificadores específicos do usuário. Do ponto de vista de um atacante, a enumeração bem-sucedida reduz significativamente a superfície de ataque, transformando uma tentativa de Brute Force cega em uma exploração direcionada de identidades conhecidas e válidas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### As mensagens de erro de autenticação são consistentes em todos os cenários?
- [ ] Sim — mensagens de erro genéricas são usadas tanto para nomes de usuário inválidos quanto para senhas inválidas  
- [ ] Sim — as mensagens são semelhantes, mas pequenas variações na pontuação ou no uso de maiúsculas/minúsculas **estão presentes**  
- [ ] Não — as mensagens de erro declaram explicitamente "Usuário não encontrado" ou "Senha incorreta" *(Vulnerável)*  

### O fluxo de redefinição de senha ou "esqueci minha senha" vaza a existência da conta?
- [ ] Não — a aplicação retorna uma mensagem genérica independentemente de o e-mail/nome de usuário existir  
- [ ] Sim — existem controles, mas o bypass **é possível** através do comprimento da resposta ou códigos de status  
- [ ] Sim — a aplicação confirma explicitamente se um e-mail de redefinição foi enviado ou se a conta **não existe**  

### O processo de registro está protegido contra enumeração de usuários?
- [ ] Não — o registro não vaza informações ou utiliza CAPTCHA/Rate Limiting para evitar verificações em massa  
- [ ] Sim — existem controles, mas o bypass **é possível** através de diferenças de tempo ou de resposta da API  
- [ ] Sim — a aplicação notifica imediatamente o usuário se o nome de usuário ou e-mail **já está em uso**  

### Diferenças de tempo são mensuráveis ao comparar nomes de usuário válidos vs. inválidos?
- [ ] Não — os tempos de resposta são consistentes independentemente da validade da conta devido a comparações em tempo constante  
- [ ] Sim — existem diferenças de tempo, mas são muito pequenas para serem medidas de forma confiável através da rede  
- [ ] Sim — discrepâncias de tempo mensuráveis **estão presentes** (ex: devido à execução de hashing de senha apenas para usuários válidos)  

### A aplicação utiliza convenções de nomenclatura previsíveis ou adivinháveis para as contas?
- [ ] Não — os identificadores de conta são aleatórios (UUIDs) ou definidos pelo usuário e complexos  
- [ ] Sim — os identificadores de conta seguem um padrão (ex: `emp001`, `emp002`) e a enumeração **é possível**  
- [ ] Sim — os identificadores são números inteiros sequenciais, tornando possível a enumeração completa do banco de dados **possível**  

---