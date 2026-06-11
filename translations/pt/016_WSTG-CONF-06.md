## WSTG-CONF-06 — Testar Métodos HTTP

O teste de métodos HTTP envolve a identificação de quais verbos são suportados pelo servidor web e pela aplicação para garantir que apenas as funcionalidades necessárias estejam expostas. Além dos padrões `GET` e `POST`, os servidores podem inadvertidamente habilitar métodos perigosos como `PUT`, `DELETE` ou `TRACE`, que podem permitir que atacantes realizem o upload de arquivos maliciosos, excluam conteúdo existente ou exfiltrem cookies de sessão via Cross-Site Tracing (XST). Pentesters examinam cabeçalhos de resposta como `Allow` ou `Public` e tentam burlar restrições usando técnicas de Method Overriding ou Verb Tampering para acessar funcionalidades não autorizadas. Este teste é crucial para o hardening da superfície de ataque e para prevenir configurações incorretas que poderiam levar ao comprometimento do servidor ou manipulação não autorizada de dados.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média* |

> *A severidade torna-se Alta se `PUT` ou `DELETE` permitirem a manipulação não autorizada de arquivos ou se `TRACE` facilitar a exfiltração de tokens de sessão.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Os métodos HTTP perigosos como `PUT` ou `DELETE` estão desabilitados em toda a aplicação?
- [ ] Sim — apenas métodos seguros (GET, POST, HEAD) **estão habilitados**  
- [ ] Não — `PUT` ou `DELETE` **estão habilitados**, mas exigem autenticação válida  
- [ ] Não — `PUT` ou `DELETE` **estão habilitados** e acessíveis sem autenticação *(Crítico)*  

### O método `TRACE` está desabilitado para prevenir Cross-Site Tracing (XST)?
- [ ] Sim — o método `TRACE` **está desabilitado** ou retorna um 405 Method Not Allowed  
- [ ] Não — o método `TRACE` **está habilitado**, mas não reflete cabeçalhos sensíveis  
- [ ] Não — o método `TRACE` **está habilitado** e reflete cabeçalhos `Cookie` ou `Authorization` *(Média)*  

### O HTTP Method Overriding (ex: `X-HTTP-Method-Override`) é suportado pelo servidor?
- [ ] Não — o servidor **não** processa cabeçalhos de substituição de método  
- [ ] Sim — o servidor processa cabeçalhos de substituição, mas os controles de acesso **são aplicados**  
- [ ] Sim — o servidor processa cabeçalhos de substituição e o bypass de controle de acesso **é possível**  

### O servidor responde de forma segura a métodos HTTP arbitrários ou malformados?
- [ ] Sim — o servidor retorna um 405 Method Not Allowed ou 501 Not Implemented  
- [ ] Não — o servidor retorna um 200 OK ou 500 Error para métodos arbitrários como `JEFF` ou `TEST`  
- [ ] Não — o uso de métodos arbitrários permite burlar filtros de autenticação ou autorização  

### O método `OPTIONS` está restrito ou configurado para minimizar a exposição de informações?
- [ ] Sim — `OPTIONS` está desabilitado ou retorna um cabeçalho `Allow` minimalista  
- [ ] Não — `OPTIONS` revela uma ampla gama de métodos habilitados, incluindo verbos DAV ou de depuração (debugging)  

---