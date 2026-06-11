## WSTG-INPV-03 — Teste de HTTP Verb Tampering

O HTTP Verb Tampering explora fraquezas na forma como servidores web e frameworks de aplicação autorizam o acesso a recursos específicos com base no método HTTP utilizado. Atacantes tentam burlar restrições de segurança substituindo métodos padrão como `GET` ou `POST` por alternativas como `HEAD`, `PUT`, `OPTIONS`, ou até mesmo strings arbitrárias e não padronizadas que o backend pode processar de forma inconsistente. Esta vulnerabilidade ocorre tipicamente quando configurações de segurança — como filtros web.xml do Java EE ou regras de autorização .NET — listam explicitamente métodos permitidos, mas falham ao não considerar outros, ou quando uma abordagem de "negar por método" (deny-by-method) é usada em vez de "negar tudo" (deny-all). A exploração bem-sucedida pode resultar em acesso não autorizado a funções administrativas, modificação de dados ou divulgação de informações sobre a configuração interna do servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se funções administrativas ou modificação de dados (PUT/DELETE) puderem ser realizadas sem autenticação.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### A aplicação responde a métodos HTTP não padronizados ou arbitrários?
- [ ] Não — a aplicação rejeita todos os métodos, exceto aqueles explicitamente necessários  
- [ ] Sim — a aplicação responde a métodos padrão (OPTIONS, TRACE), mas **não consegue** burlar a segurança  
- [ ] Sim — a aplicação aceita strings arbitrárias (ex: FOO, TEST) e as processa como requisições `GET` ou `POST`  

### A autenticação/autorização pode ser burlada alterando o método HTTP?
- [ ] Não — os controles de segurança são aplicados globalmente, independentemente do verbo HTTP  
- [ ] Sim — os controles existem, mas o bypass **é possível** utilizando o método `HEAD`  
- [ ] Sim — os controles de segurança **não são aplicados** a métodos alternativos, permitindo acesso não autorizado a endpoints restritos  

### Métodos perigosos como `PUT` ou `DELETE` estão habilitados no servidor?
- [ ] Não — os métodos perigosos estão **desabilitados** ou retornam um 405 Method Not Allowed  
- [ ] Sim — os métodos estão habilitados, mas exigem autenticação válida de alto privilégio  
- [ ] Sim — os métodos estão **habilitados** e permitem upload de arquivos não autorizado ou exclusão de recursos *(Crítico)*  

### O método `OPTIONS` revela informações sensíveis sobre verbos permitidos?
- [ ] Não — o `OPTIONS` está **desabilitado** ou retorna uma resposta genérica  
- [ ] Sim — o `OPTIONS` retorna o cabeçalho `Allow`, mas não expõe métodos internos restritos  
- [ ] Sim — o `OPTIONS` revela métodos apenas internos ou administrativos que auxiliam em explorações futuras  

### O método `TRACE` está habilitado, permitindo potencialmente Cross-Site Tracing (XST)?
- [ ] Não — os métodos `TRACE` e `TRACK` estão **desabilitados**  
- [ ] Sim — o `TRACE` está **habilitado** e reflete cabeçalhos HTTP, incluindo cookies/tokens sensíveis  

---