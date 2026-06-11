## WSTG-SESS-06 — Teste de Funcionalidade de Logout

A funcionalidade de logout é um controle de segurança crítico projetado para encerrar a sessão de um usuário e invalidar os identificadores de sessão associados, tanto no lado do cliente quanto no lado do servidor. A falha na implementação adequada do logout permite ataques de Session Fixation ou Session Hijacking, pois um invasor que obtenha acesso a uma máquina após um usuário ter feito "logout" poderia potencialmente reutilizar o token de sessão ainda ativo. Pentesters avaliam isso verificando se o cookie de sessão é removido do navegador, se o estado da sessão no lado do servidor é explicitamente destruído e se a navegação pelo botão "voltar" permite o acesso a dados sensíveis em cache. Uma implementação de logout segura garante que, uma vez acionada a ação, nenhuma requisição subsequente com o antigo token de sessão seja autorizada pela aplicação.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Ferramentas:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Existe um mecanismo de logout funcional e acessível?
- [ ] Sim — o botão de logout existe e aciona uma requisição de encerramento  
- [ ] Não — o botão de logout está **ausente** ou **não** aciona um evento de encerramento  

### O identificador de sessão é invalidado no lado do servidor?
- [ ] Sim — o servidor rejeita todas as requisições subsequentes usando o antigo token de sessão  
- [ ] Não — o servidor **continua a aceitar** o antigo token de sessão após o logout  

### A aplicação limpa os cookies de sessão no navegador ao realizar o logout?
- [ ] Sim — os cookies são sobrescritos com uma data de expiração passada ou valor vazio  
- [ ] Sim — os cookies permanecem, mas **não são mais válidos** no servidor  
- [ ] Não — os cookies permanecem no navegador e **mantêm** seus valores originais  

### Conteúdo autenticado sensível pode ser acessado através do botão 'Voltar' do navegador após o logout?
- [ ] Não — cabeçalhos `Cache-Control: no-store` ou similares impedem a visualização de páginas sensíveis após o logout  
- [ ] Sim — páginas sensíveis estão em cache e **podem** ser visualizadas por navegação após a sessão ser encerrada  

---