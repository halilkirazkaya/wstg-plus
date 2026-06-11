## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

O Cross-Origin Resource Sharing (CORS) é um mecanismo baseado no navegador que permite a um servidor autorizar explicitamente o acesso aos seus recursos a partir de diferentes origens, relaxando efetivamente a Same-Origin Policy (SOP). Configurações incorretas ocorrem quando uma aplicação confia excessivamente em origens arbitrárias, geralmente refletindo o cabeçalho `Origin` no cabeçalho de resposta `Access-Control-Allow-Origin` ou utilizando whitelists de regex mal implementadas. Do ponto de vista de um atacante, uma política de CORS permissiva pode ser explorada hospedando um script malicioso em um domínio controlado que faz requisições autenticadas para a aplicação vulnerável, permitindo que o atacante realize a exfiltração de dados sensíveis, como tokens CSRF ou PII, diretamente do corpo da resposta. Esta vulnerabilidade é mais crítica em endpoints de API que processam sessões autenticadas e retornam informações não públicas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Média / Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Ferramentas:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### A aplicação implementa cabeçalhos CORS em endpoints autenticados?
- [ ] Não — O CORS **não está habilitado**; o navegador utiliza por padrão a Same-Origin Policy restrita  
- [ ] Sim — Os cabeçalhos CORS estão presentes e restritos a uma **whitelist** rigorosa  
- [ ] Sim — Os cabeçalhos CORS estão presentes, mas configurados de forma **permissiva**  

### Como o servidor manipula o cabeçalho `Access-Control-Allow-Origin` (ACAO)?
- [ ] O ACAO está configurado para um domínio estático específico e **confiável**  
- [ ] O ACAO está configurado como `*` (wildcard) e as credenciais **não podem** ser enviadas  
- [ ] O ACAO está configurado como `*` (wildcard) e as credenciais **podem** ser enviadas *(Nota: A maioria dos navegadores bloqueia isso)*  
- [ ] O ACAO **reflete dinamicamente** o valor do cabeçalho `Origin` a partir da requisição  

### O cabeçalho `Access-Control-Allow-Credentials` (ACAC) está definido como `true`?
- [ ] Não — O ACAC **não está presente** ou está definido como `false`  
- [ ] Sim — O ACAC está definido como `true`, mas o ACAO está **restrito** a origens confiáveis  
- [ ] Sim — O ACAC está definido como `true` e o ACAO **reflete** origens arbitrárias *(Crítico)*  

### A whitelist de origens pode ser burlada via manipulação de subdomínio ou origin null?
- [ ] Não — a whitelist é rigorosamente aplicada e **não pode** ser burlada  
- [ ] Sim — a whitelist aceita **qualquer** subdomínio do alvo (ex: `attacker.target.com`)  
- [ ] Sim — a whitelist aceita a origem `null` através de iframes com sandbox  
- [ ] Sim — a whitelist é vulnerável a bypasses de regex (ex: `target.com.attacker.com` ou `target.com-safe.com`)  

### A configuração de CORS permite a exfiltração de dados sensíveis?
- [ ] Não — os endpoints expostos **não** contêm dados sensíveis ou específicos da sessão  
- [ ] Sim — endpoints autenticados retornam PII, credenciais ou tokens CSRF que **podem** ser lidos por uma origem não autorizada  

---