## WSTG-CLNT-03 — HTML Injection

A Injeção de HTML (HTML Injection) ocorre quando uma aplicação falha ao sanitizar a entrada fornecida pelo usuário antes de renderizá-la no navegador, permitindo que um atacante injete tags HTML arbitrárias no Document Object Model (DOM) da página web. Embora seja semelhante ao Cross-Site Scripting (XSS), o objetivo principal da HTML Injection é manipular a apresentação visual da página ou facilitar ataques de phishing através da injeção de formulários maliciosos e conteúdo enganoso. Atacantes visam parâmetros que são refletidos na UI, como termos de busca, campos de perfil de usuário ou mensagens de erro, para enganar as vítimas e levá-las a divulgar credenciais sensíveis ou interagir com links de terceiros não autorizados. Esta vulnerabilidade representa um risco significativo para a integridade da interface de usuário da aplicação e pode ser um precursor para ataques de engenharia social ou ataques relacionados à sessão mais complexos.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### As entradas controladas pelo usuário são refletidas no DOM sem a devida codificação HTML?
- [ ] Não — toda entrada refletida é estritamente codificada em HTML (HTML-encoded) antes de ser renderizada  
- [ ] Sim — alguns caracteres **são** codificados, mas bypasses para certas tags **são possíveis**  
- [ ] Sim — a entrada é refletida de forma bruta (raw), e a HTML Injection **é possível**  

### Elementos estruturais de HTML podem ser injetados para alterar o layout da página?
- [ ] Não — a aplicação filtra ou remove tags estruturais como `<div>`, `<table>` ou `<iframe>`  
- [ ] Sim — elementos estruturais **podem** ser injetados, mas **não** impactam significativamente a UI  
- [ ] Sim — uma desfiguração (defacement) significativa da UI **é possível** através de tags injetadas  

### É possível injetar elementos funcionais de phishing, como formulários ou links maliciosos?
- [ ] Não — tags `<form>`, `<input>` e `<a>` são efetivamente bloqueadas ou sanitizadas  
- [ ] Sim — links maliciosos **podem** ser injetados para redirecionar usuários para sites externos  
- [ ] Sim — elementos `<form>` funcionais **podem** ser injetados para capturar credenciais *(Média)*  

### Cabeçalhos de segurança ou Políticas de Segurança de Conteúdo (CSP) mitigam o impacto da injeção?
- [ ] Sim — uma CSP restritiva está implementada e `form-action` ou `frame-src` **é aplicado**  
- [ ] Não — cabeçalhos de segurança estão presentes, mas a CSP **está desativada** ou contém unsafe-inline/wildcards  
- [ ] Não — nenhuma CSP ou cabeçalhos de segurança relevantes **estão ativados**  

---