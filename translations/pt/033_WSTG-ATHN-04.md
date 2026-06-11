## WSTG-ATHN-04 — Teste de Bypass de Esquema de Autenticação

Burlar o esquema de autenticação (Bypassing authentication schema) envolve identificar e explorar falhas na lógica da aplicação que permitem que um atacante obtenha acesso a recursos protegidos sem fornecer credenciais válidas. Esta vulnerabilidade ocorre tipicamente quando a aplicação depende de controlos do lado do cliente (client-side), falha em impor a autorização no lado do servidor (server-side) em cada pedido, ou deixa "backdoors" administrativas e endpoints de depuração (debug) expostos em produção. Os atacantes exploram estas fraquezas através de navegação forçada (forced browsing) para URLs sensíveis, manipulação de parâmetros HTTP como `authenticated=true`, ou spoofing de cabeçalhos (headers) para enganar a aplicação e fazê-la assumir que uma sessão já foi estabelecida. A exploração bem-sucedida resulta na quebra completa do perímetro de autenticação, levando potencialmente à exfiltração não autorizada de dados ou ao comprometimento total do sistema.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Ferramentas:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Páginas internas sensíveis podem ser acedidas diretamente sem uma sessão ativa?
- [ ] Não — o acesso direto resulta em redirecionamento para o login ou uma resposta 403/404  
- [ ] Sim — algumas páginas sensíveis estão acessíveis, mas fornecem dados limitados ou não sensíveis  
- [ ] Sim — páginas administrativas sensíveis ou específicas do utilizador **estão** totalmente acessíveis sem autenticação *(Crítica)*  

### A aplicação depende de flags ou parâmetros no lado do cliente para determinar o status da autenticação?
- [ ] Não — o status da autenticação é gerido estritamente através do estado da sessão no lado do servidor (server-side)  
- [ ] Sim — existem flags no lado do cliente, mas a modificação **não** concede acesso a áreas protegidas  
- [ ] Sim — a modificação de parâmetros (ex: `is_authenticated=true`, `user_role=admin`) **permite** o bypass da autenticação  

### A autenticação pode ser burlada através de spoofing ou manipulação de cabeçalhos (headers) HTTP específicos?
- [ ] Não — cabeçalhos como `X-Forwarded-For`, `Referer`, ou cabeçalhos personalizados não têm impacto na autenticação  
- [ ] Sim — a manipulação de cabeçalhos (ex: `X-Custom-IP-Authorization`, `X-Remote-User`) **é possível** e concede acesso não autorizado  

### Existem caminhos alternativos ou artefactos de desenvolvimento que contornam o fluxo de login padrão?
- [ ] Não — todos os recursos protegidos são controlados por uma verificação de autenticação centralizada e pronta para produção  
- [ ] Sim — endpoints legados, rotas de depuração (ex: `/debug/login`), ou versões de API esquecidas **estão** acessíveis sem credenciais  

### A aplicação falha em reautenticar ou verificar sessões para ações de privilégio elevado?
- [ ] Não — ações de privilégio elevado ou sensíveis exigem um token de sessão novo ou válido  
- [ ] Sim — uma vez que um bypass é encontrado num endpoint, este **pode** ser usado para realizar ações administrativas em toda a aplicação  

---