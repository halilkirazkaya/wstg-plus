## WSTG-INFO-10 — Mapear a Arquitetura da Aplicação

O mapeamento da arquitetura da aplicação envolve a identificação das tecnologias subjacentes, componentes de infraestrutura e caminhos de fluxo de dados que suportam a aplicação web. Ao analisar cabeçalhos de resposta HTTP, mensagens de erro, formatos de cookies e extensões de arquivo, os examinadores podem reconstruir um esquemático dos servidores web, servidores de aplicação, bancos de dados e integrações de terceiros em uso. Este conhecimento é fundamental para personalizar ataques subsequentes, pois permite a seleção de exploits específicos para a plataforma e a identificação de potenciais gargalos ou dispositivos intermediários mal configurados, como balanceadores de carga e WAFs. Um atacante utiliza este mapa estrutural para passar de uma varredura genérica para a exploração direcionada da stack de software específica e seus componentes interconectados.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Informativa / Baixa* |

> *A severidade torna-se Média se a topologia da rede interna ou endereços IP privados forem expostos.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### As tecnologias do servidor web e do servidor de aplicação podem ser identificadas com precisão?
- [ ] Não — nenhuma identificação é **possível** devido a hardening ou ofuscação eficazes  
- [ ] Sim — o tipo de tecnologia é conhecido, mas versões específicas **não podem** ser determinadas  
- [ ] Sim — a tecnologia e as versões específicas **são** totalmente expostas via cabeçalhos ou assinaturas de arquivo *(Informativa)*  

### Dispositivos intermediários (WAFs, Balanceadores de Carga, Proxies) são detectáveis no caminho de comunicação?
- [ ] Não — nenhum intermediário é detectado ou eles são totalmente transparentes  
- [ ] Sim — a existência é suspeitada via timing ou comportamento, mas a identidade **não é** confirmada  
- [ ] Sim — produtos específicos (ex: Cloudflare, Nginx, F5) **são** identificados via cabeçalhos ou comportamento únicos  

### A aplicação vaza informações sobre sua estrutura de diretórios internos ou topologia de rede?
- [ ] Não — caminhos internos e IPs **não são** expostos  
- [ ] Sim — caminhos internos são vazados em mensagens de erro ou stack traces, mas IPs internos **não são**  
- [ ] Sim — tanto as estruturas de diretórios internos quanto os endereços IP privados **são** expostos  

### O tipo e a versão do banco de dados backend podem ser inferidos a partir do comportamento da aplicação?
- [ ] Não — os detalhes do banco de dados **não podem** ser determinados  
- [ ] Sim — o tipo de banco de dados (ex: PostgreSQL, MSSQL) é inferido via mensagens de erro ou comportamento  
- [ ] Sim — o tipo e a versão do banco de dados **são** explicitamente expostos nos corpos das respostas ou cabeçalhos  

### A aplicação está hospedada em provedores de serviços em nuvem identificáveis ou CDNs de terceiros específicos?
- [ ] Não — a infraestrutura de hospedagem **não é** identificável  
- [ ] Sim — o provedor de nuvem (AWS, Azure, GCP) é identificado, mas serviços específicos **não são**  
- [ ] Sim — serviços de nuvem específicos (ex: buckets S3, Lambda, Azure Functions) **são** mapeados e acessíveis  

---