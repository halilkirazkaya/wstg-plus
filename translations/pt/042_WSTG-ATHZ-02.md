## WSTG-ATHZ-02 — Teste de Bypass ao Esquema de Autorização

O bypass de esquemas de autorização ocorre quando uma aplicação falha em aplicar controles de acesso, permitindo que um atacante acesse recursos ou funções fora de suas permissões pretendidas. Esta vulnerabilidade normalmente se manifesta como Escalação de Privilégios Horizontal (Horizontal Privilege Escalation), onde um atacante acessa dados pertencentes a outro usuário do mesmo nível, ou Escalação de Privilégios Vertical (Vertical Privilege Escalation), onde um usuário de baixo privilégio obtém capacidades administrativas. Atacantes exploram essas falhas manipulando parâmetros de requisição, como IDs de recursos, modificando tokens de sessão ou utilizando cabeçalhos HTTP como `X-Original-URL` para contornar restrições baseadas em caminhos (path-based restrictions). Garantir uma autorização robusta é crítico para manter a confidencialidade dos dados e prevenir ações administrativas não autorizadas que poderiam comprometer todo o sistema.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Ferramentas:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### A escalação de privilégios horizontal é evitada entre usuários do mesmo perfil (role)?
- [ ] Sim — as verificações de autorização **são aplicadas** a cada requisição de recurso e o bypass **não é possível**  
- [ ] Sim — as verificações de autorização **são aplicadas**, mas podem ser burladas via IDOR ou manipulação de parâmetros (parameter tampering)  
- [ ] Não — os usuários **podem** acessar dados pertencentes a outros usuários simplesmente alterando um ID de recurso *(Alta)*  

### A escalação de privilégios vertical é evitada para usuários de baixo privilégio?
- [ ] Não — a aplicação possui apenas um nível de privilégio  
- [ ] Sim — funções administrativas **não podem** ser acessadas por usuários de baixo privilégio  
- [ ] Sim — as funções administrativas estão ocultas na interface (UI), mas **podem** ser acessadas via URL direta  
- [ ] Não — usuários de baixo privilégio **podem** realizar ações administrativas manipulando perfis (roles) ou permissões *(Crítica)*  

### A autorização pode ser burlada utilizando tampering de verbos HTTP?
- [ ] Não — os controles de acesso são aplicados independentemente do método HTTP utilizado  
- [ ] Sim — alterar o método (ex: de `GET` para `POST` ou `PUT`) **burla** as verificações de autorização  

### Endpoints administrativos ou restritos estão acessíveis sem qualquer autenticação?
- [ ] Não — todos os endpoints sensíveis exigem uma sessão válida e permissões adequadas  
- [ ] Sim — alguns endpoints administrativos são acessíveis se o atacante conhecer a URL direta  
- [ ] Sim — o acesso não autenticado a funções sensíveis **é possível** *(Crítica)*  

### Os cabeçalhos HTTP permitem a circunvenção da autorização baseada em caminho (path)?
- [ ] Não — a aplicação **não é** suscetível a bypasses baseados em cabeçalhos (header-based bypasses)  
- [ ] Sim — cabeçalhos como `X-Original-URL`, `X-Rewrite-URL` ou `X-Forwarded-For` **podem** ser usados para burlar controles de acesso  

---