## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

O Reflected Cross Site Scripting (XSS) ocorre quando uma aplicação inclui dados não confiáveis em uma resposta HTTP sem validação ou encoding suficiente, fazendo com que o payload seja executado no contexto do navegador da vítima. Atacantes entregam payloads maliciosos via engenharia social, tipicamente através de URLs ou formulários manipulados, para sequestrar sessões de usuário, exfiltrar cookies sensíveis ou realizar ações não autorizadas em nome do usuário. Esta vulnerabilidade é comumente encontrada em parâmetros de busca, mensagens de erro e qualquer endpoint que reflita a entrada diretamente de volta para a interface do usuário. A exploração depende da interação da vítima com um link malicioso, tornando-o um vetor de ataque não persistente, mas altamente eficaz para atingir usuários administrativos ou autenticados específicos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### As entradas fornecidas pelo usuário são refletidas no corpo da resposta?
- [ ] Não — a entrada **nunca** é refletida de volta para o usuário  
- [ ] Sim — a entrada é refletida, mas codificada corretamente e o bypass **não é possível**  
- [ ] Sim — a entrada é refletida **sem** qualquer codificação ou sanitização  

### A codificação de saída sensível ao contexto (context-aware output encoding) está implementada?
- [ ] Sim — a codificação adequada (HTML, Attribute, JavaScript) é **aplicada** com base no contexto específico da reflexão  
- [ ] Sim — a codificação é **aplicada**, mas é insuficiente para o contexto específico (ex: codificação HTML dentro de uma tag `<script>`)  
- [ ] Não — a codificação **não é aplicada**  

### Filtros de validação de entrada ou de Web Application Firewall (WAF) podem ser burlados?
- [ ] Não — os filtros bloqueiam efetivamente todos os payloads de XSS comuns e ofuscados  
- [ ] Sim — os filtros estão presentes, mas o bypass **é possível** utilizando codificação de caracteres, tags não padronizadas ou polyglots  
- [ ] Não — não há filtros ou mecanismos de validação implementados  

### Qual é o contexto de execução da entrada refletida?
- [ ] Seguro — a entrada é refletida em um local não executável (ex: dentro de tags `<div>` ou `<span>` padrão)  
- [ ] Arriscado — a entrada é refletida dentro de atributos HTML ou dentro de `src`/`href` de tags  
- [ ] Crítico — a entrada é refletida diretamente dentro de blocos `<script>`, manipuladores de eventos (event handlers) ou template literals  

### Cabeçalhos de segurança de navegadores modernos estão presentes para mitigar o impacto de XSS?
- [ ] Sim — `Content-Security-Policy` (CSP) está **habilitado** com um script-src restritivo  
- [ ] Sim — `Content-Security-Policy` (CSP) está **habilitado**, mas contém `unsafe-inline` ou whitelists fracas  
- [ ] Não — não há CSP ou cabeçalhos legados `X-XSS-Protection` presentes  

---