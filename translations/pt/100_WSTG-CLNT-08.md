## WSTG-CLNT-08 — Teste de Cross Site Flashing

O Cross-Site Flashing (XSF) é uma vulnerabilidade client-side que ocorre quando um arquivo Flash (SWF) processa incorretamente entradas controladas pelo usuário, permitindo que um atacante execute código ActionScript malicioso ou estabeleça uma ponte para o ambiente JavaScript do navegador. Ao manipular parâmetros como `FlashVars` ou strings de consulta de URL passadas para sinks como `ExternalInterface.call`, `getURL` ou `loadMovie`, um atacante pode realizar ações no contexto do domínio vulnerável, incluindo session hijacking e data exfiltration. Esta vulnerabilidade é encontrada principalmente em aplicações corporativas legadas que ainda hospedam arquivos SWF, onde a validação de entrada insuficiente permite que o objeto Flash seja reaproveitado como um vetor para Cross-Site Scripting (XSS) ou para contornar a Same-Origin Policy (SOP) por meio de configurações de cross-domain permissivas.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Arquivos legados Adobe Flash (.swf) estão hospedados na aplicação?
- [ ] Não — nenhum arquivo Flash está hospedado ou é referenciado dentro do escopo da aplicação  
- [ ] Sim — arquivos SWF estão presentes, mas servem apenas conteúdo estático  
- [ ] Sim — arquivos SWF estão presentes e aceitam parâmetros dinâmicos via `FlashVars` ou strings de URL  

### A entrada passada para sinks de ActionScript é sanitizada?
- [ ] Sim — todas as entradas são estritamente validadas e os sinks de ActionScript **não podem** ser manipulados  
- [ ] Sim — a validação é aplicada, mas o bypass **é possível** por meio de técnicas específicas de encoding  
- [ ] Não — a entrada é passada diretamente para sinks sensíveis como `ExternalInterface.call` ou `getURL` *(Crítico)*  

### O arquivo Flash pode ser usado para executar JavaScript arbitrário (XSS)?
- [ ] Não — `ExternalInterface` está **desabilitado** ou o parâmetro `allowScriptAccess` está definido como `never`  
- [ ] Sim — `allowScriptAccess` está definido como `sameDomain`, mas o SWF está hospedado no domínio alvo  
- [ ] Sim — `allowScriptAccess` está definido como `always`, permitindo XSS a partir de qualquer domínio  

### A política `crossdomain.xml` impede requisições cross-origin não autorizadas?
- [ ] Sim — o `crossdomain.xml` é restritivo e permite apenas origens específicas e confiáveis  
- [ ] Não — o arquivo `crossdomain.xml` **está ausente**  
- [ ] Sim — a política é excessivamente permissiva (ex: `<allow-access-from domain="*" />`)  

### O arquivo SWF pode ser forçado a carregar filmes externos controlados por um atacante?
- [ ] Não — a aplicação **não pode** ser forçada a carregar arquivos SWF externos  
- [ ] Sim — as funções `loadMovie` ou `loadMovieNum` aceitam URLs externas não validadas  

---