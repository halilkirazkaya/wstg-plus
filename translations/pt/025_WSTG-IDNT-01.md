## WSTG-IDNT-01 — Teste de Definições de Funções

O teste de definições de funções envolve a identificação e documentação das várias funções (roles) de usuário e níveis de permissão definidos na aplicação para garantir que estejam logicamente separados e sigam o princípio do privilégio mínimo (**least privilege**). Um atacante analisa a aplicação para mapear funções de alto privilégio em relação a funções de usuário padrão, procurando por qualquer ambiguidade nos limites de permissão ou funções administrativas não documentadas. A identificação dessas funções é o primeiro passo para descobrir caminhos de escalada de privilégios, pois inconsistências nas definições de funções frequentemente levam ao acesso não autorizado a dados sensíveis ou funcionalidades administrativas. Esta fase ocorre normalmente durante o reconhecimento inicial e o mapeamento da lógica de negócio, focando em perfis de usuário, painéis administrativos e estruturas de permissão de API.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### As funções estão claramente definidas e documentadas na aplicação ou em sua documentação?
- [ ] Sim — as funções estão totalmente documentadas e seguem o princípio do **privilégio mínimo**  
- [ ] Sim — as funções estão documentadas, mas contêm **permissões sobrepostas**  
- [ ] Não — não existem documentações ou definições formais de funções  

### Existe uma separação clara entre as funções administrativas e as de usuário padrão?
- [ ] Sim — as funções administrativas estão **completamente isoladas** das visualizações de usuário padrão  
- [ ] Sim — a separação existe, mas os endpoints administrativos são **previsíveis ou adivinháveis**  
- [ ] Não — as funções administrativas e padrão estão **misturadas** nas mesmas interfaces  

### A aplicação utiliza um mecanismo centralizado para Controle de Acesso Baseado em Funções (RBAC)?
- [ ] Sim — RBAC ou ABAC centralizado está **implementado** e é aplicado de forma consistente  
- [ ] Sim — existem controles descentralizados, mas são **aplicados de forma inconsistente** entre os módulos  
- [ ] Não — a lógica de controle de acesso está **hardcoded** em páginas ou componentes individuais  

### Existem funções não documentadas ou ocultas presentes no sistema?
- [ ] Não — todas as funções descobertas correspondem à funcionalidade documentada  
- [ ] Sim — funções ocultas ou "shadow roles" (ex: `super_admin`, `support`, `test`) **estão presentes**  

### Um usuário de baixo privilégio pode enumerar as permissões ou funções de outros usuários?
- [ ] Não — os usuários **não podem** visualizar ou enumerar as funções/permissões de outras entidades  
- [ ] Sim — os usuários **podem** enumerar funções através de páginas de perfil, metadados ou respostas de API  *(Média)*  

---