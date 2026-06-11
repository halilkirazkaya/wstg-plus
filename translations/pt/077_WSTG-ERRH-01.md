## WSTG-ERRH-01 — Teste de Manipulação Inadequada de Erros

A manipulação inadequada de erros ocorre quando uma aplicação web divulga detalhes técnicos sensíveis — como stack traces, informações de esquema de banco de dados ou caminhos de arquivos internos — por meio de suas respostas de erro. Atacantes provocam esses erros enviando entradas malformadas, acessando recursos inexistentes ou induzindo exceções no lado do servidor para mapear a arquitetura subjacente da aplicação e identificar possíveis vetores para exploração posterior. Esse vazamento de informações frequentemente serve como um precursor para ataques mais graves, incluindo SQL Injection ou Path Traversal, ao fornecer ao atacante especificações precisas do ambiente. Uma implementação segura deve fornecer mensagens genéricas amigáveis ao usuário, capturando diagnósticos detalhados apenas em logs seguros no lado do servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### A aplicação utiliza um manipulador de erros global e genérico para exceções não tratadas?
- [ ] Sim — páginas de erro genéricas estão **habilitadas** e não revelam **nenhum** detalhe técnico  
- [ ] Sim — páginas de erro personalizadas são utilizadas, mas o vazamento **é possível** por meio de cabeçalhos específicos  
- [ ] Não — páginas de erro padrão do servidor (ex: Tomcat, IIS, Nginx) estão **visíveis**  

### É possível enumerar informações técnicas por meio de entradas malformadas ou casos de borda?
- [ ] Não — os erros são manipulados de forma adequada com IDs de referência únicos para registro (logging)  
- [ ] Sim — stack traces ou consultas de banco de dados **são expostos** nas respostas  
- [ ] Sim — caminhos de arquivos internos, variáveis de ambiente ou strings de versão do servidor **são expostos**  

### As respostas de API estão vazando objetos de erro detalhados ou informações de depuração?
- [ ] Não — as APIs retornam códigos de erro padronizados e mensagens JSON sanitizadas  
- [ ] Sim — as APIs retornam objetos de depuração detalhados ou detalhes completos da exceção no corpo da resposta  
- [ ] Sim — stack traces estão incluídos nos campos `details` ou `exception` da resposta JSON  

### A aplicação se comporta de forma diferente (Baseada em tempo ou Baseada em conteúdo) quando ocorrem erros?
- [ ] Não — as assinaturas de resposta e os tempos (timings) são consistentes, independentemente do tipo de erro  
- [ ] Sim — respostas diferenciadas **podem** ser usadas para enumerar estados válidos vs. inválidos (ex: Enumeração de Usuários)  

---