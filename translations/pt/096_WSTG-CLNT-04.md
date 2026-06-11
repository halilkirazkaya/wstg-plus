## WSTG-CLNT-04 — Testing for Client-Side URL Redirect

O redirecionamento de URL no lado do cliente (Client-side URL redirection) ocorre quando uma aplicação web aceita entrada controlada pelo usuário — geralmente através de parâmetros de URL ou fragmentos de hash — e a utiliza dentro de um sink de redirecionamento no lado do cliente, como `window.location` ou `document.location.href`. Esta vulnerabilidade é explorada principalmente por atacantes para conduzir campanhas sofisticadas de phishing, pois a vítima confia inicialmente no domínio legítimo antes de ser redirecionada silenciosamente para um site malicioso. Além do phishing, redirecionamentos no lado do cliente podem frequentemente ser encadeados para exfiltrar dados sensíveis, como tokens de acesso OAuth ou identificadores de sessão, se o redirecionamento ocorrer enquanto esses valores estiverem presentes na URL ou no cabeçalho `Referer`. Em Single Page Applications (SPAs) modernas, essa vulnerabilidade aparece frequentemente em lógicas de roteamento, parâmetros "return to" após a autenticação ou funcionalidades de troca de idioma.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média* |

> *A severidade torna-se Alta se o redirecionamento puder ser usado para exfiltrar tokens OAuth, IDs de sessão ou contornar controles de segurança em um fluxo de autenticação.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Ferramentas:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### A aplicação utiliza scripts no lado do cliente para lidar com redirecionamentos baseados na entrada do usuário?
- [ ] Não — o redirecionamento é tratado exclusivamente no lado do servidor via códigos de status HTTP `3xx`  
- [ ] Sim — a aplicação utiliza sinks de JavaScript como `window.location` para navegar com base em parâmetros de URL  
- [ ] Sim — a aplicação utiliza um roteador de framework no lado do cliente (ex: React Router, Vue Router) que processa caminhos fornecidos pelo usuário  

### Existem controles de validação de entrada ou whitelisting implementados para a URL de destino?
- [ ] Sim — uma **whitelist** rigorosa de domínios/caminhos permitidos é aplicada e o bypass **não é possível**  
- [ ] Sim — a validação está presente, mas depende de regex fracas ou blacklists, e o bypass **é possível**  
- [ ] Não — a aplicação aceita qualquer string arbitrária como destino de redirecionamento  

### A lógica de redirecionamento pode ser contornada utilizando técnicas comuns de ofuscação?
- [ ] Não — os controles tratam corretamente caracteres codificados, URLs relativas ao protocolo (`//`) e barras invertidas  
- [ ] Sim — o bypass **é possível** utilizando URL encoding ou double-encoding  
- [ ] Sim — o bypass **é possível** utilizando URLs relativas ao protocolo ou caracteres `@` (ex: `https://expected.com@attacker.com`)  
- [ ] Sim — o bypass **é possível** utilizando comportamentos específicos do navegador (browser quirks) ou injeção de null byte  

### O processo de redirecionamento expõe dados sensíveis ao site de destino?
- [ ] Não — nenhum dado sensível está presente na URL ou é transmitido via cabeçalhos durante o redirecionamento  
- [ ] Sim — dados sensíveis (ex: tokens de sessão, chaves de API) **são vazados** via cabeçalho `Referer` para o site externo  
- [ ] Sim — dados sensíveis presentes no fragmento da URL (`#`) **estão acessíveis** para os scripts do site de destino  

### É possível executar JavaScript através do sink de redirecionamento (DOM-based XSS)?
- [ ] Não — o sink permite apenas navegação para os protocolos `http` ou `https`  
- [ ] Sim — o sink de redirecionamento permite URIs `javascript:` ou `data:`, tornando o DOM-based XSS **possível**  

---