## WSTG-CONF-10 — Subdomain Takeover

O Subdomain Takeover ocorre quando uma entrada DNS (tipicamente um CNAME, mas ocasionalmente registros A ou MX) aponta para um recurso desativado ou inexistente em um provedor de nuvem de terceiros ou serviço de hospedagem. Os atacantes identificam esses registros DNS "dangling" (pendentes) procurando por mensagens de erro específicas de provedores como AWS, GitHub ou Azure que indicam que um recurso não é mais reivindicado. Ao registrar o nome do recurso abandonado na plataforma do provedor, o atacante obtém controle total sobre o subdomínio, permitindo a hospedagem de conteúdo malicioso, a exfiltração de cookies de sessão com escopo no domínio base e o bypass de proteções de Content Security Policy (CSP) ou CORS. Esta vulnerabilidade é particularmente perigosa porque utiliza a confiança associada à estrutura de domínio legítima da organização para facilitar phishing de alto impacto e roubo de dados.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica* |

> *A severidade torna-se Crítica se o subdomínio puder ser usado para sequestrar cookies de sessão do domínio base ou realizar o bypass de redirecionamentos de SSO/OAuth.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Ferramentas:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### A aplicação utiliza hospedagem de terceiros ou serviços de nuvem via subdomínios?
- [ ] Não — todos os subdomínios apontam para um espaço de IP controlado pela organização  
- [ ] Sim — os subdomínios apontam para provedores de terceiros (S3, GitHub Pages, Heroku, etc.)  

### Existem registros DNS "dangling" apontando para recursos inexistentes?
- [ ] Não — todos os registros DNS identificados resolvem para recursos ativos e controlados  
- [ ] Sim — existem registros CNAME ou A para recursos que **não existem mais** no provedor  
- [ ] Sim — os registros DNS resolvem para NXDOMAIN ou páginas de erro específicas do provedor *(ex: "NoSuchBucket")*  

### O recurso "dangling" identificado pode ser reivindicado com sucesso?
- [ ] Não — o provedor possui proteções contra a reivindicação de nomes abandonados ou é necessária a verificação da conta  
- [ ] Sim — o nome do recurso **pode** ser registrado na plataforma de terceiros por qualquer usuário  

### O domínio base utiliza cookies com escopo de "Domínio" que poderiam ser comprometidos?
- [ ] Não — os cookies têm escopo restrito a subdomínios específicos ou utilizam flags `Host-Only`  
- [ ] Sim — cookies sensíveis (IDs de sessão, JWTs) têm escopo no domínio pai e **podem** ser exfiltrados via um subdomínio sequestrado  

### O subdomínio pode ser utilizado para burlar cabeçalhos de segurança ou fluxos de autenticação?
- [ ] Não — não há dependências de segurança nos subdomínios identificados  
- [ ] Sim — o subdomínio está na whitelist das diretivas `script-src` ou `connect-src` do CSP  
- [ ] Sim — o subdomínio é uma URI de redirecionamento válida para configurações de OAuth ou SSO  

---