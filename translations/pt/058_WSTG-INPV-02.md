## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Stored Cross Site Scripting (XSS), também conhecido como Persistent XSS, ocorre quando uma aplicação recebe dados de um usuário e os armazena em um banco de dados persistente, sistema de arquivos ou outro meio de armazenamento sem a devida validação ou codificação (encoding). Quando uma vítima subsequentemente navega para uma página que recupera e exibe esses dados não sanitizados, o script malicioso é executado dentro do contexto do seu navegador sob a origem da aplicação. Esta vulnerabilidade é particularmente perigosa porque não exige que a vítima clique em um link especialmente manipulado; qualquer usuário que visualize a página afetada torna-se um alvo, levando potencialmente ao sequestro de sessão (session hijacking), ações não autorizadas ou coleta de credenciais (credential harvesting). Atacantes normalmente visam seções de comentários, perfis de usuários, fóruns de mensagens e logs administrativos onde a entrada é consistentemente renderizada para outros usuários ou administradores.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Ferramentas:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Existem pontos de entrada que armazenam a entrada do usuário para exibição posterior a outros usuários?
- [ ] Não — a aplicação não armazena entrada controlável pelo usuário para renderização posterior  
- [ ] Sim — a entrada é armazenada (ex: perfis, comentários), mas **não** é renderizada em um contexto HTML  
- [ ] Sim — a entrada é armazenada e renderizada para outros usuários ou administradores  

### A codificação de saída (output encoding) ou sanitização é aplicada quando os dados armazenados são renderizados?
- [ ] Sim — a codificação de saída sensível ao contexto (context-aware output encoding) **é aplicada** e o bypass **não é possível** *(Mais seguro)*  
- [ ] Sim — a sanitização (ex: `DOMPurify`) **é aplicada**, mas o bypass **é possível** através de erros de configuração  
- [ ] Não — os dados são renderizados diretamente no DOM **sem** codificação ou sanitização *(Alta)*  

### A validação de entrada ou sanitização pode ser burlada (bypassed) usando payloads alternativos?
- [ ] Não — os filtros lidam com várias codificações, tags não padrão e manipuladores de eventos (event handlers) de forma segura  
- [ ] Sim — o bypass **é possível** usando codificação de entidades HTML ou tags específicas (ex: `<svg>`, `<img>`)  
- [ ] Sim — o bypass **é possível** via payloads poliglotas (polyglot payloads) ou XSS baseado em mutação (mXSS)  

### Uma Política de Segurança de Conteúdo (CSP) fornece uma camada secundária de defesa?
- [ ] Sim — uma CSP estrita está **habilitada** e impede a execução de scripts inline e fontes não autorizadas  
- [ ] Sim — uma CSP está **habilitada**, mas está mal configurada (ex: `unsafe-inline` ou `script-src` amplo)  
- [ ] Não — nenhuma CSP está **habilitada** para a aplicação  

### É possível alcançar "Stored XSS para RCE" ou outras cadeias de alto impacto (Blind XSS)?
- [ ] Não — o impacto é limitado ao contexto do navegador no lado do cliente (client-side)  
- [ ] Sim — o stored XSS **pode** ser usado para visar administradores (Blind XSS) em painéis internos  
- [ ] Sim — o stored XSS **pode** ser encadeado com outras vulnerabilidades (ex: CSRF, exfiltração de dados sensíveis)  

---