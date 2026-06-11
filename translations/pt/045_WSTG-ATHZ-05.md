## WSTG-ATHZ-05 — Teste de Fraquezas no OAuth

As fraquezas no OAuth abrangem uma variedade de falhas na implementação dos protocolos OAuth 2.0 ou OpenID Connect (OIDC), resultando frequentemente em account takeover completo ou acesso não autorizado a dados. Estas vulnerabilidades manifestam-se tipicamente durante o fluxo de autorização, especificamente no que diz respeito à validação do `redirect_uri`, à entropia e verificação do parâmetro `state`, e ao manuseio seguro de access ou ID tokens. Atacantes exploram estas falhas interceptando authorization codes, redirecionando tokens para domínios controlados pelo atacante através de open redirects, ou realizando Cross-Site Request Forgery (CSRF) para vincular a sessão de uma vítima à conta de um atacante. Uma vez que o OAuth serve frequentemente como um mecanismo de autenticação primário, uma única configuração incorreta pode comprometer a integridade de todo o sistema de identidade do utilizador.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Ferramentas:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### O parâmetro `state` está implementado e é validado para prevenir CSRF?
- [ ] Sim — o `state` é obrigatório, único por sessão e estritamente validado no servidor  
- [ ] Sim — o `state` está presente, mas é estático ou previsível entre diferentes sessões  
- [ ] Sim — o `state` está presente, mas a aplicação **não** o valida no callback  
- [ ] Não — o parâmetro `state` **não** é utilizado na solicitação de autorização *(Crítica)*  

### O `redirect_uri` é estritamente validado contra uma whitelist?
- [ ] Sim — apenas correspondências exatas com uma whitelist pré-registada são **aceites**  
- [ ] Sim — a validação é aplicada, mas o bypass **é possível** através de path traversal ou manipulação de subdomínios  
- [ ] Sim — a validação é aplicada, mas o bypass **é possível** através de parameter pollution ou fragment injection  
- [ ] Não — a aplicação **permite** URLs arbitrários no parâmetro `redirect_uri` *(Crítica)*  

### O `response_type` pode ser manipulado para alterar o fluxo?
- [ ] Não — a aplicação aceita apenas o fluxo pretendido (ex: `code`) e rejeita outros  
- [ ] Sim — a mudança de `code` para `token` (Implicit flow) **é possível** e vaza tokens no URL  
- [ ] Sim — fluxos híbridos ou valores de `response_type` não autorizados estão **ativados** e são processados  

### Os access tokens ou authorization codes são vazados através de cabeçalhos Referer ou do histórico do navegador?
- [ ] Não — os tokens/codes são manuseados de forma segura e as páginas sensíveis utilizam uma `Referrer-Policy` adequada  
- [ ] Sim — os tokens/codes **são** vazados para domínios de terceiros através do cabeçalho `Referer`  
- [ ] Sim — os tokens/codes **estão** visíveis no histórico do navegador devido a cache insegura ou solicitações GET  

### A aplicação impõe o princípio de "Least Privilege" para os scopes do OAuth?
- [ ] Sim — a aplicação solicita apenas os scopes mínimos necessários para a funcionalidade  
- [ ] Não — a aplicação solicita scopes excessivos ou administrativos por padrão  
- [ ] Não — o utilizador **não pode** rever ou optar por não aceitar scopes específicos durante a fase de consentimento  

---