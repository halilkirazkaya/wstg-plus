## WSTG-ATHN-08 — Teste de Respostas Fracas a Perguntas de Segurança

O teste de respostas fracas a perguntas de segurança envolve a avaliação dos mecanismos de autenticação baseada em conhecimento (KBA) utilizados para verificar a identidade de um usuário, normalmente durante fluxos de recuperação de senha ou autenticação de múltiplos fatores. Isso é importante porque as perguntas de segurança geralmente dependem de informações estáticas e não secretas que podem ser facilmente descobertas por meio de inteligência de fontes abertas (OSINT) ou Engenharia Social, levando ao Account Takeover não autorizado. Essas vulnerabilidades ocorrem tipicamente nos módulos de "Esqueci a Senha" ou "Desbloqueio de Conta", onde os usuários fornecem respostas a perguntas pré-definidas. Do ponto de vista de um atacante, este é um alvo principal para Brute Force automatizado ou pesquisa direcionada, já que muitos usuários fornecem respostas previsíveis ou publicamente verificáveis para perguntas comuns como "Qual é o nome de solteira da sua mãe?" ou "Qual escola secundária você frequentou?".

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a pergunta de segurança for o único requisito para uma redefinição completa de senha e Account Takeover sem verificação adicional.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Ferramentas:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### A aplicação implementa perguntas de segurança para verificação de identidade ou recuperação de senha?
- [ ] Não — perguntas de segurança **não são utilizadas** para autenticação ou recuperação  
- [ ] Sim — perguntas de segurança são **utilizadas** como um fator secundário opcional  
- [ ] Sim — perguntas de segurança são **utilizadas** como o mecanismo de recuperação primário/único *(Alto Risco)*  

### As perguntas de segurança são selecionadas de uma lista fixa ou são definidas pelo usuário?
- [ ] Não — as perguntas são inteiramente definidas pelo usuário e exigem alta entropia  
- [ ] Sim — as perguntas são fixas, mas incluem opções não descobertas via OSINT  
- [ ] Sim — as perguntas são de uma lista fixa de tópicos comuns e fáceis de descobrir via OSINT *(Vulnerável)*  

### O Rate Limiting ou bloqueio de conta é aplicado às tentativas de resposta às perguntas de segurança?
- [ ] Sim — Rate Limiting rigoroso e bloqueio de conta (Account Lockout) **são aplicados** para evitar Brute Force  
- [ ] Sim — o Rate Limiting **é aplicado**, mas o Exploit de Bypass **é possível** via rotação de IP ou manipulação de cabeçalhos (Headers)  
- [ ] Não — **nenhum Rate Limiting** é aplicado às tentativas de resposta  

### A aplicação impõe complexidade ou validação no conteúdo da resposta?
- [ ] Sim — a validação impede palavras comuns e garante a complexidade/exclusividade da resposta  
- [ ] Sim — a validação básica está presente, mas **pode** ser burlada com listas de palavras comuns ou ataques de dicionário  
- [ ] Não — **nenhuma validação** é realizada; respostas simples ou de um único caractere **são permitidas**  

### O desafio da pergunta de segurança pode ser burlado através de falhas lógicas?
- [ ] Não — o fluxo de recuperação exige estritamente uma resposta válida para prosseguir  
- [ ] Sim — o fluxo de recuperação **pode** ser burlado (Bypass) manipulando códigos de resposta HTTP ou parâmetros de sessão  
- [ ] Sim — a resposta é refletida no código-fonte ou em campos ocultos da página  

---