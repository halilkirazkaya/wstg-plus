## WSTG-ATHZ-03 — Teste de Escalação de Privilégios

A escalação de privilégios ocorre quando um atacante explora vulnerabilidades para obter acesso a recursos ou funcionalidades reservados a usuários com níveis de autorização mais elevados ou identidades diferentes. Na escalação vertical, um usuário padrão tenta acessar funções administrativas, enquanto a escalação horizontal envolve o acesso a dados pertencentes a outro usuário com o mesmo nível de privilégio. Essas falhas normalmente se manifestam em listas de controle de acesso (ACLs) mal implementadas, referências diretas a objetos inseguras (IDOR) ou manipulação de parâmetros (parameter tampering) dentro de tokens de sessão ou de identidade. Do ponto de vista de um atacante, isso é frequentemente alcançado manipulando IDs numéricos, modificando parâmetros baseados em funções em cookies ou JWTs, ou chamando diretamente endpoints de API ocultos que carecem de verificações de autorização no lado do servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Ferramentas:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### A aplicação implementa múltiplos níveis de privilégio ou multi-tenancy?
- [ ] Não — a aplicação é um sistema de usuário único ou função única  
- [ ] Sim — múltiplas funções ou tenants **estão** definidos e exigem testes de autorização  

### A escalação de privilégios horizontal (acesso ao mesmo nível) é possível?
- [ ] Não — as verificações de autorização **são aplicadas** a todos os identificadores de recursos e proprietários de sessão  
- [ ] Sim — o acesso aos dados de outros usuários **é possível** através de IDOR ou manipulação de parâmetros  
- [ ] Sim — o vazamento de dados **é possível**, mas a modificação de dados de outros usuários **não é possível**  

### A escalação de privilégios vertical (baixo-para-alto) é possível?
- [ ] Não — os endpoints administrativos aplicam estritamente controles de acesso baseados em funções (RBAC)  
- [ ] Sim — as funções administrativas **podem** ser acessadas por usuários com baixos privilégios através de acesso direto via URL  
- [ ] Sim — as funções administrativas **podem** ser acessadas manipulando cabeçalhos relacionados a funções, cookies ou claims de JWT  

### As funções ou permissões de usuário podem ser modificadas via Mass Assignment ou Parameter Pollution?
- [ ] Não — os campos baseados em funções são estritamente de leitura e **não podem** ser modificados pelos usuários  
- [ ] Sim — as permissões de usuário **podem** ser elevadas incluindo parâmetros ocultos (ex: `role=admin`) em atualizações de perfil ou registro  

### As verificações de autorização são aplicadas de forma consistente em todas as versões de API e métodos HTTP?
- [ ] Sim — os controles **são aplicados** consistentemente em todas as versões e métodos  
- [ ] Não — versões legadas da API (ex: `/v1/`) ou métodos HTTP específicos (ex: `PUT`, `DELETE`, `PATCH`) **ignoram** a autorização  
- [ ] Não — as funções administrativas estão ocultas na UI, mas **habilitadas** no nível da API para todos os usuários  

---