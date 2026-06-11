## WSTG-CLNT-06 — Teste de Manipulação de Recursos no Lado do Cliente

A manipulação de recursos no lado do cliente (Client-side resource manipulation) ocorre quando uma aplicação permite que entradas controladas pelo usuário influenciem a origem ou o caminho de recursos carregados pelo navegador, tais como scripts, folhas de estilo (stylesheets) ou iframes. Ao manipular fragmentos de URI, parâmetros de consulta (query parameters) ou outras fontes de DOM, um atacante pode coagir a aplicação a carregar conteúdo externo malicioso ou executar código não autorizado. Esta vulnerabilidade é tipicamente encontrada em lógica JavaScript que preenche dinamicamente os atributos `src` ou `href` de elementos HTML sem validação suficiente contra uma lista de permissões (allow-list) confiável. Do ponto de vista de um atacante, isso é explorado ao criar uma URL que redireciona o carregamento de recursos para um servidor controlado pelo atacante, resultando potencialmente em Cross-Site Scripting (XSS) baseado em DOM (DOM-based XSS), exfiltração de dados sensíveis ou UI redressing.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Ferramentas:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### A aplicação utiliza sinks no lado do cliente para carregar ou incorporar recursos dinamicamente?
- [ ] Não — os recursos são carregados apenas via HTML estático ou lógica no lado do servidor  
- [ ] Sim — o JavaScript no lado do cliente preenche dinamicamente atributos de recursos como `src`, `href` ou `data`  

### Os controles de validação ou listas de permissões (allow-lists) estão implementados para as fontes de URI de recursos?
- [ ] Sim — listas de permissões (allow-lists) rigorosas e validação de origem **são aplicadas** a todos os recursos dinâmicos  
- [ ] Sim — a validação está presente, mas depende de listas de bloqueio (blacklists) fracas ou regex facilmente contornáveis  
- [ ] Não — nenhum controle de validação é aplicado aos caminhos de recursos controlados pelo usuário  

### É possível contornar a validação de URI usando protocolos alternativos ou codificação?
- [ ] Não — o bypass de filtros de recursos **não é possível**  
- [ ] Sim — o bypass **é possível** usando diferentes protocolos como `data:`, `vbscript:` ou `javascript:`  
- [ ] Sim — o bypass **é possível** usando caracteres codificados ou null bytes para burlar correspondências de strings simples  

### Um atacante pode redirecionar o carregamento de recursos para um domínio externo não confiável?
- [ ] Não — os caminhos de recursos estão restritos à mesma origem (same origin) ou a um subdiretório fixo  
- [ ] Sim — a aplicação **pode** ser forçada a carregar scripts ou estilos de um domínio externo arbitrário  

### Qual é o impacto máximo da manipulação de recursos?
- [ ] Baixo — a manipulação afeta apenas elementos não executáveis, como imagens ou texto não sensível  
- [ ] Médio — a manipulação permite Injeção de CSS (CSS injection) ou UI redressing significativo  
- [ ] Alto — a manipulação leva a XSS baseado em DOM (DOM-based XSS) ou execução de JavaScript arbitrário *(Crítico)*  

---