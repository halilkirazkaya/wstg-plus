## WSTG-CLNT-13 — Teste para Cross Site Script Inclusion

A Cross-Site Script Inclusion (XSSI) ocorre quando uma aplicação web exporta dados sensíveis dentro de um arquivo JavaScript gerado dinamicamente ou resposta JSONP, que um site controlado por um atacante pode então incluir através de uma tag `<script>`. Como a inclusão de script ignora a Same-Origin Policy (SOP), um atacante pode executar o script em seu próprio contexto e sobrescrever objetos globais ou variáveis para exfiltrar informações sensíveis. Esta vulnerabilidade manifesta-se tipicamente em endpoints autenticados que retornam dados específicos do usuário, como endereços de e-mail, chaves de API ou metadados de sessão em formato de script. Do ponto de vista de um atacante, a exploração envolve a criação de uma página maliciosa que referencia o script vulnerável e captura as alterações resultantes em variáveis globais ou chamadas de função.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Média / Alta |

> *A gravidade torna-se Alta se os dados vazados incluírem tokens CSRF, identificadores de sessão ou PII (Informações de Identificação Pessoal) altamente sensíveis.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Algum endpoint autenticado retorna JavaScript dinâmico ou JSONP contendo informações sensíveis?
- [ ] Não — todos os scripts são estáticos e **não podem** conter dados específicos do usuário  
- [ ] Sim — existem scripts dinâmicos, mas **não contêm** informações sensíveis  
- [ ] Sim — scripts dinâmicos contêm PII ou tokens e **estão** acessíveis entre origens (cross-origin)  

### O cabeçalho `X-Content-Type-Options: nosniff` está implementado em respostas de scripts sensíveis?
- [ ] Sim — o cabeçalho está **aplicado** e evita o MIME-type sniffing  
- [ ] Não — o cabeçalho está **ausente**, permitindo potencialmente que conteúdo não-script seja executado como script  

### Os scripts sensíveis estão protegidos por mecanismos anti-XSSI, como prefixos não executáveis ou cabeçalhos personalizados?
- [ ] Sim — os scripts exigem um cabeçalho personalizado ou token que **não pode** ser enviado através de uma tag `<script>` padrão  
- [ ] Sim — os scripts utilizam prefixos não executáveis (ex: `)]}'`) que **não podem** ser facilmente contornados  
- [ ] Não — os scripts são diretamente acessíveis via requisições GET utilizando credenciais de ambiente (ambient credentials) *(Crítico)*  

### A exfiltração de dados sensíveis é possível através da sobrescrita de variáveis globais ou protótipos (prototypes)?
- [ ] Não — os dados sensíveis têm escopo local ou estão protegidos através de funcionalidades modernas do navegador  
- [ ] Sim — os dados sensíveis são atribuídos a variáveis globais e **podem** ser capturados  
- [ ] Sim — os dados são vazados através de callbacks JSONP que **podem** ser interceptados  

---