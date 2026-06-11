## WSTG-INFO-09 — Fingerprint da Aplicação Web

O Fingerprinting de uma aplicação web é o processo de identificar o framework específico, o Sistema de Gerenciamento de Conteúdo (CMS) ou a stack de tecnologia subjacente e suas versões associadas. Esta fase de reconhecimento é vital pois permite que um atacante mapeie os componentes identificados contra vulnerabilidades conhecidas (CVEs) e exploits públicos, reduzindo significativamente a superfície de ataque. As informações são normalmente colhidas a partir de cabeçalhos de resposta HTTP, caminhos de arquivo únicos, nomes de cookies, metadados de código-fonte HTML e hashes criptográficos de ativos estáticos. Ao determinar com precisão a stack de tecnologia, um atacante pode passar de uma varredura automatizada genérica para a exploração altamente direcionada de vulnerabilidades específicas da versão.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Informativa |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### Os cabeçalhos de resposta HTTP estão divulgando a stack de tecnologia ou números de versão?
- [ ] Não — cabeçalhos sensíveis são removidos ou contêm valores genéricos  
- [ ] Sim — cabeçalhos padrão como `X-Powered-By`, `Server`, ou `X-AspNet-Version` **estão** presentes  
- [ ] Sim — os cabeçalhos divulgam versões específicas e desatualizadas do servidor web ou framework da aplicação  

### O framework da aplicação ou CMS é identificável através de caminhos de arquivo únicos ou convenções de nomenclatura?
- [ ] Não — os caminhos de arquivo estão ofuscados e **não** revelam a tecnologia subjacente  
- [ ] Sim — estruturas de diretórios comuns (ex: `/wp-content/`, `/_next/`, `/node_modules/`) **estão** visíveis  
- [ ] Sim — arquivos únicos como `robots.txt`, `humans.txt` ou `sitemap.xml` contêm metadados específicos da tecnologia  

### Números de versão específicos estão expostos dentro do código-fonte HTML ou ativos estáticos?
- [ ] Não — nenhuma informação de versão é encontrada em comentários ou parâmetros de ativos  
- [ ] Sim — comentários HTML ou meta tags (ex: `<meta name="generator" content="...">`) **estão** presentes  
- [ ] Sim — strings de versão são anexadas aos nomes de arquivos CSS/JS (ex: `jquery.js?v=1.12.4`) e **não podem** ser desativadas  

### Páginas de erro padrão ou interfaces de gerenciamento revelam o ambiente de software?
- [ ] Não — a aplicação utiliza páginas de erro personalizadas e genéricas para todos os códigos de status  
- [ ] Sim — páginas de erro padrão do servidor web ou framework **estão** habilitadas e vazam dados de versão  
- [ ] Sim — portais de login administrativo (ex: `/wp-admin`, `/phpmyadmin`) **estão** acessíveis e identificáveis  

### Os cookies estão revelando o framework de gerenciamento de sessão?
- [ ] Não — os cookies de sessão utilizam nomes genéricos ou personalizados  
- [ ] Sim — nomes de cookies específicos da tecnologia (ex: `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`) **são** utilizados  

---