## WSTG-INFO-01 — Realizar Descoberta e Reconhecimento em Motores de Busca para Vazamento de Informações

A descoberta em motores de busca envolve o aproveitamento de mecanismos de busca públicos, páginas em cache e serviços de indexação para identificar informações que a organização-alvo expôs involuntariamente. Atacantes utilizam operadores de busca avançados (Google Dorks) e serviços de indexação de terceiros para descobrir arquivos sensíveis, diretórios, portais de login, mensagens de erro e metadados que não deveriam estar acessíveis publicamente. Esta fase de reconhecimento é crítica porque requer interação direta zero com o alvo, tornando-a virtualmente indetectável, ao mesmo tempo que pode revelar pontos de entrada de alto valor, tais como painéis de administração expostos, arquivos de configuração, dumps de banco de dados e documentação interna.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Ferramentas:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Arquivos ou diretórios sensíveis estão indexados por motores de busca?
- [ ] Não — nenhum conteúdo sensível foi encontrado nos resultados dos motores de busca  
- [ ] Sim — arquivos de configuração, backups ou documentos internos **estão** indexados *(Crítico)*  

### O Google Dorking revela interfaces administrativas ou de login expostas?
- [ ] Não — painéis de administração e páginas de login **não** estão indexados  
- [ ] Sim — painéis de administração **estão** indexados, mas exigem autenticação multifator forte  
- [ ] Sim — painéis de administração **estão** indexados e acessíveis publicamente **sem** controles suficientes  

### Versões em cache de páginas estão expondo dados sensíveis que foram removidos desde então?
- [ ] Não — nenhum dado sensível foi encontrado em snapshots em cache  
- [ ] Sim — páginas em cache contêm credenciais, endereços IP internos ou informações sensíveis  

### O arquivo `robots.txt` está divulgando caminhos sensíveis involuntariamente?
- [ ] Não — o `robots.txt` **não** revela estruturas de diretórios sensíveis  
- [ ] Sim — o `robots.txt` lista caminhos sensíveis que auxiliam no reconhecimento do atacante  
- [ ] Não existe o arquivo `robots.txt`  

### Serviços de terceiros (`Shodan`, `Censys`, `Wayback Machine`) estão revelando exposição histórica ou atual?
- [ ] Não — sem descobertas significativas em serviços de indexação de terceiros  
- [ ] Sim — snapshots históricos ou varreduras de serviços revelam metadados sensíveis ou banners de serviços

---

## WSTG-INFO-02 — Fingerprint Web Server

O fingerprinting de servidor web é o processo de identificar o tipo específico de software, a versão e o sistema operacional subjacente de um servidor web alvo. Os atacantes realizam este reconhecimento para identificar vulnerabilidades conhecidas, como CVEs não corrigidos ou fraquezas de configuração, que são específicos para certas versões de Apache, Nginx, IIS ou outras tecnologias de servidor. Isso é normalmente alcançado através da análise de cabeçalhos de resposta HTTP (ex: `Server`, `X-Powered-By`), do exame de comportamentos únicos de protocolos ou da observação de informações vazadas por meio de arquivos e páginas de erro padrão. Um fingerprinting bem-sucedido permite que um atacante adapte sua estratégia de Exploit, passando de um escaneamento genérico para ataques de alta probabilidade contra falhas arquiteturais conhecidas.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Informativo / Baixa* |

> *A severidade torna-se Alta se o fingerprinting revelar uma versão de servidor obsoleta ou em fim de vida (EOL) com vulnerabilidades críticas conhecidas.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Ferramentas:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### As strings de identificação estão presentes nos cabeçalhos de resposta HTTP?
- [ ] Não — os cabeçalhos `Server` e `X-Powered-By` **não estão presentes** ou contêm valores genéricos  
- [ ] Sim — os cabeçalhos revelam o software do servidor, mas **não** os números de versão específicos  
- [ ] Sim — os cabeçalhos revelam o software do servidor específico e os números de versão **exatos**  

### As páginas de erro padrão ou respostas geradas pelo sistema vazam detalhes do servidor?
- [ ] Não — páginas de erro personalizadas são usadas e **não** divulgam informações do servidor  
- [ ] Sim — as páginas de erro padrão vazam o nome e/ou a versão do software do servidor  
- [ ] Sim — as páginas de erro vazam detalhes do servidor, versão e o **sistema operacional subjacente**  

### A versão do servidor é exposta através de arquivos padrão ou estruturas de diretórios?
- [ ] Não — arquivos de instalação padrão, manuais e scripts de teste foram **removidos**  
- [ ] Sim — arquivos padrão (ex: `info.php`, `manual/`, `test.html`) estão **presentes** e divulgam dados da versão  

### O tipo de servidor pode ser inferido com precisão através da análise de comportamento único?
- [ ] Não — as respostas do servidor são normalizadas e o fingerprinting baseado em comportamento **não é possível**  
- [ ] Sim — comportamentos únicos (ex: ordenação de cabeçalhos, códigos de erro específicos ou peculiaridades da pilha TCP/IP) **permitem** a identificação do servidor  

### A versão do servidor identificada é atualmente suportada e está corrigida (patched)?
- [ ] Sim — a versão identificada é atual e **suportada** pelo fornecedor  
- [ ] Não — a versão identificada está **desatualizada** ou em **fim de vida (EOL)** com vulnerabilidades conhecidas

---

## WSTG-INFO-03 — Revisão de Meta-arquivos do Servidor Web para Vazamento de Informações

Meta-arquivos de servidor web, como `robots.txt`, `sitemap.xml` e `security.txt`, destinam-se a orientar crawlers e fornecer metadados administrativos, mas frequentemente revelam inadvertidamente estruturas de diretórios sensíveis ou endpoints ocultos. Atacantes analisam esses arquivos para enumerar a superfície de ataque da aplicação, identificando diretivas "Disallow" que frequentemente apontam para painéis administrativos não vinculados, diretórios de backup ou ambientes de desenvolvimento. Além das instruções para mecanismos de busca, arquivos no diretório `.well-known/` ou arquivos como `humans.txt` podem expor stacks de tecnologia, informações de desenvolvedores ou contatos de segurança organizacional. A revisão sistemática desses arquivos permite que um testador obtenha insights estruturais e técnicos sem realizar uma descoberta agressiva por Brute Force.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Baixa / Informativa* |

> *A gravidade torna-se Média se os meta-arquivos revelarem interfaces administrativas não autenticadas ou backups de configuração sensíveis.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### O arquivo `robots.txt` existe e expõe diretórios sensíveis?
- [ ] Não — o `robots.txt` **não** existe  
- [ ] Sim — o `robots.txt` existe, mas contém apenas caminhos públicos padrão  
- [ ] Sim — o `robots.txt` revela caminhos de diretórios sensíveis ou interfaces administrativas  
- [ ] Sim — o `robots.txt` revela credenciais ou versões específicas de software em comentários  

### Um arquivo `sitemap.xml` está presente e revela uma estrutura oculta da aplicação?
- [ ] Não — o `sitemap.xml` **não** existe  
- [ ] Sim — o `sitemap.xml` está presente, mas lista apenas URLs voltadas para o público  
- [ ] Sim — o `sitemap.xml` lista endpoints não vinculados ou recursos apenas internos que **podem** ser acessados  

### Os meta-arquivos de segurança e organizacionais estão expondo detalhes técnicos?
- [ ] Não — nenhum arquivo de metadados encontrado na raiz ou em `.well-known/`  
- [ ] Sim — `security.txt` ou `humans.txt` estão presentes, mas contêm informações padrão  
- [ ] Sim — os meta-arquivos expõem hostnames internos, identidades de desenvolvedores ou detalhes da stack de tecnologia que **podem** auxiliar em engenharia social ou ataques posteriores  

### Existem meta-arquivos legados ou específicos de framework presentes?
- [ ] Não — nenhum arquivo específico de framework (ex: `info.php`, `composer.json`, `package.json`) encontrado  
- [ ] Sim — arquivos como `README.md`, `CHANGELOG.txt` ou `.DS_Store` estão presentes, mas sanitizados  
- [ ] Sim — arquivos específicos de framework ou versão estão presentes e **permitem** um fingerprinting preciso da tecnologia

---

## WSTG-INFO-04 — Enumerar Aplicações no Servidor Web

A enumeração de aplicações é o processo de identificar todas as aplicações e serviços web hospedados em um único servidor web ou endereço IP. Como os servidores web modernos frequentemente hospedam múltiplos hosts virtuais (Virtual Hosts), ambientes de teste (staging) legados ou interfaces administrativas em portas e caminhos não padrão, a falha em identificar esses ativos ocultos deixa uma parte significativa da superfície de ataque (attack surface) sem testes. Atacantes utilizam técnicas como transferências de zona DNS (DNS zone transfers), virtual host brute-forcing e pesquisas de IP reverso (reverse IP lookups) para descobrir esses pontos de entrada esquecidos ou obscurecidos, que são frequentemente menos seguros do que a aplicação de produção principal.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Informativa / Baixa |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Ferramentas:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Múltiplos endereços IP ou aliases de DNS estão associados à infraestrutura alvo?
- [ ] Não — apenas um único IP e registro A (A-record) existem  
- [ ] Sim — múltiplos endereços IP ou aliases **foram** identificados, expandindo o escopo  

### O servidor web hospeda múltiplos Hosts Virtuais (vhosts) no mesmo IP?
- [ ] Não — apenas o domínio primário é servido pelo servidor  
- [ ] Sim — hosts virtuais adicionais foram identificados, mas o acesso **não é possível** (ex: 403 Forbidden)  
- [ ] Sim — hosts virtuais ocultos, de desenvolvimento ou de staging (homologação) **foram** identificados e estão acessíveis  

### Existem aplicações hospedadas em portas não padrão?
- [ ] Não — apenas portas padrão (80/443) estão abertas  
- [ ] Sim — portas não padrão estão abertas, mas **não** hospedam aplicações web  
- [ ] Sim — aplicações web adicionais **foram** identificadas em portas não padrão (ex: 8080, 8443, 9000)  

### Aplicações distintas estão mapeadas para caminhos de URI específicos?
- [ ] Não — uma única aplicação serve todo o diretório raiz  
- [ ] Sim — múltiplas aplicações **foram** descobertas através de enumeração de diretórios (ex: `/api`, `/portal`, `/backup`)  

### Serviços de reconhecimento de terceiros revelam aplicações históricas ou secundárias?
- [ ] Não — sem descobertas significativas no Shodan, Censys ou Wayback Machine  
- [ ] Sim — snapshots históricos ou escaneamentos de serviço revelam aplicações que **estão** atualmente ativas, mas não vinculadas

---

## WSTG-INFO-05 — Revisão do Conteúdo da Página Web para Vazamento de Informações

A revisão do conteúdo da página web envolve a inspeção manual e automatizada das respostas da aplicação, incluindo arquivos HTML, JavaScript e CSS, para identificar informações sensíveis expostas involuntariamente aos usuários. Os atacantes analisam o código-fonte do lado do cliente (client-side) em busca de comentários de desenvolvedores, campos de formulário ocultos, caminhos internos do servidor, chaves de API (API keys) codificadas (hardcoded) e informações de depuração (debugging) que possam auxiliar em fases posteriores de exploração. Esse vazamento ocorre frequentemente quando os desenvolvedores esquecem de remover metadados ou código de depuração antes de mover para a produção, potencialmente revelando falhas de lógica ou detalhes da infraestrutura de backend. Do ponto de vista de um atacante, isso fornece um método de baixo esforço para mapear a estrutura interna da aplicação e descobrir pontos de entrada negligenciados sem acionar alertas de segurança ou interagir com a lógica de backend do servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média* |

> *A severidade torna-se Alta se credenciais codificadas (hardcoded), chaves privadas ou variáveis de ambiente internas sensíveis forem descobertas.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Ferramentas:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Existem comentários de desenvolvedores contendo informações sensíveis no código-fonte?
- [ ] Não — nenhum comentário ou apenas comentários genéricos e não sensíveis foram encontrados  
- [ ] Sim — existem comentários, mas **não** contêm lógica ou dados sensíveis  
- [ ] Sim — os comentários **revelam** caminhos internos, consultas SQL ou instruções administrativas  
- [ ] Sim — os comentários **revelam** credenciais ou notas sensíveis de desenvolvedores *(Alta)*  

### Os campos de entrada HTML ocultos contêm dados sensíveis ou informações de estado?
- [ ] Não — nenhum campo oculto encontrado ou eles contêm apenas tokens não sensíveis  
- [ ] Sim — campos ocultos são usados para estado, mas estão **criptografados** ou **assinados**  
- [ ] Sim — os campos ocultos contêm senhas em **texto simples**, funções de usuário (user roles) ou IDs internos  

### Existem segredos codificados (hardcoded), chaves de API ou endereços IP internos presentes em arquivos JavaScript?
- [ ] Não — nenhuma string sensível ou segredos encontrados em arquivos JS  
- [ ] Sim — os arquivos JS contêm chaves de API públicas com restrições **adequadas**  
- [ ] Sim — os arquivos JS contêm chaves de API **não protegidas**, IPs internos ou endpoints privados  
- [ ] Sim — os arquivos JS contêm credenciais administrativas ou chaves privadas **codificadas (hardcoded)** *(Crítica)*  

### A aplicação revela versões de frameworks ou caminhos de arquivos internos no código-fonte?
- [ ] Não — as informações do framework e caminhos de arquivos estão **minificados** ou **ofuscados**  
- [ ] Sim — os nomes e versões dos frameworks estão **visíveis**, mas corrigidos (patched)  
- [ ] Sim — caminhos de arquivos internos ou caminhos absolutos do servidor estão **expostos** no código-fonte  

### O código-fonte está propenso a vazamentos devido à falta de minificação ou ofuscação?
- [ ] Não — o código de produção está **totalmente minificado** e sem comentários  
- [ ] Sim — o código está minificado, mas os comentários **permanecem** no código-fonte  
- [ ] Sim — os source maps (arquivos `.map`) estão **publicamente acessíveis**, permitindo a reconstrução total do código-fonte

---

## WSTG-INFO-06 — Identificar Pontos de Entrada da Aplicação

A identificação de pontos de entrada da aplicação envolve o mapeamento de toda a superfície de ataque ao enumerar todas as formas possíveis pelas quais um utilizador ou sistema externo pode interagir com a aplicação. Este processo inclui a descoberta de todos os URLs, endpoints de API, parâmetros (GET, POST, baseados em cookies), cabeçalhos HTTP e protocolos especializados como WebSockets ou gRPC. Para um penetration tester, esta fase é vital porque vulnerabilidades são frequentemente encontradas em APIs "shadow" não documentadas, endpoints legados ou estruturas de parâmetros complexas que foram ignoradas durante o desenvolvimento. Uma enumeração minuciosa garante que nenhum componente da aplicação permaneça como uma "black box" não testada, prevenindo assim que atacantes encontrem rotas não monitorizadas para o sistema.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Estado do Teste** | Não Realizado |
| **Severidade** | Informativo |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Todos os parâmetros GET e POST foram enumerados em toda a aplicação?
- [ ] Sim — a enumeração abrangente através de crawling automatizado e manual **está concluída**  
- [ ] Sim — apenas parâmetros comuns foram identificados através de crawling padrão  
- [ ] Não — os parâmetros estão em grande parte **não identificados** ou não testados  

### Parâmetros não documentados ou ocultos (ex: debug, admin) são procurados utilizando Fuzzing?
- [ ] Não — a descoberta de parâmetros ocultos **não é necessária** para este âmbito específico  
- [ ] Sim — o Fuzzing de parâmetros (ex: utilizando `Arjun`) **foi realizado** sem descobertas  
- [ ] Sim — parâmetros ocultos **foram descobertos** e mapeados para testes adicionais  
- [ ] Não — a descoberta de parâmetros ocultos **não foi** realizada  

### Todos os métodos HTTP suportados (PUT, DELETE, PATCH, etc.) foram identificados para cada endpoint?
- [ ] Sim — a descoberta de métodos **é aplicada** em todos os endpoints descobertos  
- [ ] Sim — a descoberta de métodos **é aplicada** apenas a endpoints de alto interesse ou sensíveis  
- [ ] Não — apenas os métodos padrão GET e POST **são identificados**  

### Os pontos de entrada não padronizados como WebSockets, gRPC ou cabeçalhos personalizados estão mapeados?
- [ ] Não — a aplicação **não utiliza** protocolos não padronizados ou cabeçalhos personalizados  
- [ ] Sim — todos os pontos de entrada específicos de protocolo e cabeçalhos personalizados **estão identificados**  
- [ ] Não — pontos de entrada não padronizados **existem**, mas não foram mapeados  

### A superfície da API (incluindo endpoints com versão como /v1/ ou /v2/) foi totalmente mapeada?
- [ ] Não — a aplicação **não expõe** uma API  
- [ ] Sim — todas as versões de API e endpoints **estão identificados** e documentados  
- [ ] Sim — apenas a versão atual da API de produção **está identificada**  
- [ ] Não — os endpoints de API **permanecem não mapeados** ou apenas parcialmente descobertos

---

## WSTG-INFO-07 — Mapear Caminhos de Execução na Aplicação

O mapeamento dos caminhos de execução envolve a identificação sistemática de todos os endpoints acessíveis, fluxos funcionais e ramificações de tomada de decisão dentro de uma aplicação web. Este processo é vital para garantir que nenhuma funcionalidade "obscura" — como código legado, interfaces de depuração (debug) ou rotas de API não documentadas — permaneça oculta da análise de segurança, uma vez que estas frequentemente carecem de controles de segurança modernos. Os atacantes utilizam uma combinação de spidering automatizado e exploração manual para visualizar a estrutura da aplicação, frequentemente descobrindo vulnerabilidades como acesso administrativo não autorizado ou falhas de lógica de negócio durante o processo. Ao correlacionar entradas com respostas específicas do servidor, os testadores podem identificar lógica de processamento sensível que requer testes de análise profunda direcionados para garantir uma cobertura robusta.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Informativo |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### A aplicação foi totalmente indexada usando crawling automatizado e manual?
- [ ] Sim — o mapeamento abrangente de todos os recursos visíveis e vinculados **está concluído**  
- [ ] Sim — o crawling automatizado **está concluído**, mas a exploração manual está pendente  
- [ ] Não — a aplicação **não** foi indexada  

### Endpoints ocultos ou não documentados foram identificados através de análise de JS ou brute-forcing de diretórios?
- [ ] Não — nenhum caminho não documentado foi descoberto via análise de canais secundários  
- [ ] Sim — caminhos ocultos foram descobertos, mas eles **não podem** ser acessados sem autorização  
- [ ] Sim — endpoints não documentados **estão** acessíveis e fornecem funcionalidades sensíveis *(Crítica / Alta)*  

### Os caminhos de execução podem ser alterados via parâmetros ou cabeçalhos não padronizados?
- [ ] Não — parâmetros como `debug`, `test` ou `admin` **não** afetam o fluxo de execução  
- [ ] Sim — os parâmetros alteram a saída, mas **não** burlam os controles de segurança  
- [ ] Sim — parâmetros de alteração de caminho **são aplicados** e permitem burlar a lógica pretendida  

### O fluxo de lógica de negócio de múltiplas etapas (ex: autenticação de dois fatores, checkout) está totalmente mapeado?
- [ ] Sim — todos os estados e transições em processos de múltiplas etapas **estão identificados**  
- [ ] Não — transições complexas de máquina de estados **não podem** ser totalmente mapeadas  

### Arquivos de documentação de API (Swagger/OpenAPI) ou mapas do lado do cliente (Source Maps) estão expostos?
- [ ] Não — nenhum mapa arquitetural ou arquivo de documentação está exposto publicamente  
- [ ] Sim — a documentação está presente, mas **está protegida** por autenticação  
- [ ] Sim — Swagger UI, OpenAPI JSON ou JS Source Maps **estão habilitados** e acessíveis publicamente

---

## WSTG-INFO-08 — Fingerprinting de Framework de Aplicação Web

O fingerprinting de um framework de aplicação web envolve a identificação da stack tecnológica específica e das versões de software utilizadas para construir e servir a aplicação. Atacantes analisam cabeçalhos de resposta HTTP, cookies específicos do framework, estruturas de arquivos previsíveis e artefatos de JavaScript exclusivos para determinar se o alvo está executando plataformas como React, Angular, Django ou Spring. Esta fase de reconhecimento é vital pois permite que um testador restrinja a superfície de ataque a vulnerabilidades conhecidas (CVEs) e fraquezas de configuração comuns específicas para aquele framework. Ao compreender a arquitetura subjacente, um atacante pode passar de testes genéricos para estratégias de Exploit altamente direcionadas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Informativa |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Os cabeçalhos de resposta HTTP estão expondo o nome ou a versão do framework?
- [ ] Não — cabeçalhos como `X-Powered-By` ou `Server` **não** estão presentes ou estão sanitizados  
- [ ] Sim — o nome do framework está presente, mas a versão **está oculta**  
- [ ] Sim — tanto o nome quanto a versão do framework **estão expostos**  

### Os cookies ou as estruturas de diretórios específicos do framework são identificáveis?
- [ ] Não — artefatos comuns de framework (ex: caminhos `.js`, nomes de cookies) estão ofuscados ou renomeados  
- [ ] Sim — cookies específicos do framework (ex: `JSESSIONID`, `PHPSESSID`, `csrftoken`) **são** identificáveis  
- [ ] Sim — estruturas de diretórios (ex: `/wp-content/`, `_next/static/`) **estão** visíveis e confirmam o framework  

### As mensagens de erro ou comentários no código-fonte divulgam detalhes do framework?
- [ ] Não — o tratamento de erros é genérico e os comentários foram removidos  
- [ ] Sim — stack traces específicos do framework **estão habilitados** e visíveis nas respostas  
- [ ] Sim — o código-fonte HTML contém comentários ou tags de metadados que identificam o framework  

### Existem vulnerabilidades conhecidas associadas à versão do framework identificada?
- [ ] Não — o framework identificado está atualizado e **sem** vulnerabilidades públicas conhecidas  
- [ ] Sim — a versão do framework está desatualizada e CVEs específicos **podem ser** identificados

---

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

## WSTG-CONF-01 — Testar a Configuração da Infraestrutura de Rede

O teste de configuração da infraestrutura de rede envolve a identificação de vulnerabilidades nos componentes de servidor e rede subjacentes que suportam a aplicação web. Atacantes visam serviços mal configurados, protocolos obsoletos e portas abertas desnecessárias para obter um ponto de apoio, realizar movimentação lateral ou efetuar reconhecimento. Descobertas comuns incluem versões inseguras de TLS, banners de serviço detalhados e interfaces administrativas expostas que deveriam estar restritas a redes internas. Ao explorar essas falhas de nível de infraestrutura, um adversário pode comprometer a confidencialidade dos dados em trânsito ou obter acesso não autorizado ao sistema operacional hospedeiro.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Ferramentas:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Existem portas abertas ou serviços desnecessários expostos no host da aplicação?
- [ ] Não — apenas as portas necessárias (ex: 443) estão **acessíveis**  
- [ ] Sim — serviços adicionais estão expostos, mas seguem uma configuração **segura**  
- [ ] Sim — serviços não essenciais (ex: FTP, Telnet, SMB) estão **expostos** publicamente  

### Os banners ou cabeçalhos de serviço vazam informações confidenciais de versão?
- [ ] Não — os banners foram removidos ou são genéricos  
- [ ] Sim — os banners revelam o software do servidor (ex: Apache/nginx), mas **não** os números de versão  
- [ ] Sim — banners detalhados revelam versões específicas de software, auxiliando a **exploração**  

### A configuração de TLS segue as melhores práticas de segurança modernas?
- [ ] Sim — apenas TLS 1.2/1.3 estão **habilitados** com cipher suites fortes  
- [ ] Sim — protocolos legados (TLS 1.0/1.1) estão **habilitados**  
- [ ] Não — cifras fracas ou protocolos inseguros (SSLv2/v3) estão **habilitados**  

### As interfaces administrativas ou de gerenciamento estão restritas a redes autorizadas?
- [ ] Sim — as interfaces (ex: SSH, RDP, painéis de controle) **não estão acessíveis** a partir da internet pública  
- [ ] No — as interfaces estão publicamente **acessíveis**, mas exigem autenticação de múltiplos fatores  
- [ ] No — as interfaces estão publicamente **acessíveis** com autenticação de fator único ou credenciais padrão

---

## WSTG-CONF-02 — Testar a Configuração da Plataforma da Aplicação

O teste de configuração da plataforma da aplicação envolve a auditoria do servidor web subjacente, do servidor de aplicação e das definições do framework para garantir que estejam endurecidos (hardened) contra exploits comuns. Atacantes buscam ativamente por credenciais padrão, vulnerabilidades de plataforma não corrigidas (unpatched) e interfaces administrativas expostas para obter acesso não autorizado ou executar código. Configurações incorretas (misconfigurations) frequentemente resultam na exposição de variáveis de ambiente sensíveis, caminhos internos do sistema ou na presença de aplicações de exemplo desnecessárias que expandem a superfície de ataque. Do ponto de vista profissional, uma plataforma mal configurada serve como um ponto de entrada de alta probabilidade para alcançar o acesso inicial ao ambiente alvo.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se interfaces administrativas estiverem acessíveis com credenciais padrão ou se scripts de exemplo permitirem Execução Remota de Código (Remote Code Execution).

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Credenciais ou contas padrão estão ativas na plataforma?
- [ ] Não — todas as contas padrão estão **desativadas** ou as senhas foram alteradas  
- [ ] Sim — existem contas padrão, mas **não estão acessíveis** a partir da rede externa  
- [ ] Sim — as contas padrão estão **ativas** e acessíveis via internet *(Crítico)*  

### A plataforma está expondo informações sensíveis de versão através de cabeçalhos ou páginas de erro?
- [ ] Não — as strings de versão estão **ocultas** ou são genéricas  
- [ ] Sim — informações detalhadas de versão **são expostas** em cabeçalhos HTTP (ex: `Server`, `X-Powered-By`)  
- [ ] Sim — stack traces completos e caminhos internos do sistema **são expostos** em mensagens de erro detalhadas (verbose)  

### As interfaces administrativas ou de gerenciamento estão devidamente restritas?
- [ ] Não — interfaces administrativas **não estão expostas**  
- [ ] Sim — as interfaces existem, mas estão **restritas** por IP allowlisting ou Autenticação de Múltiplos Fatores (Multi-Factor Authentication)  
- [ ] Sim — interfaces (ex: `/admin`, `/manager`, `/console`) estão **publicamente acessíveis** com autenticação de fator único  

### Existem arquivos de exemplo, scripts ou documentação presentes no servidor de produção?
- [ ] Não — todos os arquivos não essenciais foram **removidos**  
- [ ] Sim — arquivos de exemplo ou documentação **estão presentes**, mas não vazam informações sensíveis  
- [ ] Sim — scripts de exemplo (ex: `info.php`, `examples/`) **estão presentes** e fornecem um vetor de ataque direto  

### O servidor está configurado para suportar métodos HTTP inseguros?
- [ ] Não — apenas `GET` e `POST` estão **habilitados**  
- [ ] Sim — métodos potencialmente arriscados como `PUT`, `DELETE` ou `TRACE` estão **habilitados**, mas restritos  
- [ ] Sim — métodos arriscados estão **habilitados** e permitem a modificação não autorizada de arquivos ou roubo de credenciais

---

## WSTG-CONF-03 — Teste de Manipulação de Extensões de Arquivo para Informações Sensíveis

O teste de manipulação de extensões de arquivo envolve identificar se o servidor web ou o servidor de aplicação revela informações sensíveis ao fornecer arquivos que deveriam ser restritos ou executados. Atacantes frequentemente procuram por arquivos de backup, arquivos de configuração e fragmentos de código-fonte (ex: `.bak`, `.old`, `.env`, `.inc`) que podem ter sido deixados na raiz web (web root) e fornecidos como texto simples devido à falta de mapeamentos de manipuladores (handlers) específicos. Esta vulnerabilidade é relevante porque pode levar à exposição de credenciais de banco de dados, chaves de API e lógica de negócio interna, facilitando a exploração mais profunda da infraestrutura. Essas exposições ocorrem tipicamente em diretórios públicos, pastas de upload ou via configurações incorretas de MIME-type no servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Médio / Alto* |

> *A gravidade torna-se Alta se arquivos de configuração contendo credenciais ou o código-fonte completo estiverem acessíveis.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Extensões de arquivos de backup ou temporários sensíveis (ex: `.bak`, `.old`, `.swp`, `~`) estão acessíveis?
- [ ] Não — o servidor retorna 403 Forbidden ou 404 Not Found para extensões de backup comuns  
- [ ] Sim — o servidor permite o download de arquivos de backup, mas eles **não** contêm dados sensíveis  
- [ ] Sim — o servidor permite o download de arquivos de backup contendo dados sensíveis ou código-fonte *(Alta)*  

### O servidor expõe arquivos de configuração de ambiente ou do sistema (ex: `.env`, `.config`, `.yml`, `.ini`)?
- [ ] Não — arquivos de configuração restritos **não** podem ser acessados e retornam 403/404  
- [ ] Sim — arquivos de configuração sensíveis **estão** acessíveis e divulgam segredos internos ou credenciais  

### O código-fonte pode ser recuperado anexando extensões alternativas (ex: `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] Não — o código da aplicação **não** é renderizado como texto simples, independentemente da manipulação da extensão  
- [ ] Sim — o código-fonte é revelado porque o servidor **não** possui um manipulador (handler) para a extensão anexada  

### Diretórios administrativos ou de metadados (ex: `.git/`, `.svn/`, `.DS_Store`) estão bloqueados para acesso público?
- [ ] Não — esses diretórios/arquivos **não** existem na raiz web  
- [ ] Sim — o acesso **não é possível** devido a regras de reescrita no lado do servidor ou permissões  
- [ ] Sim — diretórios de metadados **estão** acessíveis e permitem a reconstrução total do código-fonte  

### Como o servidor trata requisições de arquivos com múltiplas extensões (ex: `file.php.jpg`)?
- [ ] Não — o servidor prioriza corretamente a segurança da extensão interna ou bloqueia a requisição  
- [ ] Sim — o servidor processa o arquivo como a primeira extensão, mas o bypass **não é possível** para execução  
- [ ] Sim — o servidor ignora a extensão final, permitindo que a execução de código ou a divulgação da fonte **seja possível**

---

## WSTG-CONF-04 — Revisão de Backups Antigos e Arquivos Não Referenciados em busca de Informações Sensíveis

A revisão de backups antigos e arquivos não referenciados envolve a identificação de arquivos esquecidos, temporários ou ocultos em um servidor web que não se destinam ao acesso público. Esses arquivos geralmente incluem backups de código-fonte (`.zip`, `.bak`), arquivos de swap de editores de texto (`.swp`, `~`) ou metadados de controle de versão (`.git`, `.svn`) que podem vazar informações sensíveis. Atacantes utilizam wordlists automatizadas e ferramentas de descoberta para realizar Brute Force em convenções de nomenclatura comuns, visando a exfiltração de credenciais de banco de dados, chaves de API hardcoded ou lógica que facilite explorações posteriores. Essa vulnerabilidade geralmente decorre de manutenção manual do servidor ou pipelines de CI/CD falhas que não realizam a sanitização da raiz web (web root) de produção.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se arquivos de configuração, arquivos de código-fonte ou credenciais forem descobertos.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### A listagem de diretórios (directory listing) está habilitada no servidor web?
- [ ] Não — a listagem de diretórios está **desabilitada** e retorna um erro 403 Forbidden ou um erro personalizado  
- [ ] Sim — a listagem de diretórios está **habilitada** em diretórios não sensíveis  
- [ ] Sim — a listagem de diretórios está **habilitada** em diretórios que contêm código-fonte ou arquivos sensíveis *(Crítico)*  

### Arquivos de backup com extensões comuns (ex: .bak, .old, .save) são detectáveis?
- [ ] Não — extensões de backup comuns **não foram encontradas** ou estão bloqueadas pela configuração do servidor  
- [ ] Sim — arquivos de backup existem, mas **não** contêm informações sensíveis  
- [ ] Sim — arquivos de backup de configuração ou código-fonte **estão** acessíveis *(Alta)*  

### Diretórios de controle de versão (ex: .git, .svn) ou metadados estão expostos?
- [ ] Não — metadados de controle de versão **não estão presentes** ou estão bloqueados corretamente  
- [ ] Sim — metadados existem, mas o repositório completo **não pode** ser reconstruído  
- [ ] Sim — todo o repositório de código-fonte **pode** ser exfiltrado através de metadados expostos  

### Arquivos compactados (ex: .zip, .tar.gz, .rar) da aplicação estão presentes na raiz web (web root)?
- [ ] Não — nenhum arquivo compactado sensível foi descoberto via Brute Force ou enumeração  
- [ ] Sim — arquivos compactados foram encontrados, mas estão protegidos por senha ou contêm ativos públicos  
- [ ] Sim — arquivos compactados não criptografados contendo o código ou dados da aplicação **estão** publicamente acessíveis  

### A aplicação vaza arquivos temporários criados por editores de texto ou IDEs?
- [ ] Não — arquivos temporários como `.swp`, `~` ou `.DS_Store` **não estão presentes**  
- [ ] Sim — arquivos temporários não sensíveis estão **presentes**  
- [ ] Sim — arquivos temporários revelam trechos de código-fonte ou caminhos internos

---

## WSTG-CONF-05 — Enumerar Interfaces de Administração de Infraestrutura e Aplicação

As interfaces administrativas representam o plano de controle de gerenciamento de uma aplicação e de sua infraestrutura subjacente, frequentemente concedendo acesso privilegiado a configurações do sistema, dados de usuários e operações server-side. Atacantes priorizam a enumeração dessas interfaces por meio de Brute Force de diretórios, análise de `robots.txt` ou observação de padrões de URL previsíveis para encontrar pontos de entrada que possam carecer de autenticação robusta ou depender de credenciais padrão. A descoberta de um painel de administração exposto aumenta significativamente o risco de comprometimento total do sistema, modificação não autorizada de dados ou interrupção do serviço. Essas interfaces são frequentemente encontradas em portas não padrão ou subdomínios, tornando-as alvos principais para scanners automatizados e reconhecimento manual.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a interface permitir alterações de configuração em todo o sistema, gerenciamento de usuários ou exfiltração de dados sem autenticação de múltiplos fatores (MFA).

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### As interfaces administrativas são descobertas via Fuzzing de diretórios ou reconhecimento?
- [ ] Não — nenhuma interface administrativa está exposta ou é detectável  
- [ ] Sim — as interfaces são detectáveis, mas o acesso **é restrito** a faixas de IP internos  
- [ ] Sim — as interfaces **são** detectáveis e acessíveis publicamente  

### A autenticação é obrigatória nos portais administrativos descobertos?
- [ ] Sim — autenticação de múltiplos fatores (MFA) **é obrigatória**  
- [ ] Sim — autenticação de fator único **é obrigatória**  
- [ ] Não — nenhuma autenticação é necessária para acessar a interface *(Crítico)*  

### Os caminhos administrativos estão ocultos ou são não padrão para evitar a descoberta?
- [ ] Sim — os caminhos utilizam nomenclatura não padrão, aleatória ou não previsível  
- [ ] Não — os caminhos utilizam convenções de nomenclatura comuns (ex: `/admin`, `/manager`, `/console`) e **podem** ser facilmente adivinhados  

### A interface fornece acesso a funcionalidades sensíveis do sistema ou da aplicação?
- [ ] Não — a interface é um dashboard de status apenas de leitura ("read-only") com impacto mínimo  
- [ ] Sim — a interface **pode** modificar a configuração da aplicação ou dados de usuários  
- [ ] Sim — a interface **pode** executar comandos em nível de sistema ou realizar gerenciamento de banco de dados

---

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

## WSTG-CONF-07 — Testar HTTP Strict Transport Security

O HTTP Strict Transport Security (HSTS) é um mecanismo de segurança que permite que um servidor web informe aos navegadores que ele só deve ser acessado via HTTPS, prevenindo ataques de downgrade de protocolo e sequestro de cookies (cookie hijacking). Ao forçar uma conexão criptografada, o HSTS mitiga ataques de Man-in-the-Middle (MitM), como o SSLStrip, onde um invasor intercepta uma requisição HTTP inicial insegura para impedir a transição para TLS. Do ponto de vista de um atacante, a ausência deste cabeçalho ou uma duração curta de `max-age` fornece uma janela de oportunidade para interceptar tokens de sessão sensíveis ou credenciais durante o handshake inicial em texto claro. Este teste foca em verificar a presença, duração e escopo do cabeçalho `Strict-Transport-Security` em todos os endpoints sensíveis da aplicação.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média* |

> *A severidade torna-se Média se a aplicação manipular PII, tokens de sessão ou credenciais, pois a falta de HSTS aumenta a taxa de sucesso de ataques MitM.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### O cabeçalho `Strict-Transport-Security` está presente nas respostas HTTPS?
- [ ] Sim — o cabeçalho **está presente** em todos os endpoints sensíveis  
- [ ] Sim — o cabeçalho **está presente**, mas apenas na página inicial (landing page)  
- [ ] Não — o cabeçalho **está ausente** em toda a aplicação  

### A diretiva `max-age` está configurada com uma duração suficiente?
- [ ] Sim — `max-age` está configurado para um ano ou mais (ex: `31536000`)  
- [ ] Sim — `max-age` está configurado, mas é **muito curto** (ex: menos de 6 meses)  
- [ ] Não — a diretiva `max-age` **está ausente** ou configurada como `0` *(Crítico)*  

### A política HSTS cobre todos os subdomínios?
- [ ] Sim — a diretiva `includeSubDomains` **está aplicada**  
- [ ] Não — a diretiva `includeSubDomains` **está ausente**, deixando os subdomínios vulneráveis a downgrade  
- [ ] Não — a funcionalidade não se aplica, pois não existem subdomínios  

### A aplicação está registrada para HSTS Preloading?
- [ ] Sim — a diretiva `preload` **está presente** e o domínio está na lista de preload de HSTS  
- [ ] Sim — a diretiva `preload` **está presente**, mas o domínio **ainda não** está na lista de preload  
- [ ] Não — a diretiva `preload` **está ausente**, permitindo uma janela para MitM na primeira visita  

### O cabeçalho HSTS é enviado incorretamente via HTTP em texto claro?
- [ ] Não — o cabeçalho é enviado **apenas** através de conexões HTTPS seguras  
- [ ] Sim — o cabeçalho **é enviado** via HTTP, o que é ignorado pelos navegadores e pode vazar detalhes de configuração

---

## WSTG-CONF-08 — Testar Políticas de Domínio Cruzado (Cross-Domain) de RIA

As políticas de domínio cruzado (Cross-Domain) de Rich Internet Applications (RIA) envolvem arquivos como `crossdomain.xml` e `clientaccesspolicy.xml`, que definem como clientes web — especificamente Adobe Flash e Microsoft Silverlight — tratam requisições de origem cruzada (Cross-Origin). Esses arquivos são críticos para a segurança pois substituem explicitamente a Política de Mesma Origem (SOP), determinando quais domínios externos têm permissão para interagir com a aplicação e ler suas respostas. Configurações excessivamente permissivas, como o uso de curingas (Wildcards) no atributo `domain`, permitem que atacantes hospedem objetos RIA maliciosos em sites de terceiros que podem exfiltrar dados sensíveis ou realizar ações em nome de usuários autenticados. Pentesters normalmente localizam esses arquivos na raiz do servidor web (Web Root) para avaliar o nível de confiança concedido a origens de terceiros e identificar potenciais vazamentos de dados entre origens.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Média / Alta* |

> *A gravidade torna-se Alta se a aplicação processar dados sensíveis de usuários ou identificadores de sessão que possam ser exfiltrados por meio de uma política permissiva.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Os arquivos de política de domínio cruzado (`crossdomain.xml` ou `clientaccesspolicy.xml`) estão presentes na raiz do servidor web?
- [ ] Não — os arquivos **não puderam** ser encontrados no diretório raiz  
- [ ] Sim — o arquivo `crossdomain.xml` (Flash) **está** presente  
- [ ] Sim — o arquivo `clientaccesspolicy.xml` (Silverlight) **está** presente  
- [ ] Sim — ambos os arquivos de política RIA **estão** presentes  

### O atributo de domínio `allow-access-from` está restrito a origens confiáveis?
- [ ] Não — os arquivos existem, mas não contêm entradas `allow-access-from`  
- [ ] Sim — domínios específicos e confiáveis estão explicitamente na lista de permissões (Whitelisted) e o bypass **não é possível**  
- [ ] Sim — os domínios estão na lista de permissões, mas incluem terceiros não confiáveis ou subdomínios  
- [ ] Sim — um curinga `*` **é utilizado**, permitindo o acesso de **qualquer** origem *(Crítico)*  

### A política permite comunicação insegura via HTTP para recursos HTTPS?
- [ ] Não — `secure="true"` está definido (ou é o padrão), impedindo que origens HTTP acessem dados HTTPS  
- [ ] Sim — `secure="false"` está explicitamente definido, permitindo que atacantes MITM exfiltrem dados seguros  

### Os cabeçalhos HTTP estão excessivamente permissivos na configuração de domínio cruzado?
- [ ] Não — `allow-http-request-headers-from` está restrito ou **não habilitado**  
- [ ] Sim — cabeçalhos específicos são permitidos para domínios confiáveis  
- [ ] Sim — o curinga `*` é utilizado para cabeçalhos, permitindo que atacantes enviem cabeçalhos personalizados de qualquer domínio  

### A configuração de `site-control` (master-policy) é restritiva?
- [ ] Não — `permitted-cross-domain-policies` está definido como `none` ou `master-only`  
- [ ] Sim — a política permite que outros arquivos no servidor definam suas próprias regras (`all`)

---

## WSTG-CONF-09 — Teste de Permissões de Arquivo

O teste de permissões de arquivo envolve a auditoria dos níveis de controle de acesso atribuídos a arquivos e diretórios no servidor web para garantir que recursos sensíveis não estejam excessivamente expostos a usuários ou processos não autorizados. Permissões configuradas incorretamente podem permitir que atacantes leiam arquivos de configuração sensíveis contendo credenciais de banco de dados, visualizem código-fonte proprietário ou modifiquem scripts do lado do servidor para obter execução remota de código. Esta vulnerabilidade ocorre tipicamente durante a fase de implantação (deployment), quando sinalizações de "leitura global" (world-readable) ou "escrita global" (world-writable) são deixadas em diretórios críticos da aplicação, como caminhos de configuração, pastas de log ou repositórios de upload. Do ponto de vista de um atacante, a descoberta de um arquivo incorretamente protegido frequentemente serve como o catalisador primário para o escalonamento de privilégios ou o comprometimento total do sistema.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se arquivos de configuração sensíveis (ex: `.env`, `web.config`) estiverem legíveis ou se o acesso de escrita na raiz da web (web root) for possível.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Ferramentas:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Os arquivos de configuração sensíveis da aplicação estão protegidos contra acesso não autorizado?
- [ ] Sim — arquivos de configuração (ex: `.env`, `config.php`) **não são acessíveis** via requisições web  
- [ ] Sim — os arquivos estão presentes, mas o acesso é **restrito** apenas a usuários locais autorizados  
- [ ] Não — arquivos de configuração sensíveis **estão acessíveis** e expõem credenciais ou segredos  

### Um atacante pode modificar arquivos ou diretórios dentro da raiz da web (web root)?
- [ ] Não — todos os diretórios da raiz da web são de **apenas leitura** para o usuário do servidor web  
- [ ] Sim — existem diretórios com permissão de escrita, mas a execução de scripts está **desativada**  
- [ ] Sim — existem diretórios com permissão de escrita global (**world-writable**) que **permitem** o upload e a execução de scripts *(Crítico)*  

### Metadados de controle de versão ou arquivos de backup estão expostos devido a permissões frouxas?
- [ ] Não — arquivos `.git`, `.svn` e arquivos de backup (ex: `.bak`, `~`) **não estão presentes** ou estão protegidos  
- [ ] Sim — diretórios de metadados existem, mas a listagem de diretórios está **desativada**  
- [ ] Sim — metadados sensíveis ou backups estão **totalmente acessíveis**, permitindo a recuperação do código-fonte  

### O usuário do servidor web opera com o princípio do menor privilégio?
- [ ] Sim — o processo do servidor web é executado como um usuário **dedicado de baixo privilégio**  
- [ ] Não — o processo do servidor web é executado com **privilégios desnecessários** (ex: `root` ou `Administrator`)  

### Os arquivos de log estão protegidos contra leitura ou modificação não autorizada?
- [ ] Sim — os arquivos de log são **restritos** a usuários administrativos  
- [ ] Sim — os arquivos de log são legíveis, mas **não** contêm tokens de sessão ou PII (Informações de Identificação Pessoal)  
- [ ] Não — os arquivos de log são **publicamente legíveis** e expõem dados sensíveis de transações ou informações de usuários

---

## WSTG-CONF-10 — Subdomain Takeover

O Subdomain Takeover ocorre quando uma entrada DNS (tipicamente um CNAME, mas ocasionalmente registros A ou MX) aponta para um recurso desativado ou inexistente em um provedor de nuvem de terceiros ou serviço de hospedagem. Os atacantes identificam esses registros DNS "dangling" (pendentes) procurando por mensagens de erro específicas de provedores como AWS, GitHub ou Azure que indicam que um recurso não é mais reivindicado. Ao registrar o nome do recurso abandonado na plataforma do provedor, o atacante obtém controle total sobre o subdomínio, permitindo a hospedagem de conteúdo malicioso, a exfiltração de cookies de sessão com escopo no domínio base e o bypass de proteções de Content Security Policy (CSP) ou CORS. Esta vulnerabilidade é particularmente perigosa porque utiliza a confiança associada à estrutura de domínio legítima da organização para facilitar phishing de alto impacto e roubo de dados.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica* |

> *A severidade torna-se Crítica se o subdomínio puder ser usado para sequestrar cookies de sessão do domínio base ou realizar o bypass de redirecionamentos de SSO/OAuth.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Ferramentas:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### A aplicação utiliza hospedagem de terceiros ou serviços de nuvem via subdomínios?
- [ ] Não — todos os subdomínios apontam para um espaço de IP controlado pela organização  
- [ ] Sim — os subdomínios apontam para provedores de terceiros (S3, GitHub Pages, Heroku, etc.)  

### Existem registros DNS "dangling" apontando para recursos inexistentes?
- [ ] Não — todos os registros DNS identificados resolvem para recursos ativos e controlados  
- [ ] Sim — existem registros CNAME ou A para recursos que **não existem mais** no provedor  
- [ ] Sim — os registros DNS resolvem para NXDOMAIN ou páginas de erro específicas do provedor *(ex: "NoSuchBucket")*  

### O recurso "dangling" identificado pode ser reivindicado com sucesso?
- [ ] Não — o provedor possui proteções contra a reivindicação de nomes abandonados ou é necessária a verificação da conta  
- [ ] Sim — o nome do recurso **pode** ser registrado na plataforma de terceiros por qualquer usuário  

### O domínio base utiliza cookies com escopo de "Domínio" que poderiam ser comprometidos?
- [ ] Não — os cookies têm escopo restrito a subdomínios específicos ou utilizam flags `Host-Only`  
- [ ] Sim — cookies sensíveis (IDs de sessão, JWTs) têm escopo no domínio pai e **podem** ser exfiltrados via um subdomínio sequestrado  

### O subdomínio pode ser utilizado para burlar cabeçalhos de segurança ou fluxos de autenticação?
- [ ] Não — não há dependências de segurança nos subdomínios identificados  
- [ ] Sim — o subdomínio está na whitelist das diretivas `script-src` ou `connect-src` do CSP  
- [ ] Sim — o subdomínio é uma URI de redirecionamento válida para configurações de OAuth ou SSO

---

## WSTG-CONF-11 — Test Cloud Storage

O teste de configurações incorretas de armazenamento em nuvem envolve a identificação e auditoria de serviços de armazenamento de objetos acessíveis publicamente, como Amazon S3, Azure Blobs ou Google Cloud Storage. Esses recursos frequentemente contêm dados sensíveis, incluindo backups, código-fonte, PII ou arquivos de configuração que estão expostos devido a Access Control Lists (ACLs) ou políticas de Identity and Access Management (IAM) excessivamente permissivas. Atacantes enumeram nomes de buckets por meio do reconhecimento de arquivos JavaScript, código-fonte HTML e técnicas de Brute Force para encontrar pontos de entrada para exfiltração de dados ou modificação não autorizada de arquivos. A exploração pode levar a um impacto significativo, incluindo violações de dados completas ou a capacidade de servir arquivos maliciosos a partir de um domínio confiável.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica* |

> *A severidade torna-se Crítica se PII, credenciais ou backups de produção estiverem acessíveis, ou se o acesso de gravação (write access) não autenticado estiver habilitado.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Os endpoints de armazenamento em nuvem (buckets/containers) foram identificados e enumerados?
- [ ] Não — nenhum endpoint de armazenamento em nuvem é utilizado ou detectável  
- [ ] Sim — os endpoints de armazenamento foram identificados via análise de código ou monitoramento de tráfego  
- [ ] Sim — os endpoints de armazenamento foram descobertos via Brute Force de convenção de nomes  

### Usuários não autenticados conseguem listar o conteúdo do armazenamento em nuvem?
- [ ] Não — a listagem do bucket está **desativada** e retorna 403 Forbidden ou 404 Not Found  
- [ ] Sim — a listagem está **ativada**, mas nenhum arquivo sensível está presente  
- [ ] Sim — a listagem está **ativada** e nomes de arquivos sensíveis/metadados estão visíveis  

### O download de arquivos sensíveis dos buckets identificados é possível?
- [ ] Não — os arquivos estão protegidos por políticas de IAM ou autenticação válida é exigida  
- [ ] Sim — os arquivos estão acessíveis, mas exigem uma URL assinada (signed URL) que **não pode** ser adivinhada facilmente  
- [ ] Sim — arquivos sensíveis (backups, `.env`, PII) estão **publicamente acessíveis** para download *(Crítica)*  

### O upload ou modificação de arquivos de forma não autenticada é permitido?
- [ ] Não — uploads ou modificações não autenticados **não são possíveis**  
- [ ] Sim — o upload autenticado é exigido, mas o bypass **é possível** via condições de política fracas  
- [ ] Sim — a escrita, sobrescrita ou exclusão de arquivos de forma não autenticada é **possível** *(Crítica)*  

### As políticas de Cross-Origin Resource Sharing (CORS) no endpoint de armazenamento são restritivas?
- [ ] Sim — o CORS está **desativado** ou restrito a origens confiáveis específicas  
- [ ] Não — o CORS está **ativado** com uma origem wildcard `*`, permitindo acesso não autorizado baseado em web

---

## WSTG-CONF-12 — Content Security Policy (CSP)

A Content Security Policy (CSP) é um mecanismo crítico de defesa em profundidade implementado através de cabeçalhos de resposta HTTP para mitigar riscos como Cross-Site Scripting (XSS), clickjacking e ataques de injeção de dados. Ao definir quais recursos dinâmicos têm permissão para carregar, uma CSP bem configurada restringe a capacidade de um atacante de executar scripts não autorizados ou exfiltrar dados sensíveis para domínios externos. Pentesters avaliam a política em busca de diretivas excessivamente permissivas, o uso de palavras-chave inseguras como `'unsafe-inline'` e a dependência de CDNs inseguras que possam facilitar bypasses. Uma CSP fraca ou ausente não cria uma vulnerabilidade diretamente, mas aumenta significativamente o impacto de ataques de injeção bem-sucedidos ao falhar em fornecer uma camada secundária de proteção.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Ferramentas:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### O cabeçalho Content Security Policy (CSP) está presente nas respostas da aplicação?
- [ ] Sim — o cabeçalho `Content-Security-Policy` está **presente** e é **aplicado**  
- [ ] Sim — `Content-Security-Policy-Report-Only` está **presente** para testes  
- [ ] Não — nenhum cabeçalho CSP está **presente** nas respostas  

### As diretivas `script-src` ou `default-src` estão devidamente restritas?
- [ ] Sim — as diretivas utilizam whitelisting rigoroso, nonces ou hashes e o bypass **não é possível**  
- [ ] Sim — as diretivas estão **configuradas corretamente**, mas a whitelist inclui CDNs conhecidas passíveis de bypass  
- [ ] Não — as diretivas utilizam wildcards (`*`) ou esquemas `data:`, tornando o bypass **possível**  

### A política permite a execução de scripts inline inseguros ou eval?
- [ ] Não — `'unsafe-inline'` e `'unsafe-eval'` **não estão presentes**  
- [ ] Sim — `'unsafe-inline'` está **presente**, mas protegido por um `nonce` ou `hash`  
- [ ] Sim — `'unsafe-inline'` ou `'unsafe-eval'` estão **habilitados** sem proteções adicionais *(Crítico)*  

### A aplicação está protegida contra clickjacking através da diretiva `frame-ancestors`?
- [ ] Sim — `frame-ancestors` está definido como `'none'` ou `'self'`  
- [ ] Sim — `frame-ancestors` está **habilitado**, mas permite domínios de terceiros confiáveis específicos  
- [ ] Não — `frame-ancestors` está **ausente**, dependendo do legado `X-Frame-Options` ou de nenhuma proteção  

### Existe um mecanismo para reportar violações de CSP?
- [ ] Sim — `report-uri` ou `report-to` está **configurado** e ativo  
- [ ] Não — o relatório de violações está **desabilitado** ou **não configurado**

---

## WSTG-CONF-13 — Path Confusion

O Path Confusion surge de discrepâncias na forma como diferentes componentes web, tais como proxies reversos, balanceadores de carga e servidores de aplicação backend, analisam e interpretam caminhos de URL. Atacantes exploram estas inconsistências injetando caracteres específicos como pontos e vírgulas, barras codificadas ou segmentos de ponto (dot-segments) para enganar filtros de segurança e aceder a endpoints restritos ou recursos estáticos. Esta vulnerabilidade pode levar a falhas críticas de segurança, incluindo bypass de autenticação, acesso não autorizado a dados e envenenamento de cache web (Web Cache Poisoning), uma vez que o front-end pode aplicar regras de segurança a um caminho que o back-end interpreta de forma diferente. A exploração bem-sucedida ocorre tipicamente na fronteira arquitetural onde o roteamento de requisições e a lógica de normalização divergem ao longo da stack tecnológica.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Médio / Alto* |

> *A gravidade torna-se Alta se o Path Confusion resultar num bypass de autenticação ou controlo de acesso para endpoints administrativos sensíveis.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### As diferentes camadas arquiteturais interpretam os delimitadores de caminho (ex: `;`, `#`, `?`) de forma consistente?
- [ ] Sim — todas as camadas normalizam e interpretam delimitadores de caminho de forma idêntica  
- [ ] Não — existem inconsistências, mas **não podem** ser utilizadas para aceder a recursos restritos  
- [ ] Não — discrepâncias que permitem a um atacante contornar filtros de front-end e alcançar a lógica de back-end **são possíveis**  

### Os controlos de acesso podem ser contornados utilizando sequências de Path Traversal ou caracteres codificados?
- [ ] Não — a normalização é aplicada consistentemente antes das verificações de segurança  
- [ ] Sim — o bypass é **possível** via sequências de segmentos de ponto (ex: `/admin/..;/`)  
- [ ] Sim — o bypass é **possível** via caracteres codificados em URL (ex: `%2f`, `%2e%2e%2f`)  

### A aplicação é suscetível a Web Cache Poisoning através de Path Confusion?
- [ ] Não — a lógica de cache não é afetada por ambiguidade de caminho ou informações de caminho extra  
- [ ] Sim — conteúdo malicioso **pode** ser armazenado em cache para caminhos legítimos devido a discrepâncias de mapeamento  
- [ ] Sim — informações sensíveis **podem** ser armazenadas em cache em diretórios públicos através de técnicas de `RCD` (Relative Path Overwrite)  

### O servidor trata a "informação de caminho extra" (PathInfo) de forma segura?
- [ ] Não — a funcionalidade está **desativada** ou não existe  
- [ ] Sim — o `PathInfo` é processado e **não** interfere com os filtros de segurança  
- [ ] Não — o `PathInfo` permite que um atacante anexe dados que confundem a lógica de roteamento da aplicação

---

## WSTG-CONF-14 — Testar Outras Má Configurações de Cabeçalhos de Segurança HTTP

O teste para más configurações de cabeçalhos de segurança HTTP envolve verificar a presença e a implementação correta de cabeçalhos defensivos que fornecem proteção essencial contra vetores de ataque comuns baseados na web. Embora esses cabeçalhos não corrijam vulnerabilidades de código subjacentes, sua ausência aumenta significativamente o risco de ataques man-in-the-middle (MITM), exploração de MIME-sniffing e vazamento não intencional de dados entre origens (cross-origin) via referrers. Do ponto de vista de um atacante, a ausência de cabeçalhos de segurança são indicadores de alta visibilidade de um ambiente mal endurecido, facilitando frequentemente cadeias de exploração mais complexas, como session hijacking ou exfiltração de informações. Essas configurações são normalmente avaliadas no nível do servidor e devem ser aplicadas de forma consistente em todos os tipos de resposta, incluindo API endpoints e recursos estáticos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Auditoria de Presença de Cabeçalhos de Segurança
| Cabeçalho | Presente | Ausente | Configuração Recomendada |
|-----------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` or `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Específico por recurso, ex: `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### Como os cabeçalhos de segurança são aplicados em todo o escopo da aplicação?
- [ ] Sim — os cabeçalhos são aplicados globalmente por meio de um balanceador de carga central ou proxy reverso e **não podem** ser substituídos  
- [ ] Sim — os cabeçalhos são aplicados às respostas do documento principal, mas estão **ausentes** em respostas de API ou ativos estáticos  
- [ ] No — os cabeçalhos são aplicados de forma inconsistente ou estão **ausentes** em todo o domínio da aplicação  

### O cabeçalho Strict-Transport-Security (HSTS) está configurado corretamente?
- [ ] Sim — `max-age` está definido para uma longa duração (ex: 2 anos) e `includeSubDomains` está **habilitado**  
- [ ] Sim — `max-age` está definido, mas `includeSubDomains` está **desabilitado**, deixando os subdomínios vulneráveis  
- [ ] No — o cabeçalho HSTS **não está presente** ou o `max-age` está definido como 0, permitindo ataques de downgrade de protocolo  

### A Referrer-Policy evita o vazamento de parâmetros de URL sensíveis?
- [ ] Sim — a política está definida como `no-referrer` ou `same-origin` *(Mais segura)*  
- [ ] Sim — a política está definida como `strict-origin-when-cross-origin`  
- [ ] No — a política **não está definida** ou está definida como `unsafe-url`, potencialmente vazando tokens ou PII sensíveis em cabeçalhos referer  

### O cabeçalho X-Content-Type-Options está prevenindo o MIME-sniffing?
- [ ] Sim — a diretiva `nosniff` está **presente** e corretamente implementada  
- [ ] No — o cabeçalho está **ausente**, potencialmente permitindo que um navegador execute arquivos não executáveis (ex: imagens) como scripts

---

## WSTG-IDNT-01 — Teste de Definições de Funções

O teste de definições de funções envolve a identificação e documentação das várias funções (roles) de usuário e níveis de permissão definidos na aplicação para garantir que estejam logicamente separados e sigam o princípio do privilégio mínimo (**least privilege**). Um atacante analisa a aplicação para mapear funções de alto privilégio em relação a funções de usuário padrão, procurando por qualquer ambiguidade nos limites de permissão ou funções administrativas não documentadas. A identificação dessas funções é o primeiro passo para descobrir caminhos de escalada de privilégios, pois inconsistências nas definições de funções frequentemente levam ao acesso não autorizado a dados sensíveis ou funcionalidades administrativas. Esta fase ocorre normalmente durante o reconhecimento inicial e o mapeamento da lógica de negócio, focando em perfis de usuário, painéis administrativos e estruturas de permissão de API.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### As funções estão claramente definidas e documentadas na aplicação ou em sua documentação?
- [ ] Sim — as funções estão totalmente documentadas e seguem o princípio do **privilégio mínimo**  
- [ ] Sim — as funções estão documentadas, mas contêm **permissões sobrepostas**  
- [ ] Não — não existem documentações ou definições formais de funções  

### Existe uma separação clara entre as funções administrativas e as de usuário padrão?
- [ ] Sim — as funções administrativas estão **completamente isoladas** das visualizações de usuário padrão  
- [ ] Sim — a separação existe, mas os endpoints administrativos são **previsíveis ou adivinháveis**  
- [ ] Não — as funções administrativas e padrão estão **misturadas** nas mesmas interfaces  

### A aplicação utiliza um mecanismo centralizado para Controle de Acesso Baseado em Funções (RBAC)?
- [ ] Sim — RBAC ou ABAC centralizado está **implementado** e é aplicado de forma consistente  
- [ ] Sim — existem controles descentralizados, mas são **aplicados de forma inconsistente** entre os módulos  
- [ ] Não — a lógica de controle de acesso está **hardcoded** em páginas ou componentes individuais  

### Existem funções não documentadas ou ocultas presentes no sistema?
- [ ] Não — todas as funções descobertas correspondem à funcionalidade documentada  
- [ ] Sim — funções ocultas ou "shadow roles" (ex: `super_admin`, `support`, `test`) **estão presentes**  

### Um usuário de baixo privilégio pode enumerar as permissões ou funções de outros usuários?
- [ ] Não — os usuários **não podem** visualizar ou enumerar as funções/permissões de outras entidades  
- [ ] Sim — os usuários **podem** enumerar funções através de páginas de perfil, metadados ou respostas de API  *(Média)*

---

## WSTG-IDNT-02 — Testar o Processo de Registro de Usuário

Testar o processo de registro de usuário envolve avaliar o fluxo de trabalho por meio do qual novas identidades são criadas para garantir que ele não possa ser explorado para acesso não autorizado ou exploração automatizada. As vulnerabilidades nesse processo manifestam-se frequentemente como falta de verificação de identidade (e-mail/SMS), suscetibilidade à criação massiva de contas via bots ou exposição de informações por meio de mensagens de erro detalhadas que revelam nomes de usuários existentes. Atacantes exploram essas falhas para realizar a enumeração de usuários (User Enumeration), conduzir campanhas de spam em larga escala ou manipular parâmetros de registro para obter privilégios elevados durante a fase de criação da conta. Este teste é vital para proteger a integridade da aplicação e evitar que atacantes povoem o sistema com contas fraudulentas ou maliciosas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### A verificação de identidade é exigida antes que uma nova conta se torne ativa?
- [ ] Sim — o registro requer um token único e com limite de tempo enviado por e-mail ou SMS  
- [ ] Sim — a verificação é exigida, mas os tokens são **fracos** ou **previsíveis**  
- [ ] Não — as contas são ativadas **imediatamente** sem qualquer etapa de verificação  

### O processo de registro impede a enumeração de usuários?
- [ ] Sim — a aplicação retorna respostas **idênticas** tanto para e-mails/usuários novos quanto existentes  
- [ ] Não — mensagens de erro (ex: "E-mail já em uso") **permitem** que um atacante mapeie usuários registrados  
- [ ] Não — diferenças de tempo (timing differences) nas respostas do servidor **revelam** se um usuário existe  

### Controles anti-automação estão implementados para evitar o registro em massa?
- [ ] Sim — mecanismos de CAPTCHA ou proof-of-work estão **presentes** e são **eficazes**  
- [ ] Sim — existem controles, mas o bypass **é possível** através de endpoints de API ou manipulação de cabeçalhos  
- [ ] Não — nenhum Rate Limiting ou CAPTCHA é **aplicado** ao endpoint de registro  

### Os parâmetros de registro podem ser manipulados para escalada de privilégios?
- [ ] Não — as funções de usuário (roles) são atribuídas **estritamente** pela lógica do lado do servidor  
- [ ] Sim — parâmetros como `role`, `admin` ou `group_id` **podem** ser modificados na requisição para obter acesso elevado  

### A política de senhas é aplicada durante a fase de registro?
- [ ] Sim — requisitos de senha forte são **aplicados** no lado do servidor  
- [ ] Não — senhas fracas (ex: "password123") são **aceitas** pelo sistema

---

## WSTG-IDNT-03 — Teste do Processo de Provisionamento de Contas

O processo de provisionamento de contas rege como novas identidades são criadas, verificadas e como lhes são atribuídos privilégios dentro de um ambiente de aplicação. Atacantes visam este fluxo de trabalho para realizar registros em massa automatizados para fins de spam ou Denial-of-Service (DoS), contornar etapas de verificação de identidade para criar contas fraudulentas ou manipular parâmetros para conceder a si mesmos permissões elevadas durante a fase de cadastro. Vulnerabilidades manifestam-se frequentemente em formulários de registro por autoatendimento, sistemas apenas para convidados ou consoles de provisionamento administrativo onde falhas de lógica de negócio permitem a criação não autorizada de contas ou a Escalada de Privilégios (Privilege Escalation). Do ponto de vista de um atacante, comprometer o fluxo de provisionamento fornece uma base legítima que pode ser usada como ponto de partida para explorações mais profundas, exfiltração de dados ou ataques de engenharia social.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Crítica se um atacante puder provisionar contas administrativas ou contornar restrições globais de registro.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### O auto-registro é estritamente controlado ou limitado a usuários autorizados?
- [ ] Sim — o registro está **desabilitado** ou requer aprovação administrativa manual  
- [ ] Sim — o registro está aberto, mas restrito a domínios de e-mail **autorizados** ou tokens de convite  
- [ ] Não — o registro está **aberto** a qualquer usuário sem restrições de domínio ou token  

### É necessário um mecanismo robusto de verificação de identidade (ex: e-mail/SMS) antes da ativação da conta?
- [ ] Sim — a verificação é **obrigatória** e **não pode** ser contornada  
- [ ] Sim — a verificação é **obrigatória**, mas **pode** ser contornada via manipulação de parâmetros ou tokens previsíveis  
- [ ] Não — as contas ficam **ativas imediatamente** após o envio, sem qualquer verificação  

### O usuário pode manipular parâmetros de função (role) ou permissão durante a requisição de registro?
- [ ] Não — as funções são **atribuídas no lado do servidor** e não existem parâmetros no lado do cliente  
- [ ] Sim — existem parâmetros de função, mas a manipulação **não é possível** devido à validação no lado do servidor  
- [ ] Sim — parâmetros de função/permissão (ex: `role=admin`, `is_staff=true`) **podem** ser modificados para obter acesso elevado *(Crítico)*  

### Estão implementados controles de Rate Limiting ou anti-automação para evitar a criação de contas em massa?
- [ ] Sim — Rate Limiting e CAPTCHAs estão **ativos** e são eficazes  
- [ ] Sim — existem controles, mas o **bypass é possível** via spoofing de cabeçalho ou serviços de resolução de CAPTCHA  
- [ ] Não — nenhum Rate Limiting ou controle anti-automação é **aplicado**  

### O processo de provisionamento vaza informações sobre contas existentes (User Enumeration)?
- [ ] Não — as mensagens de resposta são **idênticas** para usuários existentes e não existentes  
- [ ] Sim — respostas diferentes ou variações de tempo **permitem** a enumeração de usuários existentes

---

## WSTG-IDNT-04 — Teste de Enumeração de Contas e Contas de Usuário Adivinháveis

A enumeração de contas ocorre quando uma aplicação revela a existência ou não de uma conta de usuário por meio de variações nas respostas HTTP, mensagens de erro ou diferenças de tempo mensuráveis. Atacantes exploram esse comportamento enviando sistematicamente listas de nomes de usuário para identificar contas válidas, que são posteriormente visadas para credential stuffing, ataques de Brute Force ou engenharia social. Esta vulnerabilidade manifesta-se comumente em portais de login, funcionalidades de redefinição de senha, formulários de registro e endpoints de API que manipulam identificadores específicos do usuário. Do ponto de vista de um atacante, a enumeração bem-sucedida reduz significativamente a superfície de ataque, transformando uma tentativa de Brute Force cega em uma exploração direcionada de identidades conhecidas e válidas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### As mensagens de erro de autenticação são consistentes em todos os cenários?
- [ ] Sim — mensagens de erro genéricas são usadas tanto para nomes de usuário inválidos quanto para senhas inválidas  
- [ ] Sim — as mensagens são semelhantes, mas pequenas variações na pontuação ou no uso de maiúsculas/minúsculas **estão presentes**  
- [ ] Não — as mensagens de erro declaram explicitamente "Usuário não encontrado" ou "Senha incorreta" *(Vulnerável)*  

### O fluxo de redefinição de senha ou "esqueci minha senha" vaza a existência da conta?
- [ ] Não — a aplicação retorna uma mensagem genérica independentemente de o e-mail/nome de usuário existir  
- [ ] Sim — existem controles, mas o bypass **é possível** através do comprimento da resposta ou códigos de status  
- [ ] Sim — a aplicação confirma explicitamente se um e-mail de redefinição foi enviado ou se a conta **não existe**  

### O processo de registro está protegido contra enumeração de usuários?
- [ ] Não — o registro não vaza informações ou utiliza CAPTCHA/Rate Limiting para evitar verificações em massa  
- [ ] Sim — existem controles, mas o bypass **é possível** através de diferenças de tempo ou de resposta da API  
- [ ] Sim — a aplicação notifica imediatamente o usuário se o nome de usuário ou e-mail **já está em uso**  

### Diferenças de tempo são mensuráveis ao comparar nomes de usuário válidos vs. inválidos?
- [ ] Não — os tempos de resposta são consistentes independentemente da validade da conta devido a comparações em tempo constante  
- [ ] Sim — existem diferenças de tempo, mas são muito pequenas para serem medidas de forma confiável através da rede  
- [ ] Sim — discrepâncias de tempo mensuráveis **estão presentes** (ex: devido à execução de hashing de senha apenas para usuários válidos)  

### A aplicação utiliza convenções de nomenclatura previsíveis ou adivinháveis para as contas?
- [ ] Não — os identificadores de conta são aleatórios (UUIDs) ou definidos pelo usuário e complexos  
- [ ] Sim — os identificadores de conta seguem um padrão (ex: `emp001`, `emp002`) e a enumeração **é possível**  
- [ ] Sim — os identificadores são números inteiros sequenciais, tornando possível a enumeração completa do banco de dados **possível**

---

## WSTG-IDNT-05 — Teste para Política de Nome de Usuário Fraca ou Não Aplicada

O teste de políticas de nomes de usuário fracas ou não aplicadas foca nas regras que regem a criação de identificadores de conta e sua suscetibilidade a enumeração ou spoofing. Políticas fracas frequentemente permitem sequências previsíveis, palavras comuns de dicionário ou identificadores que espelham IDs de funcionários internos, o que reduz significativamente o espaço de busca para ataques de Brute Force e Credential Stuffing. Do ponto de vista de um atacante, identificar esses padrões é o primeiro passo para a descoberta de contas em larga escala, muitas vezes alcançada analisando mensagens de erro de registro, o comportamento de redefinição de senha ou metadados em perfis públicos. Garantir uma política robusta impede que atacantes mapeiem sistematicamente a base de usuários e facilita a defesa contra phishing direcionado e tentativas automatizadas de Authentication Bypass.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Os nomes de usuário são baseados em padrões altamente previsíveis ou sequenciais?
- [ ] Não — os nomes de usuário são aleatórios, definidos pelo usuário ou strings de alta entropia  
- [ ] Sim — os nomes de usuário seguem um formato previsível (ex: `user1001`, `user1002`), mas a autenticação é robusta  
- [ ] Sim — os nomes de usuário são **estritamente sequenciais** ou seguem um formato corporativo conhecido, facilitando a enumeração  

### A aplicação impõe requisitos de comprimento mínimo e complexidade para nomes de usuário?
- [ ] Sim — políticas rígidas de comprimento e conjunto de caracteres **são aplicadas** durante o registro  
- [ ] Sim — existem políticas, mas permitem nomes de usuário extremamente curtos (ex: 1-2 caracteres) ou excessivamente simples  
- [ ] Não — **nenhuma política** é aplicada em relação ao comprimento do nome de usuário ou tipos de caracteres  

### Nomes de usuário válidos podem ser enumerados por meio de respostas da aplicação?
- [ ] Não — a aplicação retorna mensagens de erro genéricas e exibe um timing consistente para todas as tentativas  
- [ ] Sim — a aplicação retorna mensagens de erro distintas (ex: "Nome de usuário já em uso"), mas o Rate Limiting **é aplicado**  
- [ ] Sim — a aplicação **vaza** nomes de usuário válidos através de erros de registro, redefinições de senha ou diferenças de timing  

### A política impede o registro de nomes de usuário administrativos comuns ou reservados?
- [ ] Sim — a aplicação bloqueia nomes comuns (ex: `admin`, `root`, `support`) e termos reservados do sistema  
- [ ] Não — os usuários **podem** registrar contas com nomes sensíveis ou administrativos  

### A política de nome de usuário é consistente em todos os pontos de entrada (API, Mobile, Web)?
- [ ] Sim — as políticas **são aplicadas** de forma consistente em todas as interfaces e versões  
- [ ] Não — endpoints legados ou versões específicas de API **não impõem** as mesmas restrições de nome de usuário

---

## WSTG-ATHN-01 — Testing for Credentials Transported over an Encrypted Channel

O teste de credenciais transportadas por um canal criptografado garante que dados de autenticação sensíveis, como nomes de usuário, senhas e tokens de sessão, estejam protegidos contra interceptação em trânsito. Atacantes posicionados na rede — como por meio de ataques Man-in-the-Middle (MitM) em Wi-Fi público ou redes internas comprometidas — podem usar ferramentas de packet sniffing para capturar credenciais em texto claro se o TLS/SSL não for devidamente aplicado. Esta vulnerabilidade ocorre tipicamente em endpoints de login, formulários de redefinição de senha e headers de autenticação de API onde a aplicação falha em exigir HTTPS ou realiza o redirecionamento de HTTP de forma incorreta. A exploração bem-sucedida concede ao atacante acesso total às contas de usuário, levando ao comprometimento total dos dados do usuário e potencial movimentação lateral (lateral movement) dentro da aplicação.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Ferramentas:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### O HTTPS é imposto em todo o processo de autenticação?
- [ ] Sim — O HSTS está **habilitado** e todo o tráfego é estritamente forçado via HTTPS  
- [ ] Sim — O redirecionamento para HTTPS ocorre, mas o HSTS **não está habilitado**  
- [ ] Não — As credenciais **podem** ser enviadas através de HTTP não criptografado  

### As páginas e formulários de login são servidos por um canal criptografado?
- [ ] Sim — A página que contém o formulário de login é entregue via HTTPS  
- [ ] Não — A página de login é servida via HTTP, permitindo form-action hijacking ou sniffing de credenciais  

### A flag `Secure` é aplicada a todos os cookies de sessão sensíveis?
- [ ] Sim — A flag `Secure` está **aplicada** a todos os cookies de autenticação e sessão  
- [ ] Sim — A flag `Secure` está **aplicada** apenas a alguns cookies  
- [ ] Não — A flag `Secure` **não está aplicada**, permitindo que os cookies sejam exfiltrados via requisições não criptografadas  

### O servidor suporta versões fracas de TLS ou conjuntos de cifras (cipher suites) inseguros?
- [ ] Não — Apenas TLS moderno (1.2+) e cifras fortes estão **habilitados**  
- [ ] Sim — Versões legadas (TLS 1.0/1.1) ou cifras fracas (RC4, DES, CBC) são **suportadas**  

### A aplicação impede a transmissão de credenciais para domínios de terceiros através de canais não criptografados?
- [ ] Sim — Nenhum dado sensível é enviado para domínios externos ou todas as chamadas externas são forçadas via HTTPS  
- [ ] Não — Headers de autenticação ou credenciais são enviados para endpoints de terceiros (ex: analytics ou SSO) via HTTP

---

## WSTG-ATHN-02 — Teste de Credenciais Padrão (Default Credentials)

O teste de credenciais padrão envolve a identificação de interfaces administrativas, serviços ou componentes de aplicação que ainda utilizam nomes de usuário e senhas configurados de fábrica ou fornecidos pelo fornecedor. Esta vulnerabilidade é um alvo de alta prioridade para atacantes porque frequentemente fornece um caminho imediato para o comprometimento total do sistema, acesso administrativo ou execução remota de código com esforço mínimo. Ela ocorre tipicamente em softwares comerciais (off-the-shelf), plataformas CMS, ferramentas de gerenciamento de banco de dados e hardware integrado, como dispositivos IoT ou dispositivos de rede. A exploração é geralmente realizada cruzando as versões de software identificadas com bancos de dados públicos de senhas padrão ou utilizando ferramentas automatizadas de Credential Stuffing durante as fases iniciais de reconhecimento e exploração.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Crítica / Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Ferramentas:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### As interfaces administrativas ou de gerenciamento estão expostas à internet ou a segmentos de rede não confiáveis?
- [ ] Não — nenhuma interface de gerenciamento está acessível  
- [ ] Sim — as interfaces estão presentes, mas restritas por controles de nível de rede (ex: VPN, IP Allowlist)  
- [ ] Sim — as interfaces **estão** publicamente acessíveis e exigem autenticação  

### As credenciais padrão fornecidas pelo fornecedor foram alteradas para todos os componentes identificados?
- [ ] Sim — todas as credenciais padrão **foram alteradas** em todos os componentes *(Mais seguro)*  
- [ ] Sim — as credenciais **foram alteradas** para a aplicação principal, mas componentes secundários (ex: CMS, DB) permanecem com o padrão  
- [ ] Não — as credenciais padrão **estão ativas** em uma ou mais interfaces identificáveis *(Crítico)*  

### A aplicação força a alteração da senha no primeiro login para contas novas ou administrativas?
- [ ] Sim — a alteração de senha **é obrigatória** antes que qualquer ação administrativa possa ser tomada  
- [ ] Não — a aplicação **permite** o uso de credenciais padrão indefinidamente  

### Mecanismos de bloqueio (lockout) ou controles de Rate Limiting estão ativos para prevenir testes automatizados de credenciais padrão?
- [ ] Sim — Rate Limiting agressivo ou bloqueio de conta **é aplicado**  
- [ ] Sim — os controles estão presentes, mas o bypass **é possível** via manipulação de cabeçalho ou rotação de IP  
- [ ] Não — nenhum mecanismo de Rate Limiting ou bloqueio **está habilitado**  

### Plugins de terceiros, middleware ou ferramentas administrativas (ex: phpMyAdmin, Tomcat Manager) utilizam credenciais únicas?
- [ ] Não — componentes de terceiros não existem no ambiente  
- [ ] Sim — todos os componentes de terceiros utilizam credenciais únicas e não padronizadas  
- [ ] Não — pelo menos um componente de terceiros **está acessível** via credenciais padrão conhecidas

---

## WSTG-ATHN-03 — Teste de Mecanismo de Bloqueio Fraco

Um mecanismo de bloqueio de conta (account lockout) é um controle crítico de defesa em profundidade (defense-in-depth) projetado para mitigar ataques de força bruta (brute-force) e credential stuffing, desativando uma conta após um número especificado de tentativas de autenticação malsucedidas. Sem uma política de bloqueio robusta, atacantes podem usar ferramentas automatizadas para adivinhar senhas sistematicamente em múltiplas contas (password spraying) ou visar uma única conta de alto valor até obter sucesso. Esta vulnerabilidade geralmente se manifesta em portais de login, formulários de redefinição de senha e endpoints de API que carecem de limitação de taxa (rate limiting) ou proteção ao nível de conta. Do ponto de vista de um atacante, um mecanismo de bloqueio fraco ou ausente permite tentativas de senha infinitas, enquanto um excessivamente agressivo pode ser explorado para causar uma Negação de Serviço (DoS) distribuída contra usuários legítimos ao bloquear intencionalmente suas contas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a aplicação lida com PII (informações de identificação pessoal) sensíveis, dados financeiros, ou se contas administrativas forem suscetíveis a brute-force sem quaisquer controles secundários.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Ferramentas:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### O mecanismo de bloqueio de conta é aplicado após um número definido de tentativas falhas?
- [ ] Sim — a conta é bloqueada após um limite definido e razoável (ex: 5-10 tentativas)  
- [ ] Sim — a conta é bloqueada, mas apenas após um número excessivamente alto de tentativas  
- [ ] Não — a conta **não pode** ser bloqueada e permite brute-force indefinido  

### O mecanismo de bloqueio pode ser burlado usando técnicas comuns de evasão?
- [ ] Não — o bloqueio é aplicado no lado do servidor (server-side) e **não** é afetado por alterações no lado do cliente (client-side)  
- [ ] Sim — **é possível** burlar o bloqueio rotacionando endereços IP ou usando um pool de proxies  
- [ ] Sim — **é possível** burlar o bloqueio manipulando cabeçalhos HTTP como `X-Forwarded-For` or `User-Agent`  
- [ ] Sim — **é possível** burlar o bloqueio limpando cookies ou iniciando novas sessões  

### O mecanismo de bloqueio fornece um caminho para recuperação de conta ou expiração?
- [ ] Sim — a conta é desbloqueada automaticamente após uma duração definida ou via verificação por e-mail  
- [ ] Sim — a conta requer intervenção administrativa para desbloqueio  
- [ ] Não — o bloqueio é permanente sem um caminho de recuperação claro, aumentando o risco de DoS  

### A aplicação diferencia entre um nome de usuário válido e inválido durante o bloqueio?
- [ ] Não — a resposta da aplicação é idêntica tanto para usuários existentes quanto para inexistentes  
- [ ] Sim — a aplicação revela se uma conta está bloqueada, permitindo a **enumeração de usuários** (username enumeration)  

### O mecanismo de bloqueio é suscetível a um ataque de Negação de Serviço (DoS)?
- [ ] Não — controles como CAPTCHA ou rate limiting baseado em IP previnem o bloqueio massivo de contas  
- [ ] Sim — um atacante **pode** bloquear sistematicamente todos os usuários conhecidos fornecendo senhas incorretas

---

## WSTG-ATHN-04 — Teste de Bypass de Esquema de Autenticação

Burlar o esquema de autenticação (Bypassing authentication schema) envolve identificar e explorar falhas na lógica da aplicação que permitem que um atacante obtenha acesso a recursos protegidos sem fornecer credenciais válidas. Esta vulnerabilidade ocorre tipicamente quando a aplicação depende de controlos do lado do cliente (client-side), falha em impor a autorização no lado do servidor (server-side) em cada pedido, ou deixa "backdoors" administrativas e endpoints de depuração (debug) expostos em produção. Os atacantes exploram estas fraquezas através de navegação forçada (forced browsing) para URLs sensíveis, manipulação de parâmetros HTTP como `authenticated=true`, ou spoofing de cabeçalhos (headers) para enganar a aplicação e fazê-la assumir que uma sessão já foi estabelecida. A exploração bem-sucedida resulta na quebra completa do perímetro de autenticação, levando potencialmente à exfiltração não autorizada de dados ou ao comprometimento total do sistema.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Ferramentas:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Páginas internas sensíveis podem ser acedidas diretamente sem uma sessão ativa?
- [ ] Não — o acesso direto resulta em redirecionamento para o login ou uma resposta 403/404  
- [ ] Sim — algumas páginas sensíveis estão acessíveis, mas fornecem dados limitados ou não sensíveis  
- [ ] Sim — páginas administrativas sensíveis ou específicas do utilizador **estão** totalmente acessíveis sem autenticação *(Crítica)*  

### A aplicação depende de flags ou parâmetros no lado do cliente para determinar o status da autenticação?
- [ ] Não — o status da autenticação é gerido estritamente através do estado da sessão no lado do servidor (server-side)  
- [ ] Sim — existem flags no lado do cliente, mas a modificação **não** concede acesso a áreas protegidas  
- [ ] Sim — a modificação de parâmetros (ex: `is_authenticated=true`, `user_role=admin`) **permite** o bypass da autenticação  

### A autenticação pode ser burlada através de spoofing ou manipulação de cabeçalhos (headers) HTTP específicos?
- [ ] Não — cabeçalhos como `X-Forwarded-For`, `Referer`, ou cabeçalhos personalizados não têm impacto na autenticação  
- [ ] Sim — a manipulação de cabeçalhos (ex: `X-Custom-IP-Authorization`, `X-Remote-User`) **é possível** e concede acesso não autorizado  

### Existem caminhos alternativos ou artefactos de desenvolvimento que contornam o fluxo de login padrão?
- [ ] Não — todos os recursos protegidos são controlados por uma verificação de autenticação centralizada e pronta para produção  
- [ ] Sim — endpoints legados, rotas de depuração (ex: `/debug/login`), ou versões de API esquecidas **estão** acessíveis sem credenciais  

### A aplicação falha em reautenticar ou verificar sessões para ações de privilégio elevado?
- [ ] Não — ações de privilégio elevado ou sensíveis exigem um token de sessão novo ou válido  
- [ ] Sim — uma vez que um bypass é encontrado num endpoint, este **pode** ser usado para realizar ações administrativas em toda a aplicação

---

## WSTG-ATHN-05 — Teste para "Lembrar Senha" Vulnerável

A funcionalidade "Lembrar-me" ou login persistente é projetada para manter o estado autenticado de um usuário após o reinício do navegador, armazenando um token ou credencial no lado do cliente. Se esse recurso for mal implementado — como o armazenamento de senhas em texto claro (plaintext), hashes fracos ou tokens de baixa entropia em cookies — ele expõe o usuário ao account takeover por meio de Cross-Site Scripting (XSS), acesso físico ou interceptação de rede. Pentesters avaliam a entropia desses tokens de persistência, as flags de segurança aplicadas ao mecanismo de armazenamento e o ciclo de vida da sessão para garantir que a conveniência do acesso persistente não ignore controles de segurança críticos, como autenticação de múltiplos fatores (MFA) ou invalidação de sessão. A exploração geralmente envolve a coleta desses tokens de longa duração para obter acesso não autorizado a contas sem exigir as credenciais originais.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Médio / Alto* |

> *A severidade torna-se Alta se credenciais em texto claro ou hashes sem salt forem armazenados no cookie persistente, ou se o token permitir o bypass de Autenticação de Múltiplos Fatores (MFA).

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Ferramentas:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### O aplicativo implementa uma funcionalidade "Lembrar-me" ou "Manter Conectado"?
- [ ] Não — o recurso **não** existe no aplicativo  
- [ ] Sim — o recurso existe e utiliza tokens criptograficamente fortes e não previsíveis  
- [ ] Sim — o recurso existe, mas utiliza tokens previsíveis ou de baixa entropia *(Médio)*  
- [ ] Sim — o recurso existe e armazena credenciais em texto claro (plaintext) ou senhas codificadas em base64 *(Alto)*  

### As flags de segurança estão aplicadas corretamente ao cookie persistente?
- [ ] Sim — as flags `HttpOnly`, `Secure` e `SameSite` estão **aplicadas**  
- [ ] Sim — algumas flags estão aplicadas, mas a `HttpOnly` está **ausente**  
- [ ] Sim — algumas flags estão aplicadas, mas a `Secure` está **ausente** em conexões HTTPS  
- [ ] Não — nenhuma flag de segurança está **aplicada** ao cookie persistente  

### O token de "Lembrar-me" permanece válido após um logout manual?
- [ ] Não — o token é invalidado no lado do servidor imediatamente após o logout  
- [ ] Sim — o token permanece válido, mas exige uma senha para ações sensíveis  
- [ ] Sim — o token permanece válido e permite acesso total à conta **sem** reautenticação *(Médio)*  

### A sessão persistente é encerrada após uma alteração de senha?
- [ ] Sim — todos os tokens persistentes são revogados quando o usuário altera sua senha  
- [ ] Não — o token de "Lembrar-me" permanece válido após uma redefinição ou alteração de senha **ser realizada** *(Alto)*  

### O token persistente ignora a Autenticação de Múltiplos Fatores (MFA) em visitas subsequentes?
- [ ] Não — o MFA é exigido mesmo quando um token de "Lembrar-me" está presente  
- [ ] Sim — o MFA é ignorado, mas o token está vinculado a um fingerprint de dispositivo específico  
- [ ] Sim — o MFA é **totalmente ignorado** usando apenas o cookie persistente de qualquer dispositivo *(Alto)*

---

## WSTG-ATHN-06 — Teste de Fraqueza no Cache do Navegador

A fraqueza no cache do navegador ocorre quando informações sensíveis são armazenadas no cache local do navegador e podem ser recuperadas por um usuário não autorizado com acesso à mesma máquina física. Essa falta de cabeçalhos de controle de cache adequados permite que dados potencialmente sensíveis, como informações pessoais, detalhes da conta ou identificadores de sessão, persistam após o usuário ter feito logout ou fechado a sessão. Atacantes ou usuários subsequentes em terminais compartilhados ou públicos podem explorar isso navegando pelo histórico do navegador, usando o botão "Voltar" ou inspecionando arquivos de cache locais para exfiltrar respostas armazenadas em cache. Garantir a implementação de diretivas rigorosas como `Cache-Control: no-store` e `Pragma: no-cache` é crítico para qualquer aplicação que manipule conteúdo autenticado, para assegurar que dados sensíveis nunca sejam gravados no disco.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média* |

> *A severidade torna-se Média se a aplicação for acessada frequentemente a partir de quiosques compartilhados/públicos ou contiver dados PII/PHI altamente sensíveis.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Ferramentas:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Cabeçalhos de controle de cache apropriados estão presentes em páginas autenticadas ou sensíveis?
- [ ] Sim — `Cache-Control: no-store, no-cache` e `Pragma: no-cache` estão **presentes** e **configurados corretamente**  
- [ ] Sim — alguns cabeçalhos estão presentes, mas o `no-store` está **ausente**, permitindo o cache em disco  
- [ ] Não — nenhum cabeçalho relacionado ao cache é **aplicado**, dependendo do comportamento padrão do navegador  

### A aplicação utiliza a diretiva `private` para dados específicos do usuário?
- [ ] Sim — `Cache-Control: private` está **habilitado** para todo o conteúdo personalizado para evitar o cache por proxies  
- [ ] Não — o conteúdo está marcado como `public` ou carece da diretiva `private`, tornando-o **passível de cache** por proxies intermediários  

### Informações sensíveis estão acessíveis através do botão "Voltar" do navegador após um logout bem-sucedido?
- [ ] Não — o navegador solicita nova autenticação ou a página **não é carregada** a partir do cache local  
- [ ] Sim — informações sensíveis **ainda estão visíveis** através do botão "Voltar" após a sessão ter sido encerrada  

### Arquivos não-HTML sensíveis (ex: PDFs, exportações CSV, respostas JSON de API) estão protegidos contra cache?
- [ ] Não — nenhum arquivo não-HTML sensível é gerado ou manipulado pela aplicação  
- [ ] Sim — cabeçalhos de controle de cache rigorosos são **aplicados** a todos os downloads de arquivos sensíveis e endpoints de API  
- [ ] Não — downloads sensíveis ou respostas de API são **armazenados** no cache local do navegador ou em diretórios temporários  

### A aplicação utiliza `Expires: 0` ou uma data no passado para evitar o uso de dados obsoletos?
- [ ] Sim — o cabeçalho `Expires` está definido como `0` ou uma data histórica, **garantindo** que o navegador trate o conteúdo como imediatamente obsoleto  
- [ ] Não — o cabeçalho `Expires` está **ausente** ou definido para uma data futura em páginas sensíveis

---

## WSTG-ATHN-07 — Teste de Políticas de Senhas Fracas

O teste de políticas de senhas fracas envolve a avaliação dos requisitos de complexidade, comprimento e entropia impostos por uma aplicação durante a criação de contas e atualizações de credenciais. Políticas insuficientes comprometem diretamente a confidencialidade das contas de utilizador, tornando-as suscetíveis a ataques de dicionário automatizados, tentativas de Brute Force e Credential Stuffing. Estas vulnerabilidades residem tipicamente em endpoints de registo, fluxos de redefinição de senha e painéis administrativos de gestão de utilizadores. Do ponto de vista de um atacante, uma política permissiva reduz significativamente o custo computacional e o tempo necessário para comprometer credenciais com sucesso utilizando wordlists comuns ou quebra de senhas baseada em padrões.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Baixa* |

> *A severidade torna-se Alta se a aplicação carecer de mecanismos de bloqueio de conta ou de Rate Limiting para prevenir adivinhação automatizada.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Ferramentas:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### O comprimento mínimo de senha é imposto pela aplicação?
- [ ] Sim — o comprimento mínimo é de 12+ caracteres *(Mais seguro)*  
- [ ] Sim — o comprimento mínimo está entre 8 e 11 caracteres  
- [ ] Sim — o comprimento mínimo é **inferior a** 8 caracteres  
- [ ] Não — nenhum comprimento mínimo é imposto  

### Os requisitos de complexidade (maiúsculas, minúsculas, dígitos, símbolos) são impostos?
- [ ] Sim — múltiplas classes de caracteres são obrigatórias e o bypass **não é possível**  
- [ ] Sim — as classes de caracteres são sugeridas, mas **não** impostas  
- [ ] Não — nenhum requisito de complexidade é aplicado  

### A aplicação bloqueia senhas comuns/fracas ou senhas que contenham dados específicos do utilizador?
- [ ] Sim — senhas fracas conhecidas (ex: "password123") e senhas baseadas no nome de utilizador **não podem** ser utilizadas  
- [ ] Sim — senhas comuns são bloqueadas, mas dados específicos do utilizador (ex: nome de utilizador, e-mail) **podem** ser utilizados  
- [ ] Não — qualquer string que cumpra os requisitos de comprimento/complexidade é aceite  

### Os requisitos de complexidade ou comprimento podem ser contornados via modificação no client-side?
- [ ] Não — as políticas são estritamente impostas no **server-side**  
- [ ] Sim — as políticas são impostas via JavaScript e **podem** ser contornadas intercetando o pedido (request)  

### A política de senhas é comunicada claramente ao utilizador sem vazar detalhes de implementação?
- [ ] Sim — a política é visível durante a introdução de dados e fornece mensagens de falha genéricas  
- [ ] Sim — a política é visível, mas as mensagens de falha revelam lacunas de lógica ou regex específicas  
- [ ] Não — a política **não** é visível, forçando a descoberta por tentativa e erro

---

## WSTG-ATHN-08 — Teste de Respostas Fracas a Perguntas de Segurança

O teste de respostas fracas a perguntas de segurança envolve a avaliação dos mecanismos de autenticação baseada em conhecimento (KBA) utilizados para verificar a identidade de um usuário, normalmente durante fluxos de recuperação de senha ou autenticação de múltiplos fatores. Isso é importante porque as perguntas de segurança geralmente dependem de informações estáticas e não secretas que podem ser facilmente descobertas por meio de inteligência de fontes abertas (OSINT) ou Engenharia Social, levando ao Account Takeover não autorizado. Essas vulnerabilidades ocorrem tipicamente nos módulos de "Esqueci a Senha" ou "Desbloqueio de Conta", onde os usuários fornecem respostas a perguntas pré-definidas. Do ponto de vista de um atacante, este é um alvo principal para Brute Force automatizado ou pesquisa direcionada, já que muitos usuários fornecem respostas previsíveis ou publicamente verificáveis para perguntas comuns como "Qual é o nome de solteira da sua mãe?" ou "Qual escola secundária você frequentou?".

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a pergunta de segurança for o único requisito para uma redefinição completa de senha e Account Takeover sem verificação adicional.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Ferramentas:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### A aplicação implementa perguntas de segurança para verificação de identidade ou recuperação de senha?
- [ ] Não — perguntas de segurança **não são utilizadas** para autenticação ou recuperação  
- [ ] Sim — perguntas de segurança são **utilizadas** como um fator secundário opcional  
- [ ] Sim — perguntas de segurança são **utilizadas** como o mecanismo de recuperação primário/único *(Alto Risco)*  

### As perguntas de segurança são selecionadas de uma lista fixa ou são definidas pelo usuário?
- [ ] Não — as perguntas são inteiramente definidas pelo usuário e exigem alta entropia  
- [ ] Sim — as perguntas são fixas, mas incluem opções não descobertas via OSINT  
- [ ] Sim — as perguntas são de uma lista fixa de tópicos comuns e fáceis de descobrir via OSINT *(Vulnerável)*  

### O Rate Limiting ou bloqueio de conta é aplicado às tentativas de resposta às perguntas de segurança?
- [ ] Sim — Rate Limiting rigoroso e bloqueio de conta (Account Lockout) **são aplicados** para evitar Brute Force  
- [ ] Sim — o Rate Limiting **é aplicado**, mas o Exploit de Bypass **é possível** via rotação de IP ou manipulação de cabeçalhos (Headers)  
- [ ] Não — **nenhum Rate Limiting** é aplicado às tentativas de resposta  

### A aplicação impõe complexidade ou validação no conteúdo da resposta?
- [ ] Sim — a validação impede palavras comuns e garante a complexidade/exclusividade da resposta  
- [ ] Sim — a validação básica está presente, mas **pode** ser burlada com listas de palavras comuns ou ataques de dicionário  
- [ ] Não — **nenhuma validação** é realizada; respostas simples ou de um único caractere **são permitidas**  

### O desafio da pergunta de segurança pode ser burlado através de falhas lógicas?
- [ ] Não — o fluxo de recuperação exige estritamente uma resposta válida para prosseguir  
- [ ] Sim — o fluxo de recuperação **pode** ser burlado (Bypass) manipulando códigos de resposta HTTP ou parâmetros de sessão  
- [ ] Sim — a resposta é refletida no código-fonte ou em campos ocultos da página

---

## WSTG-ATHN-09 — Teste de Funcionalidades Fracas de Alteração ou Redefinição de Senha

Mecanismos de alteração e redefinição de senha são componentes críticos do gerenciamento de autenticação que, se implementados incorretamente, permitem que atacantes assumam o controle de contas de usuários. Essas vulnerabilidades geralmente envolvem tokens de redefinição previsíveis, falta de bloqueios de conta durante tentativas de Brute Force, entrega insegura de tokens ou a capacidade de alterar senhas sem verificar a senha atual. Atacantes exploram essas falhas interceptando ou adivinhando links de recuperação, realizando Host Header Injection para redirecionar e-mails de redefinição ou utilizando fixação de sessão para manter o acesso após uma alteração de senha. Garantir que esses processos exijam uma verificação forte e utilizem aleatoriedade criptográfica é essencial para manter a integridade da conta e evitar o acesso não autorizado.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### A senha atual é obrigatória para definir uma nova senha durante uma alteração padrão?
- [ ] Sim — a senha atual **é obrigatória** e verificada  
- [ ] Sim — a senha atual **é obrigatória**, mas pode ser burlada via Parameter Pollution ou manipulação de parâmetros  
- [ ] Não — a senha atual **não é** solicitada, permitindo Account Takeover via Session Hijacking *(Alta)*  

### Os tokens de redefinição de senha são criptograficamente seguros e imprevisíveis?
- [ ] Sim — os tokens possuem alta entropia, são aleatórios e **não são previsíveis**  
- [ ] Sim — os tokens são gerados, mas apresentam baixa entropia ou padrões previsíveis (ex: baseados em timestamp)  
- [ ] Não — os tokens são **previsíveis** ou baseados em dados estáticos do usuário, como e-mail/ID codificado em Base64 *(Crítica)*  

### O link de redefinição de senha pode ser redirecionado via Host Header Injection?
- [ ] Não — a aplicação ignora ou valida o cabeçalho `Host` para a geração de links  
- [ ] Sim — a aplicação utiliza o cabeçalho `Host` ou `X-Forwarded-Host` para construir links, mas a validação **está presente**  
- [ ] Sim — os links **podem** ser redirecionados para um domínio controlado pelo atacante via manipulação de cabeçalho *(Alta)*  

### O token de redefinição possui um ciclo de vida limitado e invalidação imediata?
- [ ] Sim — os tokens expiram rapidamente e são invalidados **imediatamente** após o uso ou alteração de senha  
- [ ] Sim — os tokens expiram após um longo período (ex: 24+ horas) ou permitem **múltiplos usos**  
- [ ] Não — os tokens **nunca expiram** ou permanecem válidos após a senha ter sido alterada com sucesso  

### Existem Rate Limiting ou bloqueios nos endpoints de redefinição e alteração de senha?
- [ ] Sim — Rate Limiting rigoroso e CAPTCHAs **são aplicados** para evitar enumeração e Brute Force  
- [ ] Sim — existem controles limitados, mas o bypass **é possível** via rotação de IP ou Header Spoofing  
- [ ] Não — nenhum Rate Limiting **é aplicado**, permitindo a enumeração de contas em massa ou Brute Force de tokens

---

## WSTG-ATHN-10 — Teste de Autenticação Fraca em Canais Alternativos

O teste de autenticação fraca em canais alternativos envolve a identificação e análise de caminhos secundários para acesso à conta — como APIs móveis, fluxos de recuperação de senha, interfaces de suporte (help desk) ou subdomínios legados — que podem não aplicar o mesmo rigor de segurança que o portal web principal. Esses canais alternativos frequentemente carecem de autenticação multifator (MFA) robusta, Rate Limiting rigoroso ou requisitos complexos de senha, criando um "elo mais fraco" que compromete todo o sistema de autenticação. Atacantes visam especificamente esses pontos de entrada negligenciados para realizar Credential Stuffing ou burlar o MFA, explorando discrepâncias na forma como diferentes interfaces lidam com a verificação de identidade. Garantir a paridade em todas as superfícies de autenticação é essencial para evitar o acesso não autorizado e manter a integridade da sessão e dos dados do usuário.


| Campo | Valor |
|---|---|
| **ID WSTG** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Canais de autenticação alternativos estão presentes e foram identificados?
- [ ] Não — existe apenas um único canal de autenticação  
- [ ] Sim — múltiplos canais identificados (ex: API de aplicativo móvel, cliente desktop, portal legado, serviços SOAP)  

### Os canais alternativos aplicam paridade de segurança com o canal principal?
- [ ] Sim — todos os canais aplicam requisitos de complexidade de senha e MFA **idênticos**  
- [ ] Sim — os canais aplicam complexidade de senha, mas o MFA **não é obrigatório** ou **pode** ser burlado  
- [ ] Não — os canais alternativos possuem requisitos de autenticação **significativamente mais fracos**  

### O Rate Limiting e o bloqueio de conta são consistentes em todos os canais?
- [ ] Sim — o Rate Limiting e o bloqueio são **aplicados de forma consistente** em todos os endpoints  
- [ ] Sim — os controles estão presentes, mas o bypass **é possível** em canais específicos (ex: API móvel)  
- [ ] Não — o Rate Limiting ou o bloqueio **não são aplicados** a caminhos de autenticação secundários  

### O MFA pode ser burlado ao mudar para um canal alternativo?
- [ ] Não — o MFA é obrigatório e **não pode** ser burlado, independentemente do ponto de entrada  
- [ ] Sim — o MFA é exigido no portal web, mas **não é aplicado** na API móvel ou legada  

### Os fluxos de recuperação de senha ou "esqueci minha senha" são mais fracos que o login padrão?
- [ ] Não — os fluxos de recuperação utilizam verificação out-of-band forte e não vazam informações  
- [ ] Sim — os fluxos de recuperação usam perguntas de segurança fracas que **podem** ser facilmente adivinhadas ou pesquisadas  
- [ ] Sim — os fluxos de recuperação são vulneráveis a enumeração ou **não exigem** o mesmo nível de comprovação que um login padrão

---

## WSTG-ATHN-11 — Testando Autenticação Multifator (MFA)

O teste de Autenticação Multifator (MFA) avalia a robustez da camada secundária de segurança projetada para impedir o acesso não autorizado, mesmo quando as credenciais primárias estão comprometidas. Atacantes frequentemente tentam burlar o MFA identificando endpoints onde ele é aplicado de forma inconsistente, como versões legadas de API, backends móveis ou fluxos de redefinição de senha. A exploração geralmente envolve a manipulação de respostas do servidor, brute-forcing de códigos de curta duração (OTP) ou a exploração de race conditions e falhas de gerenciamento de sessão que permitem que um usuário transite para um estado autenticado sem fornecer o segundo fator.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### O MFA é aplicado de forma consistente em todos os portais de autenticação?
- [ ] Sim — O MFA é obrigatório para todas as tentativas de login via web, mobile e baseadas em API  
- [ ] Sim — mas o MFA **não é aplicado** em endpoints específicos (ex: `/api/v1/login` legado ou portais específicos para mobile)  
- [ ] Não — O MFA **não está implementado** na aplicação *(Crítico)*  

### A etapa de verificação de MFA pode ser burlada via navegação direta por URL ou manipulação de resposta?
- [ ] Não — a navegação direta ou a adulteração de parâmetros (parameter tampering) **não podem** burlar o desafio  
- [ ] Sim — navegar diretamente para URLs de dashboards internos **é possível** sem concluir o desafio de MFA  
- [ ] Sim — manipular a resposta do servidor (ex: alterar um `401 Unauthorized` para `200 OK`) **é possível** para obter acesso  

### O processo de verificação de código MFA está protegido contra ataques de brute-force?
- [ ] Sim — rate limiting rigoroso ou bloqueio de conta **é aplicado** após múltiplas tentativas falhas de OTP  
- [ ] Sim — o rate limiting existe, mas **é possível** burlá-lo via rotação de IP ou manipulação de headers  
- [ ] Não — nenhum rate limiting é aplicado, e o brute-forcing automatizado de códigos **é possível**  

### A aplicação mantém um estado de sessão seguro durante a transição de MFA?
- [ ] Sim — tokens de sessão de alto privilégio são emitidos **apenas após** a conclusão bem-sucedida do MFA  
- [ ] Não — um cookie de sessão totalmente funcional é emitido **antes** da conclusão do MFA, permitindo acesso a alguns recursos  
- [ ] Não — a aplicação utiliza um identificador estático ou uma transição de sessão previsível que **pode** ser sequestrada (hijacked)  

### Fatores alternativos de MFA (SMS, E-mail, Códigos de Backup) são vulneráveis a exploração?
- [ ] Não — todos os fatores utilizam valores seguros, não previsíveis e com tempo limitado  
- [ ] Sim — os códigos de backup são previsíveis ou **podem** ser enumerados  
- [ ] Sim — a funcionalidade "Reenviar Código" pode ser abusada para realizar SMS/Email flooding ou revelar detalhes parciais de contato

---

## WSTG-ATHZ-01 — Testando Directory Traversal e File Include

Vulnerabilidades de Directory Traversal e inclusão de arquivos (File Include) surgem quando uma aplicação utiliza entrada controlável pelo usuário para construir caminhos para arquivos ou diretórios sem validação ou sanitização suficiente. Atacantes exploram essas falhas injetando sequências como `../` para navegar fora do diretório pretendido, potencialmente acessando arquivos sensíveis do sistema, dados de configuração ou código-fonte da aplicação. Em casos mais graves envolvendo Local File Inclusion (LFI) ou Remote File Inclusion (RFI), um atacante pode obter execução remota de código (Remote Code Execution) incluindo scripts maliciosos ou aproveitando log poisoning e wrappers de PHP. Essas vulnerabilidades normalmente residem em parâmetros usados para carregamento dinâmico de conteúdo, mecanismos de template (template engines) ou endpoints de recuperação de imagens onde a lógica do lado do servidor (server-side) manipula caminhos do sistema de arquivos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Ferramentas:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Os parâmetros aceitam nomes de arquivos ou caminhos para processamento no servidor?
- [ ] Não — nenhum parâmetro parece interagir com o sistema de arquivos  
- [ ] Sim — existem parâmetros, mas utilizam uma allowlist rigorosa de identificadores de arquivos  
- [ ] Sim — os parâmetros aceitam nomes de arquivos ou caminhos diretos  

### A entrada é sanitizada para evitar sequências de directory traversal?
- [ ] Sim — a entrada é validada em relação a uma allowlist rigorosa e as sequências de traversal **não são possíveis**  
- [ ] Sim — a entrada é sanitizada removendo sequências `../`, mas um bypass recursivo **é possível**  
- [ ] Não — nenhuma sanitização ou validação **é aplicada** à entrada relacionada ao caminho  

### É possível acessar arquivos fora do diretório restrito através de sequências de traversal?
- [ ] Não — a aplicação ou o SO impede o acesso a arquivos fora do escopo definido  
- [ ] Sim — o acesso a arquivos dentro da raiz da web (web root) **é possível**  
- [ ] Sim — o acesso a arquivos sensíveis do sistema (ex: `/etc/passwd`, `C:\Windows\win.ini`) **é possível** *(Crítica)*  

### O servidor permite a inclusão de URLs remotas (RFI)?
- [ ] Não — a inclusão de arquivos remotos está **desativada** no nível do servidor/aplicação  
- [ ] Sim — arquivos remotos **podem** ser incluídos, mas a execução **não é possível**  
- [ ] Sim — arquivos remotos **podem** ser incluídos e executados no servidor *(Crítica)*  

### Os filtros podem ser burlados usando codificação ou caracteres especiais?
- [ ] Não — os filtros lidam de forma eficaz com várias codificações e null bytes  
- [ ] Sim — o bypass **é possível** usando URL encoding, double URL encoding ou Unicode de 16 bits  
- [ ] Sim — o bypass **é possível** usando injeção de null byte (`%00`) ou wrappers do sistema de arquivos (ex: `php://filter`)

---

## WSTG-ATHZ-02 — Teste de Bypass ao Esquema de Autorização

O bypass de esquemas de autorização ocorre quando uma aplicação falha em aplicar controles de acesso, permitindo que um atacante acesse recursos ou funções fora de suas permissões pretendidas. Esta vulnerabilidade normalmente se manifesta como Escalação de Privilégios Horizontal (Horizontal Privilege Escalation), onde um atacante acessa dados pertencentes a outro usuário do mesmo nível, ou Escalação de Privilégios Vertical (Vertical Privilege Escalation), onde um usuário de baixo privilégio obtém capacidades administrativas. Atacantes exploram essas falhas manipulando parâmetros de requisição, como IDs de recursos, modificando tokens de sessão ou utilizando cabeçalhos HTTP como `X-Original-URL` para contornar restrições baseadas em caminhos (path-based restrictions). Garantir uma autorização robusta é crítico para manter a confidencialidade dos dados e prevenir ações administrativas não autorizadas que poderiam comprometer todo o sistema.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Ferramentas:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### A escalação de privilégios horizontal é evitada entre usuários do mesmo perfil (role)?
- [ ] Sim — as verificações de autorização **são aplicadas** a cada requisição de recurso e o bypass **não é possível**  
- [ ] Sim — as verificações de autorização **são aplicadas**, mas podem ser burladas via IDOR ou manipulação de parâmetros (parameter tampering)  
- [ ] Não — os usuários **podem** acessar dados pertencentes a outros usuários simplesmente alterando um ID de recurso *(Alta)*  

### A escalação de privilégios vertical é evitada para usuários de baixo privilégio?
- [ ] Não — a aplicação possui apenas um nível de privilégio  
- [ ] Sim — funções administrativas **não podem** ser acessadas por usuários de baixo privilégio  
- [ ] Sim — as funções administrativas estão ocultas na interface (UI), mas **podem** ser acessadas via URL direta  
- [ ] Não — usuários de baixo privilégio **podem** realizar ações administrativas manipulando perfis (roles) ou permissões *(Crítica)*  

### A autorização pode ser burlada utilizando tampering de verbos HTTP?
- [ ] Não — os controles de acesso são aplicados independentemente do método HTTP utilizado  
- [ ] Sim — alterar o método (ex: de `GET` para `POST` ou `PUT`) **burla** as verificações de autorização  

### Endpoints administrativos ou restritos estão acessíveis sem qualquer autenticação?
- [ ] Não — todos os endpoints sensíveis exigem uma sessão válida e permissões adequadas  
- [ ] Sim — alguns endpoints administrativos são acessíveis se o atacante conhecer a URL direta  
- [ ] Sim — o acesso não autenticado a funções sensíveis **é possível** *(Crítica)*  

### Os cabeçalhos HTTP permitem a circunvenção da autorização baseada em caminho (path)?
- [ ] Não — a aplicação **não é** suscetível a bypasses baseados em cabeçalhos (header-based bypasses)  
- [ ] Sim — cabeçalhos como `X-Original-URL`, `X-Rewrite-URL` ou `X-Forwarded-For` **podem** ser usados para burlar controles de acesso

---

## WSTG-ATHZ-03 — Teste de Escalação de Privilégios

A escalação de privilégios ocorre quando um atacante explora vulnerabilidades para obter acesso a recursos ou funcionalidades reservados a usuários com níveis de autorização mais elevados ou identidades diferentes. Na escalação vertical, um usuário padrão tenta acessar funções administrativas, enquanto a escalação horizontal envolve o acesso a dados pertencentes a outro usuário com o mesmo nível de privilégio. Essas falhas normalmente se manifestam em listas de controle de acesso (ACLs) mal implementadas, referências diretas a objetos inseguras (IDOR) ou manipulação de parâmetros (parameter tampering) dentro de tokens de sessão ou de identidade. Do ponto de vista de um atacante, isso é frequentemente alcançado manipulando IDs numéricos, modificando parâmetros baseados em funções em cookies ou JWTs, ou chamando diretamente endpoints de API ocultos que carecem de verificações de autorização no lado do servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Ferramentas:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### A aplicação implementa múltiplos níveis de privilégio ou multi-tenancy?
- [ ] Não — a aplicação é um sistema de usuário único ou função única  
- [ ] Sim — múltiplas funções ou tenants **estão** definidos e exigem testes de autorização  

### A escalação de privilégios horizontal (acesso ao mesmo nível) é possível?
- [ ] Não — as verificações de autorização **são aplicadas** a todos os identificadores de recursos e proprietários de sessão  
- [ ] Sim — o acesso aos dados de outros usuários **é possível** através de IDOR ou manipulação de parâmetros  
- [ ] Sim — o vazamento de dados **é possível**, mas a modificação de dados de outros usuários **não é possível**  

### A escalação de privilégios vertical (baixo-para-alto) é possível?
- [ ] Não — os endpoints administrativos aplicam estritamente controles de acesso baseados em funções (RBAC)  
- [ ] Sim — as funções administrativas **podem** ser acessadas por usuários com baixos privilégios através de acesso direto via URL  
- [ ] Sim — as funções administrativas **podem** ser acessadas manipulando cabeçalhos relacionados a funções, cookies ou claims de JWT  

### As funções ou permissões de usuário podem ser modificadas via Mass Assignment ou Parameter Pollution?
- [ ] Não — os campos baseados em funções são estritamente de leitura e **não podem** ser modificados pelos usuários  
- [ ] Sim — as permissões de usuário **podem** ser elevadas incluindo parâmetros ocultos (ex: `role=admin`) em atualizações de perfil ou registro  

### As verificações de autorização são aplicadas de forma consistente em todas as versões de API e métodos HTTP?
- [ ] Sim — os controles **são aplicados** consistentemente em todas as versões e métodos  
- [ ] Não — versões legadas da API (ex: `/v1/`) ou métodos HTTP específicos (ex: `PUT`, `DELETE`, `PATCH`) **ignoram** a autorização  
- [ ] Não — as funções administrativas estão ocultas na UI, mas **habilitadas** no nível da API para todos os usuários

---

## WSTG-ATHZ-04 — Teste de Referências Diretas Inseguras a Objetos (IDOR)

As Referências Diretas Inseguras a Objetos (IDOR) ocorrem quando uma aplicação fornece acesso direto a objetos com base em entradas fornecidas pelo usuário sem realizar uma verificação de autorização para validar as permissões do solicitante. Esta vulnerabilidade impacta significativamente a confidencialidade e a integridade, pois permite que atacantes visualizem, modifiquem ou excluam dados pertencentes a outros usuários ou ao sistema. Geralmente, manifesta-se em parâmetros de URL, dados no corpo de requisições POST ou chaves JSON que se referem a chaves internas de bancos de dados, nomes de arquivos ou identificadores de conta. Do ponto de vista de um atacante, a exploração envolve a manipulação desses identificadores — frequentemente por meio do incremento de números inteiros ou fuzzing de UUIDs — para acessar recursos fora de seu escopo autorizado.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Ferramentas:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### A aplicação utiliza identificadores diretos de objetos nos parâmetros da requisição?
- [ ] Não — a aplicação utiliza referências indiretas ou mapas criptografados *(Mais seguro)*  
- [ ] Sim — a aplicação utiliza identificadores não previsíveis (ex: UUIDs longos/hashes)  
- [ ] Sim — a aplicação utiliza identificadores previsíveis (ex: inteiros incrementais ou nomes de usuário)  

### A autorização no lado do servidor (server-side) é realizada para cada requisição que envolve um identificador de objeto?
- [ ] Sim — as verificações no lado do servidor **não podem** ser burladas para nenhum identificador  
- [ ] Sim — as verificações no lado do servidor estão presentes, mas o bypass **é possível** via parameter pollution ou manipulação de cabeçalhos  
- [ ] Não — nenhuma verificação de autorização **é aplicada** para verificar a propriedade do objeto solicitado  

### Um atacante pode acessar ou modificar objetos pertencentes a outros usuários (IDOR Horizontal)?
- [ ] Não — o acesso a dados de outros usuários **não é possível**  
- [ ] Sim — a visualização de dados de outros usuários **é possível**, mas a modificação **não é possível**  
- [ ] Sim — tanto a visualização quanto a modificação de dados de outros usuários **são possíveis** *(Crítico)*  

### É possível acessar objetos administrativos ou de nível de sistema (IDOR Vertical)?
- [ ] Não — o acesso a objetos de privilégios mais elevados **não é possível**  
- [ ] Sim — o acesso a configurações administrativas ou arquivos do sistema **é possível**  

### A aplicação vaza identificadores de objetos por meio de busca, metadados ou outros endpoints?
- [ ] Não — os identificadores **não são expostos** involuntariamente  
- [ ] Sim — identificadores de outros usuários/objetos **podem** ser enumerados através de endpoints secundários ou perfis públicos

---

## WSTG-ATHZ-05 — Teste de Fraquezas no OAuth

As fraquezas no OAuth abrangem uma variedade de falhas na implementação dos protocolos OAuth 2.0 ou OpenID Connect (OIDC), resultando frequentemente em account takeover completo ou acesso não autorizado a dados. Estas vulnerabilidades manifestam-se tipicamente durante o fluxo de autorização, especificamente no que diz respeito à validação do `redirect_uri`, à entropia e verificação do parâmetro `state`, e ao manuseio seguro de access ou ID tokens. Atacantes exploram estas falhas interceptando authorization codes, redirecionando tokens para domínios controlados pelo atacante através de open redirects, ou realizando Cross-Site Request Forgery (CSRF) para vincular a sessão de uma vítima à conta de um atacante. Uma vez que o OAuth serve frequentemente como um mecanismo de autenticação primário, uma única configuração incorreta pode comprometer a integridade de todo o sistema de identidade do utilizador.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Ferramentas:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### O parâmetro `state` está implementado e é validado para prevenir CSRF?
- [ ] Sim — o `state` é obrigatório, único por sessão e estritamente validado no servidor  
- [ ] Sim — o `state` está presente, mas é estático ou previsível entre diferentes sessões  
- [ ] Sim — o `state` está presente, mas a aplicação **não** o valida no callback  
- [ ] Não — o parâmetro `state` **não** é utilizado na solicitação de autorização *(Crítica)*  

### O `redirect_uri` é estritamente validado contra uma whitelist?
- [ ] Sim — apenas correspondências exatas com uma whitelist pré-registada são **aceites**  
- [ ] Sim — a validação é aplicada, mas o bypass **é possível** através de path traversal ou manipulação de subdomínios  
- [ ] Sim — a validação é aplicada, mas o bypass **é possível** através de parameter pollution ou fragment injection  
- [ ] Não — a aplicação **permite** URLs arbitrários no parâmetro `redirect_uri` *(Crítica)*  

### O `response_type` pode ser manipulado para alterar o fluxo?
- [ ] Não — a aplicação aceita apenas o fluxo pretendido (ex: `code`) e rejeita outros  
- [ ] Sim — a mudança de `code` para `token` (Implicit flow) **é possível** e vaza tokens no URL  
- [ ] Sim — fluxos híbridos ou valores de `response_type` não autorizados estão **ativados** e são processados  

### Os access tokens ou authorization codes são vazados através de cabeçalhos Referer ou do histórico do navegador?
- [ ] Não — os tokens/codes são manuseados de forma segura e as páginas sensíveis utilizam uma `Referrer-Policy` adequada  
- [ ] Sim — os tokens/codes **são** vazados para domínios de terceiros através do cabeçalho `Referer`  
- [ ] Sim — os tokens/codes **estão** visíveis no histórico do navegador devido a cache insegura ou solicitações GET  

### A aplicação impõe o princípio de "Least Privilege" para os scopes do OAuth?
- [ ] Sim — a aplicação solicita apenas os scopes mínimos necessários para a funcionalidade  
- [ ] Não — a aplicação solicita scopes excessivos ou administrativos por padrão  
- [ ] Não — o utilizador **não pode** rever ou optar por não aceitar scopes específicos durante a fase de consentimento

---

## WSTG-SESS-01 — Teste de Esquema de Gerenciamento de Sessão

O teste do esquema de gerenciamento de sessão foca na análise de como a aplicação lida com o ciclo de vida e a estrutura dos identificadores de sessão para garantir que sejam robustos contra predição e sequestro (hijacking). Atacantes capturam múltiplos tokens de sessão para realizar análises estatísticas, buscando padrões, baixa entropia ou incrementos previsíveis que permitam forjar sessões válidas para outros usuários. Fraquezas no esquema frequentemente se manifestam como vulnerabilidades de fixação de sessão (Session Fixation) ou o uso de valores insuficientemente aleatórios, tipicamente encontrados em cookies HTTP, parâmetros de URL ou implementações de cabeçalhos personalizados. Comprometer o esquema de sessão é um ataque de alto impacto que concede a um adversário acesso completo ao estado autenticado de uma vítima sem a necessidade de suas credenciais primárias.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Ferramentas:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### O identificador de sessão é suficientemente complexo e aleatório?
- [ ] Sim — o token passa em testes estatísticos de aleatoriedade (alta entropia) e o bypass **não é possível**  
- [ ] Sim — o token é longo, mas contém padrões previsíveis ou segmentos estáticos  
- [ ] Não — o token é sequencial, baseado em timestamps previsíveis ou possui entropia **criticamente baixa** *(Crítico)*  

### Onde o identificador de sessão é transmitido e armazenado?
- [ ] O identificador é armazenado em um cookie `Secure` e `HttpOnly` *(Mais seguro)*  
- [ ] O identificador é armazenado em um cookie, mas faltam as flags `Secure` ou `HttpOnly`  
- [ ] O identificador é transmitido **apenas** via parâmetros de URL ou requisições `GET` *(Alto Risco)*  

### A aplicação emite um novo identificador de sessão após a autenticação bem-sucedida?
- [ ] Sim — um novo ID de sessão exclusivo **é gerado** imediatamente após o login  
- [ ] Não — o ID de sessão permanece o mesmo antes e depois do login *(Possível Session Fixation)*  

### O identificador de sessão está vinculado a um endereço IP ou User-Agent específico para evitar o reuso?
- [ ] Sim — a sessão é invalidada se o fingerprint do cliente mudar significativamente  
- [ ] Não — a sessão **pode** ser reutilizada em diferentes endereços IP ou dispositivos sem verificação adicional  

### Os identificadores de sessão são devidamente invalidados no lado do servidor após o logout ou timeout?
- [ ] Sim — o estado da sessão no lado do servidor (server-side) é **completamente destruído** após o logout  
- [ ] Não — a sessão permanece válida no servidor mesmo se o cookie do lado do cliente for excluído  
- [ ] Não — a sessão **nunca expira** ou possui um período de timeout excessivamente longo

---

## WSTG-SESS-02 — Teste de Atributos de Cookies

O gerenciamento de sessão frequentemente depende de cookies HTTP para manter o estado, tornando os atributos de segurança desses cookies um alvo primário para atacantes. Ao omitir ou configurar incorretamente atributos como `HttpOnly`, `Secure` e `SameSite`, as aplicações expõem tokens de sessão ao roubo via Cross-Site Scripting (XSS), interceptação em canais não criptografados ou ataques de Cross-Site Request Forgery (CSRF). Pentesters examinam essas flags para determinar se um atacante pode exfiltrar identificadores de sessão do Document Object Model (DOM) do navegador ou forçar o navegador a enviá-los em requisições cross-origin. A configuração adequada dos atributos é uma medida fundamental de defesa em profundidade para garantir que tokens sensíveis permaneçam confinados a contextos autorizados e seguros.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Ferramentas:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Análise da Implementação de Flags de Cookie
| Atributo | Presente | Ausente |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### A flag `HttpOnly` é aplicada a cookies de sessão sensíveis?
- [ ] Sim — aplicada a **todos** os cookies de sessão sensíveis *(Mais seguro)*  
- [ ] Sim — aplicada apenas a alguns cookies de sessão  
- [ ] Não — a flag **não é aplicada**, permitindo o acesso via scripts client-side *(Crítico)*  

### A flag `Secure` é imposta para cookies transmitidos via HTTPS?
- [ ] Sim — aplicada a **todos** os cookies transmitidos via TLS  
- [ ] Não — a flag **não é aplicada**, permitindo que os cookies sejam enviados via HTTP em texto claro  

### O atributo `SameSite` é utilizado para mitigar riscos de CSRF?
- [ ] Sim — configurado como `Strict` ou `Lax` para cookies de sessão  
- [ ] Sim — configurado como `None`, mas com a presença da flag `Secure`  
- [ ] Não — o atributo está **ausente** ou configurado como `None` **sem** `Secure`  

### Os atributos `Domain` e `Path` estão restritos ao escopo mínimo necessário?
- [ ] Sim — limitados ao subdomínio e caminho da aplicação **específicos**  
- [ ] Não — o escopo é muito **amplo** (ex: configurado para um domínio pai ou raiz `/`), aumentando a superfície de ataque  

### Os cookies de sessão sensíveis expiram adequadamente através dos atributos `Expires` ou `Max-Age`?
- [ ] Sim — datas de expiração apropriadas estão configuradas  
- [ ] Não — os cookies são persistentes com tempos de vida excessivamente **longos**  
- [ ] Não — os cookies são apenas de sessão, mas **carecem** de imposição de timeout no lado do servidor

---

## WSTG-SESS-03 — Session Fixation

A Session Fixation ocorre quando uma aplicação falha em invalidar ou rotacionar o identificador de sessão após um usuário se autenticar com sucesso, permitindo que um atacante force um token de sessão conhecido a uma vítima. Se a aplicação preservar o ID de sessão pré-autenticação após o login, um atacante pode fornecer à vítima um link especificamente criado contendo um ID de sessão fixo e, subsequentemente, sequestrar (hijack) a sessão autenticada. Esta vulnerabilidade normalmente se manifesta em formulários de login ou através de identificadores de sessão aceitos via parâmetros de URL e cookies. Do ponto de vista de um atacante, a exploração envolve "fixar" a sessão através de um link malicioso ou uma vulnerabilidade de header injection e esperar que a vítima forneça credenciais, ignorando efetivamente a necessidade de roubar um token ativo.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### O identificador de sessão muda após a autenticação bem-sucedida?
- [ ] Sim — um novo identificador de sessão **é emitido** e o antigo é invalidado *(Mais seguro)*  
- [ ] Sim — um novo identificador de sessão **é emitido**, mas o antigo **permanece válido**  
- [ ] Não — o identificador de sessão **permanece o mesmo** antes e depois da autenticação *(Crítico)*  

### A aplicação aceita identificadores de sessão fornecidos via parâmetros de URL?
- [ ] Não — os IDs de sessão são gerenciados **exclusivamente** via cookies  
- [ ] Sim — IDs de sessão são aceitos na URL, mas são **ignorados** ou **rotacionados** no login  
- [ ] Sim — IDs de sessão na URL são **aceitos** e **persistidos** após a autenticação  

### Um ID de sessão definido pelo atacante pode ser forçado na aplicação?
- [ ] Não — a aplicação **rejeita** IDs de sessão inválidos ou inexistentes fornecidos pelo usuário  
- [ ] Sim — a aplicação **aceita** e inicializa uma sessão usando qualquer ID arbitrário fornecido em um cookie  
- [ ] Sim — a aplicação **aceita** e inicializa uma sessão usando qualquer ID arbitrário fornecido em um parâmetro de URL  

### As sessões de pré-autenticação são encerradas corretamente?
- [ ] Sim — a sessão anônima é **totalmente destruída** no evento de login  
- [ ] Não — a sessão anônima **não é destruída**, permitindo a potencial coleta (session harvesting)  

### O cookie de sessão está protegido contra injeção no lado do cliente (client-side injection)?
- [ ] Sim — as flags `HttpOnly` e `Secure` são **aplicadas** para prevenir a fixação baseada em scripts  
- [ ] Não — as flags **não são aplicadas**, permitindo que IDs de sessão sejam definidos via Cross-Site Scripting (XSS)

---

## WSTG-SESS-04 — Testing for Exposed Session Variables

Variáveis de sessão expostas ocorrem quando identificadores de sessão sensíveis ou dados relacionados ao estado são transmitidos através de canais inseguros, como query strings de URL, cabeçalhos Referer ou logs do sistema. Essa exposição aumenta significativamente o risco de Session Hijacking, pois os identificadores podem ser armazenados em cache no histórico do navegador, registrados por proxies intermediários ou vazados para sites de terceiros via cabeçalho Referer. Pentesters identificam essas exposições monitorando todas as requisições de saída e inspecionando logs da aplicação ou configurações do servidor em busca de vazamentos de dados inadvertidos. Na prática, atacantes exploram esses vazamentos coletando tokens de sessão válidos de máquinas compartilhadas ou analisando padrões de tráfego para burlar a autenticação e obter acesso não autorizado a contas de usuários.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Status do Teste** | Not Performed |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a variável exposta for um token de sessão que permita o Account Takeover imediato.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Ferramentas:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### O identificador de sessão é transmitido na query string da URL?
- [ ] Não — identificadores de sessão **não** estão presentes na query string da URL  
- [ ] Sim — identificadores estão presentes, mas a sessão é de curta duração ou de baixo risco  
- [ ] Sim — IDs de sessão exclusivos **estão** presentes na query string da URL *(Crítico)*  

### A aplicação vaza tokens de sessão através do cabeçalho Referer?
- [ ] Não — a `Referrer-Policy` impede o vazamento ou não existem links para terceiros  
- [ ] Sim — tokens de sessão **são** transmitidos para domínios de terceiros via cabeçalho Referer  

### As variáveis de sessão são armazenadas em logs do servidor ou da aplicação?
- [ ] Não — as variáveis de sessão são mascaradas ou excluídas dos logs  
- [ ] Sim — as variáveis de sessão **são** registradas em logs da aplicação ou do servidor web em texto simples (plain text)  

### A aplicação utiliza requisições GET para operações que alteram o estado?
- [ ] Não — a aplicação utiliza `POST` ou outros métodos não idempotentes para operações sensíveis  
- [ ] Sim — `GET` é utilizado, fazendo com que os dados da sessão sejam armazenados em cache no histórico do navegador e em proxies intermediários  

### O cabeçalho `Cache-Control` está configurado para evitar o cache de dados relacionados à sessão?
- [ ] Sim — `Cache-Control: no-store` (ou similar) **é aplicado** a respostas sensíveis  
- [ ] Sim — os controles estão implementados, mas o bypass **é possível** através de casos de borda (edge cases) do navegador  
- [ ] Não — respostas sensíveis contendo dados de sessão **podem** ser armazenadas em cache pelo navegador

---

## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) é uma vulnerabilidade na qual um invasor engana o navegador de uma vítima para realizar uma ação indesejada em um site diferente onde a vítima está autenticada no momento. Este exploit aproveita o comportamento do navegador de anexar automaticamente credenciais "ambientais", como cookies de sessão ou cabeçalhos de Authorization, a solicitações de saída. Os invasores normalmente visam operações que alteram o estado, como alteração de senhas, atualização de endereços de e-mail ou execução de transferências financeiras, hospedando scripts maliciosos ou formulários ocultos em um site de terceiros. A exploração bem-sucedida pode levar ao total account takeover ou modificação não autorizada de dados sem o conhecimento ou consentimento do usuário.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Status do Teste** | Não Performed |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a ação vulnerável permitir account takeover, escalonamento de privilégios ou transações financeiras não autorizadas.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Ferramentas:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer para hospedagem de PoC)`

### Os tokens anti-CSRF estão implementados para solicitações que alteram o estado?
- [ ] Sim — tokens únicos e criptograficamente fortes são exigidos para todas as ações que alteram o estado  
- [ ] Sim — os tokens estão presentes, mas **não** são únicos por sessão ou são previsíveis  
- [ ] Não — os tokens anti-CSRF **não** estão implementados  

### A validação do token anti-CSRF no lado do servidor (server-side) é robusta?
- [ ] Sim — o servidor valida estritamente o token e o bypass **não é possível**  
- [ ] Sim — a validação é realizada, mas **pode** ser burlada removendo o parâmetro do token  
- [ ] Sim — a validação é realizada, mas **pode** ser burlada fornecendo um token vazio ou fictício (dummy)  
- [ ] Sim — a validação é realizada, mas o token **não** está vinculado à sessão do usuário  

### A aplicação depende de métodos facilmente burláveis para proteção contra CSRF?
- [ ] Não — a aplicação não depende apenas de cabeçalhos fracos ou verificações de origem  
- [ ] Sim — a proteção depende exclusivamente do cabeçalho `Referer` ou `Origin`, que **pode** ser falsificado ou removido  
- [ ] Sim — a proteção depende da verificação do cabeçalho `X-Requested-With`, que **pode** ser burlada através de configurações incorretas de CORS  

### Os cookies de sessão estão configurados com o atributo `SameSite`?
- [ ] Sim — `SameSite` está definido como `Strict` ou `Lax` em todos os cookies relacionados à sessão  
- [ ] Não — o atributo `SameSite` está **ausente**, assumindo o comportamento padrão do navegador  
- [ ] Não — `SameSite` está explicitamente definido como `None` sem a flag `Secure` ou mitigações adicionais  

### A proteção CSRF pode ser burlada alterando o método de solicitação HTTP?
- [ ] Não — a proteção é aplicada independentemente do método HTTP utilizado  
- [ ] Sim — os tokens são validados apenas em solicitações `POST`, mas a ação **pode** ser realizada via `GET`  
- [ ] Sim — a alteração do método (ex: para `PUT` ou `DELETE`) burla a verificação do token

---

## WSTG-SESS-06 — Teste de Funcionalidade de Logout

A funcionalidade de logout é um controle de segurança crítico projetado para encerrar a sessão de um usuário e invalidar os identificadores de sessão associados, tanto no lado do cliente quanto no lado do servidor. A falha na implementação adequada do logout permite ataques de Session Fixation ou Session Hijacking, pois um invasor que obtenha acesso a uma máquina após um usuário ter feito "logout" poderia potencialmente reutilizar o token de sessão ainda ativo. Pentesters avaliam isso verificando se o cookie de sessão é removido do navegador, se o estado da sessão no lado do servidor é explicitamente destruído e se a navegação pelo botão "voltar" permite o acesso a dados sensíveis em cache. Uma implementação de logout segura garante que, uma vez acionada a ação, nenhuma requisição subsequente com o antigo token de sessão seja autorizada pela aplicação.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Ferramentas:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Existe um mecanismo de logout funcional e acessível?
- [ ] Sim — o botão de logout existe e aciona uma requisição de encerramento  
- [ ] Não — o botão de logout está **ausente** ou **não** aciona um evento de encerramento  

### O identificador de sessão é invalidado no lado do servidor?
- [ ] Sim — o servidor rejeita todas as requisições subsequentes usando o antigo token de sessão  
- [ ] Não — o servidor **continua a aceitar** o antigo token de sessão após o logout  

### A aplicação limpa os cookies de sessão no navegador ao realizar o logout?
- [ ] Sim — os cookies são sobrescritos com uma data de expiração passada ou valor vazio  
- [ ] Sim — os cookies permanecem, mas **não são mais válidos** no servidor  
- [ ] Não — os cookies permanecem no navegador e **mantêm** seus valores originais  

### Conteúdo autenticado sensível pode ser acessado através do botão 'Voltar' do navegador após o logout?
- [ ] Não — cabeçalhos `Cache-Control: no-store` ou similares impedem a visualização de páginas sensíveis após o logout  
- [ ] Sim — páginas sensíveis estão em cache e **podem** ser visualizadas por navegação após a sessão ser encerrada

---

## WSTG-SESS-07 — Testing Session Timeout

O teste de session timeout (tempo limite de sessão) avalia se uma aplicação termina efetivamente a sessão de um utilizador após um período pré-definido de inatividade ou duração total. Este controlo é vital para mitigar o risco de Session Hijacking, particularmente em estações de trabalho partilhadas ou em cenários onde um atacante captura um identificador de sessão via interceptação de rede ou Cross-Site Scripting (XSS). Os pentesters analisam tanto os idle timeouts, que são acionados após um período de inatividade, quanto os absolute timeouts, que limitam a vida útil total de uma sessão, independentemente da atividade. Do ponto de vista de um atacante, sessões indefinidas ou excessivamente longas proporcionam uma janela de oportunidade alargada para manter acesso não autorizado e contornar a necessidade de reautenticação.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Estado do Teste** | Não Realizado |
| **Severidade** | Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### A aplicação implementa um idle session timeout?
- [ ] Sim — a sessão expira após um curto período (ex: 15-30 minutos) de inatividade  
- [ ] Sim — a sessão expira, mas a duração do timeout é **excessivamente longa** (ex: > 24 horas)  
- [ ] Não — a sessão **permanece ativa** indefinidamente durante a inatividade  

### O session timeout é imposto no lado do servidor (server-side)?
- [ ] Sim — o servidor rejeita pedidos com tokens expirados, independentemente do estado no lado do cliente (client-side)  
- [ ] Não — o timeout é **apenas** imposto via JavaScript no client-side ou meta-refresh  

### A aplicação implementa um absolute session timeout?
- [ ] Sim — a sessão é terminada após uma duração máxima fixa, independentemente da atividade  
- [ ] Não — a sessão **pode** ser estendida indefinidamente desde que haja atividade contínua  

### O identificador de sessão é invalidado no servidor após o timeout?
- [ ] Sim — o token de sessão **não pode** ser reutilizado assim que o limite de timeout é atingido  
- [ ] Não — o token de sessão **ainda é válido** no servidor mesmo após o cliente ser redirecionado para a página de login  

### A sessão pode ser mantida ativa indefinidamente utilizando pedidos de heartbeat automatizados?
- [ ] Não — o absolute timeout ou controlos secundários **impedem** a extensão infinita da sessão  
- [ ] Sim — pedidos periódicos em segundo plano (ex: API, AJAX) **podem** manter a sessão indefinidamente

---

## WSTG-SESS-08 — Session Puzzling

O Session Puzzling, também conhecido como Session Variable Overloading (Sobrecarga de Variável de Sessão), ocorre quando uma aplicação utiliza a mesma variável de sessão para múltiplos propósitos em diferentes módulos ou falha ao limpar corretamente os estados da sessão durante transições de fluxo de trabalho. Atacantes exploram esse comportamento populando variáveis de sessão em um contexto — como uma visualização de perfil não autenticada ou um registro de múltiplas etapas — e então navegando para uma área protegida onde a aplicação confia incorretamente nessas variáveis pré-existentes. Isso pode levar a impactos críticos, incluindo bypass de autenticação, escalada de privilégios (Privilege Escalation) ou acesso não autorizado a dados sensíveis, enganando a aplicação para que ela acredite que uma sessão está em um estado de privilégio mais elevado do que realmente está. A vulnerabilidade é mais prevalente em aplicações complexas e com estado (stateful), onde a lógica de gerenciamento de sessão é aplicada de forma inconsistente em diferentes componentes funcionais.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### A aplicação utiliza os mesmos nomes de variáveis de sessão para propósitos diferentes em módulos distintos?
- [ ] Não — os nomes das variáveis são únicos ou os escopos são isolados  
- [ ] Sim — existem nomes de variáveis compartilhados, mas o estado é limpo entre os módulos  
- [ ] Sim — existem nomes de variáveis compartilhados e o estado **não** é limpo  

### Ações não autenticadas podem influenciar variáveis de sessão usadas em contextos autenticados?
- [ ] Não — as variáveis de sessão são inicializadas apenas **após** a autenticação bem-sucedida  
- [ ] Sim — variáveis de sessão podem ser definidas antes do login, mas **não** afetam o estado pós-login  
- [ ] Sim — variáveis de sessão definidas durante a pré-autenticação **são** confiadas no pós-autenticação *(Crítico)*  

### A aplicação reinicializa o identificador de sessão e suas variáveis associadas após uma mudança no nível de privilégio?
- [ ] Sim — a sessão é totalmente resetada e um novo ID **é emitido**  
- [ ] Parcial — um novo ID é emitido, mas algumas variáveis de sessão **persistem**  
- [ ] Não — o ID de sessão e todas as variáveis **permanecem** inalterados  

### Privilégios administrativos podem ser obtidos manipulando variáveis de sessão através de uma conta sem privilégios?
- [ ] Não — as variáveis de privilégio **não podem** ser modificadas pelos usuários  
- [ ] Sim — as variáveis podem ser modificadas, mas a aplicação as **valida** contra o banco de dados no backend  
- [ ] Sim — as variáveis podem ser modificadas e a aplicação **confia** no estado da sessão implicitamente *(Crítico)*  

### O estado da sessão é limpo imediatamente após a conclusão de um fluxo de trabalho específico (ex: redefinição de senha, checkout)?
- [ ] Sim — as variáveis de sessão para o fluxo de trabalho são destruídas após a conclusão  
- [ ] Não — as variáveis do fluxo de trabalho **persistem** e podem ser reutilizadas em outras áreas da aplicação

---

## WSTG-SESS-09 — Teste de Session Hijacking

O Session Hijacking ocorre quando um atacante captura, prevê ou fixa um identificador de sessão (SID) válido para obter acesso não autorizado à sessão ativa de um usuário. Esta vulnerabilidade geralmente se manifesta através de segurança insuficiente na camada de transporte, ausência de flags de segurança em cookies ou algoritmos de geração de SID previsíveis que permitem a um atacante contornar completamente a autenticação. Do ponto de vista de um atacante, a exploração bem-sucedida permite a personificação total da vítima sem a necessidade de suas credenciais, sendo frequentemente alcançada através de network sniffing, Cross-Site Scripting (XSS) ou ataques de Session Fixation.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Ferramentas:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Os identificadores de sessão estão protegidos durante o trânsito?
- [ ] Sim — A flag `Secure` está **habilitada** e o HSTS é estritamente aplicado  
- [ ] Sim — A flag `Secure` está **habilitada**, mas o HSTS está **desabilitado**  
- [ ] Não — A flag `Secure` **não é aplicada**, permitindo a interceptação do SID através de canais não criptografados *(Alta)*  

### O identificador de sessão está protegido contra acesso por scripts client-side?
- [ ] Sim — A flag `HttpOnly` é **aplicada** a todos os cookies relacionados à sessão  
- [ ] Não — A flag `HttpOnly` **não é aplicada**, permitindo a exfiltração do SID via XSS *(Crítica)*  

### A aplicação implementa Session Binding a atributos do cliente?
- [ ] Sim — A sessão está vinculada a atributos do cliente (IP/User-Agent) e a reutilização **não é possível**  
- [ ] Sim — O Session Binding está presente, mas o bypass **é possível** via header spoofing  
- [ ] Não — Não existe Session Binding; o SID **é válido** quando utilizado a partir de qualquer origem  

### O identificador de sessão é suficientemente aleatório para evitar a previsibilidade?
- [ ] Sim — O SID utiliza um gerador de números pseudo-aleatórios criptograficamente seguro (CSPRNG)  
- [ ] No — O comprimento do SID é insuficiente ou segue uma sequência/padrão **previsível**  

### As sessões simultâneas são gerenciadas para limitar a janela de ataque?
- [ ] Não — A aplicação impõe estritamente uma única sessão ativa por usuário  
- [ ] Sim — Múltiplas sessões simultâneas estão **habilitadas**, mas são monitoradas  
- [ ] Sim — Sessões simultâneas ilimitadas **são possíveis** em diferentes dispositivos/localizações

---

## WSTG-SESS-10 — Teste de JSON Web Tokens

JSON Web Tokens (JWTs) são comumente utilizados para gerenciamento de sessão stateless e propagação de identidade, mas sua segurança depende inteiramente da implementação correta de algoritmos de assinatura e da verificação rigorosa de claims. Atacantes frequentemente tentam burlar a autenticação explorando falhas no algoritmo "none", realizando Brute Force em chaves secretas HS256 fracas ou executando ataques de troca de algoritmo (algorithm-switching), onde uma chave pública assimétrica é utilizada como um segredo HMAC simétrico. Essas vulnerabilidades geralmente se manifestam em cookies de sessão ou cabeçalhos de Authorization, permitindo potencialmente que um atacante realize escalonamento de privilégios ou personifique qualquer usuário ao modificar claims como `role` ou `user_id` sem invalidar a integridade criptográfica do token.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Ferramentas:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### O algoritmo "none" é aceito pelo servidor?
- [ ] Não — o servidor **rejeita** tokens utilizando `alg: none` no cabeçalho  
- [ ] Sim — o servidor **aceita** tokens com `alg: none` e uma assinatura vazia *(Crítico)*  

### Como a assinatura do JWT é verificada e protegida contra Brute Force?
- [ ] Sim — são utilizadas chaves assimétricas fortes (RS256/ES256) ou segredos HMAC de alta entropia  
- [ ] Sim — o HS256 é utilizado, mas o segredo **pode** sofrer Brute Force devido à baixa entropia ou valores padrão  
- [ ] Não — a verificação de assinatura **não é aplicada** ou **pode** ser burlada via troca de algoritmo (RS256 para HS256)  

### As claims padrão (exp, nbf, iat) são aplicadas rigorosamente pelo backend?
- [ ] Sim — o `exp` (expiração) está presente e **não pode** ser burlado  
- [ ] Sim — o `exp` está presente, mas o servidor **não** o aplica durante a validação  
- [ ] Não — as claims de expiração estão **ausentes** ou são ignoradas, permitindo o Session Replay indefinido  

### A implementação impede a injeção de chaves baseada em cabeçalho (kid, jku, x5u)?
- [ ] Sim — os cabeçalhos são sanitizados e apenas chaves/fontes internas confiáveis são **permitidas**  
- [ ] Não — o cabeçalho `kid` (Key ID) está vulnerável a Directory Traversal ou SQL Injection para apontar para um arquivo conhecido  
- [ ] Não — os cabeçalhos `jku` ou `x5u` **podem** ser apontados para URLs controladas pelo atacante para carregar chaves maliciosas

---

## WSTG-SESS-11 — Teste de Sessões Simultâneas

O teste de sessões simultâneas avalia se uma aplicação permite que uma única conta de utilizador mantenha múltiplas sessões ativas simultâneas em diferentes navegadores, dispositivos ou endereços IP. A falha em restringir sessões simultâneas aumenta a janela de oportunidade para atacantes utilizarem tokens de sessão sequestrados (hijacked session tokens) ou credenciais comprometidas sem alertar o utilizador legítimo ou terminar as sessões existentes. Este comportamento é tipicamente avaliado através da autenticação a partir de múltiplas fontes e da monitorização da estabilidade da sessão, revelando frequentemente fragilidades na lógica de Session Management ou a ausência de regras de negócio focadas em segurança. Do ponto de vista de um atacante, a falta de controlo de concorrência permite a persistência furtiva após um comprometimento inicial e facilita ataques automatizados paralelos.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### As sessões simultâneas são limitadas pela política da aplicação?
- [ ] Sim — apenas uma sessão **é permitida** por utilizador; novos logins invalidam os antigos *(Mais seguro)*  
- [ ] Sim — um número fixo de sessões simultâneas **é imposto** e não pode ser excedido  
- [ ] Não — um número ilimitado de sessões simultâneas **é possível**  

### Como a aplicação responde quando o limite de sessões é atingido?
- [ ] A sessão ativa mais antiga **é automaticamente terminada** (mitigação de session fixation/hijacking)  
- [ ] O utilizador **é solicitado** a terminar as sessões existentes antes que a nova sessão seja autorizada  
- [ ] A nova tentativa de login **é bloqueada** até que ocorra um logout manual noutro dispositivo  
- [ ] Nenhuma ação é tomada — múltiplas sessões permanecem **ativadas** simultaneamente  

### A aplicação fornece uma interface de gestão de sessões para o utilizador?
- [ ] Sim — os utilizadores **podem** visualizar sessões ativas (IP, dispositivo, hora) e terminá-las remotamente  
- [ ] Sim — os utilizadores **podem** visualizar sessões ativas, mas **não podem** terminá-las remotamente  
- [ ] Não — os utilizadores **não podem** ver ou gerir outras sessões ativas associadas à sua conta  

### O utilizador é notificado quando é detetada atividade de login simultânea?
- [ ] Sim — um alerta (e-mail, SMS ou in-app) **é acionado** para logins de novos dispositivos ou localizações  
- [ ] Não — nenhuma notificação **é fornecida** quando sessões simultâneas são estabelecidas

---

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

## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Stored Cross Site Scripting (XSS), também conhecido como Persistent XSS, ocorre quando uma aplicação recebe dados de um usuário e os armazena em um banco de dados persistente, sistema de arquivos ou outro meio de armazenamento sem a devida validação ou codificação (encoding). Quando uma vítima subsequentemente navega para uma página que recupera e exibe esses dados não sanitizados, o script malicioso é executado dentro do contexto do seu navegador sob a origem da aplicação. Esta vulnerabilidade é particularmente perigosa porque não exige que a vítima clique em um link especialmente manipulado; qualquer usuário que visualize a página afetada torna-se um alvo, levando potencialmente ao sequestro de sessão (session hijacking), ações não autorizadas ou coleta de credenciais (credential harvesting). Atacantes normalmente visam seções de comentários, perfis de usuários, fóruns de mensagens e logs administrativos onde a entrada é consistentemente renderizada para outros usuários ou administradores.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Ferramentas:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Existem pontos de entrada que armazenam a entrada do usuário para exibição posterior a outros usuários?
- [ ] Não — a aplicação não armazena entrada controlável pelo usuário para renderização posterior  
- [ ] Sim — a entrada é armazenada (ex: perfis, comentários), mas **não** é renderizada em um contexto HTML  
- [ ] Sim — a entrada é armazenada e renderizada para outros usuários ou administradores  

### A codificação de saída (output encoding) ou sanitização é aplicada quando os dados armazenados são renderizados?
- [ ] Sim — a codificação de saída sensível ao contexto (context-aware output encoding) **é aplicada** e o bypass **não é possível** *(Mais seguro)*  
- [ ] Sim — a sanitização (ex: `DOMPurify`) **é aplicada**, mas o bypass **é possível** através de erros de configuração  
- [ ] Não — os dados são renderizados diretamente no DOM **sem** codificação ou sanitização *(Alta)*  

### A validação de entrada ou sanitização pode ser burlada (bypassed) usando payloads alternativos?
- [ ] Não — os filtros lidam com várias codificações, tags não padrão e manipuladores de eventos (event handlers) de forma segura  
- [ ] Sim — o bypass **é possível** usando codificação de entidades HTML ou tags específicas (ex: `<svg>`, `<img>`)  
- [ ] Sim — o bypass **é possível** via payloads poliglotas (polyglot payloads) ou XSS baseado em mutação (mXSS)  

### Uma Política de Segurança de Conteúdo (CSP) fornece uma camada secundária de defesa?
- [ ] Sim — uma CSP estrita está **habilitada** e impede a execução de scripts inline e fontes não autorizadas  
- [ ] Sim — uma CSP está **habilitada**, mas está mal configurada (ex: `unsafe-inline` ou `script-src` amplo)  
- [ ] Não — nenhuma CSP está **habilitada** para a aplicação  

### É possível alcançar "Stored XSS para RCE" ou outras cadeias de alto impacto (Blind XSS)?
- [ ] Não — o impacto é limitado ao contexto do navegador no lado do cliente (client-side)  
- [ ] Sim — o stored XSS **pode** ser usado para visar administradores (Blind XSS) em painéis internos  
- [ ] Sim — o stored XSS **pode** ser encadeado com outras vulnerabilidades (ex: CSRF, exfiltração de dados sensíveis)

---

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

## WSTG-INPV-04 — Testing for HTTP Parameter Pollution

A HTTP Parameter Pollution (HPP) ocorre quando uma aplicação recebe múltiplos parâmetros HTTP com o mesmo nome e os processa de maneira inconsistente ou insegura. Ao fornecer parâmetros duplicados, um atacante pode manipular a lógica interna da aplicação, potencialmente contornando Web Application Firewalls (WAF), filtros de validação de entrada ou mecanismos de controle de acesso. Esta vulnerabilidade é altamente dependente do servidor web ou framework da aplicação subjacente, que pode escolher a primeira, a última ou uma versão concatenada dos parâmetros repetidos. A exploração geralmente visa endpoints que passam parâmetros para APIs de back-end, consultas a bancos de dados ou mecanismos de redirecionamento de URL, resultando em impactos que variam de Cross-Site Scripting (XSS) até a exfiltração não autorizada de dados.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Como a aplicação lida com múltiplos parâmetros HTTP com o mesmo nome?
- [ ] Não — parâmetros duplicados são **rejeitados** ou ignorados pelo servidor  
- [ ] Sim — o servidor seleciona consistentemente apenas a **primeira** ou a **última** ocorrência  
- [ ] Sim — o servidor **concatena** valores, permitindo potencialmente injeções  

### A HPP pode ser utilizada para contornar controles de segurança, como um WAF ou filtro de entrada?
- [ ] Não — os controles de segurança inspecionam corretamente **todas** as ocorrências de um parâmetro  
- [ ] Sim — um WAF ou filtro inspeciona apenas a **primeira** ocorrência, permitindo que uma **segunda** ocorrência maliciosa passe  
- [ ] Sim — a concatenação permite que um atacante **divida** um Payload entre múltiplos parâmetros para evitar a detecção  

### A aplicação reflete parâmetros poluídos na resposta sem a devida sanitização?
- [ ] Não — os parâmetros são sanitizados ou codificados antes de serem refletidos na UI  
- [ ] Sim — a poluição resulta em conteúdo refletido, mas a sanitização **é aplicada**  
- [ ] Sim — a poluição resulta em conteúdo refletido e a sanitização **não é aplicada**, levando a XSS ou erros de lógica  

### Os parâmetros passados para chamadas de API internas ou sistemas de back-end são vulneráveis à poluição?
- [ ] Não — as requisições de back-end utilizam métodos de construção seguros e não dinâmicos  
- [ ] Sim — a HPP permite que um atacante **injete** ou **sobrescreva** parâmetros na estrutura da requisição de back-end  

### A aplicação apresenta comportamento diferente quando os parâmetros são poluídos em requisições GET versus POST?
- [ ] Não — o comportamento é consistente em todos os métodos HTTP  
- [ ] Sim — apenas parâmetros GET são vulneráveis à poluição  
- [ ] Sim — apenas parâmetros POST (corpo) são vulneráveis à poluição

---

## WSTG-INPV-05 — SQL Injection

O SQL Injection ocorre quando entradas fornecidas pelo usuário são incorporadas em consultas SQL sem a devida parametrização ou sanitização, permitindo que atacantes manipulem a lógica da consulta. A exploração bem-sucedida pode levar à exfiltração não autorizada de dados, bypass de autenticação, modificação de dados e, em alguns casos, execução remota de código (Remote Code Execution) por meio de recursos do banco de dados, como `xp_cmdshell` ou `LOAD_FILE()`. Esta vulnerabilidade afeta comumente formulários de login, funcionalidades de busca, parâmetros de API REST, cabeçalhos HTTP e qualquer endpoint que construa consultas SQL dinâmicas a partir de entradas controladas pelo usuário. Do ponto de vista do atacante, identificar um ponto de injeção é o principal portão de entrada para o comprometimento total do banco de dados e potencial movimentação lateral dentro da infraestrutura.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Ferramentas:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Os parâmetros controláveis pelo usuário são processados utilizando métodos seguros de acesso ao banco de dados?
- [ ] Sim — a aplicação utiliza ORM ou consultas parametrizadas exclusivamente e o bypass **não é possível**  
- [ ] Sim — consultas parametrizadas são utilizadas, mas o bypass **é possível** através de casos específicos (ex: cláusulas `ORDER BY`)  
- [ ] Não — a concatenação direta de strings é utilizada para construir consultas SQL **sem** parametrização  

### O mecanismo de autenticação pode ser contornado via SQL Injection?
- [ ] Não — os parâmetros de login **não** são vulneráveis a injeção  
- [ ] Sim — o bypass de autenticação **é possível** utilizando payloads baseados em tautologia (ex: `' OR 1=1 --`)  

### A exfiltração de dados é possível através de técnicas in-band ou out-of-band?
- [ ] Não — nenhuma exfiltração de dados foi identificada  
- [ ] Sim — exfiltração baseada em UNION ou baseada em erro **é possível**  
- [ ] Sim — exfiltração blind (Booleana ou baseada em tempo) **é possível**  
- [ ] Sim — exfiltração out-of-band (DNS/HTTP) **é possível**  

### As funções da aplicação apresentam vulnerabilidades de SQL Injection de segunda ordem?
- [ ] Não — os dados armazenados são manipulados de forma segura ao serem recuperados para consultas posteriores  
- [ ] Sim — a entrada do usuário é armazenada e posteriormente utilizada em uma consulta SQL insegura **sem** validação  

### Os privilégios da conta de serviço do banco de dados estão restritos ao mínimo necessário?
- [ ] Sim — a conta de serviço possui o **menor privilégio** (ex: limitada a tabelas/ações específicas)  
- [ ] Não — a conta de serviço possui privilégios elevados (ex: `DROP TABLE`, privilégios de `FILE`, ou `xp_cmdshell` **habilitado**)

---

## WSTG-INPV-06 — Teste de LDAP Injection

A LDAP Injection (Injeção de LDAP) ocorre quando uma aplicação incorpora dados fornecidos pelo usuário em filtros LDAP (Lightweight Directory Access Protocol) sem a devida sanitização ou escaping, permitindo que um atacante manipule a lógica da consulta. Ao injetar caracteres especiais, tais como asteriscos, parênteses e operadores lógicos, os atacantes podem modificar o filtro de busca para ignorar mecanismos de autenticação ou exfiltrar informações sensíveis do diretório, incluindo nomes de usuário, associações a grupos e atributos organizacionais. Esta vulnerabilidade manifesta-se tipicamente em funcionalidades como buscas em diretórios corporativos, portais de funcionários ou sistemas de Single Sign-On (SSO) que fazem interface com Active Directory ou OpenLDAP. Do ponto de vista de um atacante, a exploração bem-sucedida envolve frequentemente técnicas de blind (cegas) para enumerar a estrutura do diretório ou valores de atributos bit a bit quando a saída direta da consulta é suprimida pela aplicação.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Ferramentas:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### A aplicação utiliza LDAP para autenticação ou buscas em diretórios?
- [ ] Não — O LDAP **não é utilizado** na arquitetura da aplicação  
- [ ] Sim — O LDAP é utilizado e todas as entradas são estritamente sanitizadas ou parametrizadas  
- [ ] Sim — O LDAP é utilizado e a entrada do usuário é concatenada nos filtros **sem** o devido escaping  

### Os metacaracteres de LDAP são devidamente escapados ou filtrados na entrada do usuário?
- [ ] Sim — caracteres como `(`, `)`, `&`, `|`, `*` e `\` **não são permitidos** ou são corretamente escapados  
- [ ] Não — metacaracteres **podem** ser injetados para alterar a estrutura do filtro LDAP  

### O mecanismo de autenticação pode ser burlado via injeção?
- [ ] Não — a lógica de login **não é suscetível** a manipulação de consulta  
- [ ] Sim — a autenticação **pode** ser burlada utilizando injeção de OR lógico (`|`) ou wildcard (`*`) nos campos de nome de usuário ou senha  

### É possível realizar a exfiltração cega de dados (blind data exfiltration) do serviço de diretório?
- [ ] Não — os resultados de busca são limitados e não são observadas diferenças de tempo ou booleanas  
- [ ] Sim — atributos de diretório **podem** ser enumerados bit a bit via técnicas de Boolean-based Blind Injection  
- [ ] Sim — registros completos do diretório **são** refletidos na resposta da aplicação devido à manipulação da consulta  

### Componentes secundários da aplicação (ex: mailers, SSO) estão vulneráveis a injeção de atributos baseada em LDAP?
- [ ] Não — os atributos LDAP são manipulados de forma segura em todos os componentes  
- [ ] Sim — um atacante **pode** modificar seus próprios atributos (ex: e-mail, associação a grupos) via injeção para escalar privilégios ou redirecionar comunicações

---

## WSTG-INPV-07 — XML Injection

A XML Injection ocorre quando uma aplicação incorpora dados fornecidos pelo usuário em um documento ou stream XML sem a devida validação, sanitização ou escape de metacaracteres XML. Ao injetar tags XML específicas ou elementos estruturais, um atacante pode manipular a lógica do documento, contornar mecanismos de autenticação ou interferir na integridade dos dados processados pelo backend. Esta vulnerabilidade é comumente encontrada em web services baseados em SOAP, APIs REST que aceitam XML, asserções SAML e aplicações que geram arquivos de configuração XML ou relatórios dinamicamente. Do ponto de vista de um atacante, o objetivo é sair do contexto de dados pretendido para alterar a estrutura do documento, potencialmente levando à escalada de privilégios não autorizada ou à execução de lógica de negócios não pretendida.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Ferramentas:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### A aplicação aceita e processa entrada formatada em XML vinda do usuário?
- [ ] Não — a aplicação **não** processa entrada XML  
- [ ] Sim — a entrada XML é processada via APIs (SOAP/REST)  
- [ ] Sim — o XML é processado via uploads de arquivos (SVG, DOCX, etc.)  

### Os metacaracteres XML (ex: `<`, `>`, `&`, `'`, `"`) são devidamente escapados ou sanitizados?
- [ ] Sim — todos os metacaracteres são **devidamente** escapados ou sanitizados *(Mais seguro)*  
- [ ] Sim — algum filtro é aplicado, mas o bypass **é possível** utilizando encoding  
- [ ] Não — a entrada bruta é inserida diretamente em estruturas XML **sem** sanitização  

### A validação de esquema no lado do servidor (XSD/DTD) é aplicada para todas as entradas XML?
- [ ] Sim — a validação estrita de esquema está **ativada** e impede mudanças estruturais  
- [ ] Sim — a validação está **ativada**, mas **não** impede a injeção de tags dentro de campos de dados  
- [ ] Não — a validação de esquema está **desativada** ou não foi implementada  

### Um atacante consegue injetar novas tags XML para alterar a estrutura ou lógica do documento?
- [ ] Não — a estrutura permanece intacta independentemente da entrada  
- [ ] Sim — tags podem ser injetadas, mas **não podem** alterar a lógica de negócios  
- [ ] Sim — a injeção de tags **é possível** e permite o bypass de lógica ou modificação de dados *(Crítico)*  

### O parser XML pode ser manipulado utilizando seções CDATA ou comentários XML?
- [ ] Não — o parser trata corretamente ou rejeita marcadores de CDATA/comentários  
- [ ] Sim — a injeção de `<![CDATA[...]]>` ou `<!-- ... -->` **é possível** para ocultar ou burlar filtros

---

## WSTG-INPV-08 — Teste de SSI Injection

A Injeção de Server-Side Includes (SSI Injection) ocorre quando uma aplicação falha ao sanitizar a entrada do usuário antes de incorporá-la em arquivos HTML processados pelo motor de SSI do servidor. Atacantes exploram isso injetando diretivas SSI, tais como `<!--#exec cmd="id" -->`, para executar comandos arbitrários de shell, ler arquivos locais ou exfiltrar variáveis de ambiente. Esta vulnerabilidade é normalmente encontrada em aplicações que utilizam extensões de arquivo legadas como `.shtml`, `.shtm` ou `.stm`, ou onde o servidor web está explicitamente configurado para processar SSI dentro de arquivos HTML padrão. A exploração bem-sucedida concede ao atacante as mesmas permissões do processo do servidor web, frequentemente levando ao comprometimento total do sistema ou à exposição de dados sensíveis.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### O servidor web suporta e processa diretivas SSI?
- [ ] Não — o servidor **não suporta** SSI ou extensões de arquivo como `.shtml` estão **desativadas**  
- [ ] Sim — o SSI **está habilitado**, mas a entrada do usuário **não é** refletida nas páginas processadas  
- [ ] Sim — o SSI **está habilitado** e a entrada do usuário **é** refletida nas páginas processadas  

### Comandos arbitrários do sistema podem ser executados através da diretiva `#exec`?
- [ ] Não — a diretiva `#exec` está **desativada** ou restrita pela configuração do servidor  
- [ ] Sim — a execução de comandos **é possível** através de payloads `#exec cmd` ou `#exec cgi`  

### A exfiltração de arquivos sensíveis ou variáveis de ambiente é possível?
- [ ] Não — diretivas como `#include` ou `#config` estão **desativadas**  
- [ ] Sim — a leitura de arquivos locais (ex: `/etc/passwd`) **é possível** através de `#include virtual`  
- [ ] Sim — variáveis de ambiente do servidor **podem** ser exfiltradas via `#printenv` ou `#echo`  

### A validação de entrada ou controles de WAF são eficazes contra payloads de SSI?
- [ ] Sim — a validação rigorosa de entrada **é aplicada** e o bypass **não é possível**  
- [ ] Sim — a validação/WAF **é aplicada**, mas o bypass **é possível** utilizando codificação de caracteres ou sintaxe SSI diferente  
- [ ] Não — nenhuma validação ou proteção WAF está presente para caracteres de SSI (`<`, `!`, `#`, `-`, `"`)

---

## WSTG-INPV-09 — Testing for XPath Injection

A XPath Injection ocorre quando uma aplicação utiliza informações fornecidas pelo usuário para construir uma consulta XPath para dados XML, permitindo que um invasor interfira na lógica da consulta. Ao injetar sintaxes específicas, como aspas simples, colchetes e operadores lógicos, um invasor pode navegar pela estrutura do documento XML, burlar a autenticação ou exfiltrar nós de dados sensíveis. Esta vulnerabilidade é comumente encontrada em web services, interfaces de gerenciamento de configuração e sistemas legados que dependem de armazenamentos de dados baseados em XML em vez de bancos de dados SQL tradicionais. Do ponto de vista de um atacante, isso é frequentemente explorado usando técnicas de boolean-based ou error-based para mapear sistematicamente a árvore XML e recuperar valores ocultos do backend.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Ferramentas:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### As entradas do usuário utilizadas para construir consultas XPath são devidamente sanitizadas ou parametrizadas?
- [ ] Sim — as entradas **não** são usadas em consultas XPath ou são estritamente parametrizadas  
- [ ] Sim — uma validação de entrada/codificação robusta **é aplicada** e o bypass **não é possível**  
- [ ] Sim — alguma validação **é aplicada**, mas o bypass **é possível** utilizando caracteres codificados  
- [ ] Não — a entrada do usuário é diretamente concatenada em consultas XPath *(Crítico)*  

### A aplicação revela detalhes estruturais de XML/XPath por meio de mensagens de erro?
- [ ] Não — páginas de erro genéricas são exibidas e **nenhum** detalhe de XML é vazado  
- [ ] Sim — as mensagens de erro revelam a presença de processamento XML, mas **sem** detalhes do caminho (path)  
- [ ] Sim — mensagens de erro detalhadas revelam a sintaxe XPath, nomes de nós ou caminhos de arquivos  

### É possível burlar a lógica de autenticação ou autorização utilizando a lógica XPath?
- [ ] Não — a lógica de autenticação **não** depende de XML/XPath ou está implementada de forma segura  
- [ ] Sim — o login ou as verificações de permissão podem ser burlados utilizando payloads baseados em lógica como `' or 1=1 or '`  

### A estrutura do documento XML ou os dados podem ser enumerados via blind injection?
- [ ] Não — a aplicação **não** é suscetível à inferência baseada em booleanos ou baseada em tempo (time-based)  
- [ ] Sim — nomes de nós e dados **podem** ser exfiltrados usando inferência baseada em booleanos  
- [ ] Sim — a travessia total da árvore XML e a extração de dados **são possíveis**

---

## WSTG-INPV-10 — IMAP SMTP Injection

As vulnerabilidades de injeção IMAP e SMTP surgem quando uma aplicação web filtra incorretamente os dados fornecidos pelo utilizador antes de os incorporar em comandos enviados para um servidor de correio. Ao injetar caracteres de Carriage Return e Line Feed (CRLF), um atacante pode terminar o comando pretendido e anexar instruções de correio arbitrárias, tais como a adição de destinatários adicionais (CC/BCC) ou a modificação do corpo da mensagem. Estas falhas são frequentemente encontradas em gateways web-to-mail, formulários de contacto e clientes de e-mail que comunicam diretamente com servidores de correio através de protocolos como IMAP4 ou SMTP. A exploração bem-sucedida permite o relay não autorizado de spam, a exfiltração de conteúdos de e-mail sensíveis e, em alguns casos, o comprometimento total da integridade do servidor de correio.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Estado do Teste** | Não Realizado |
| **Gravidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### A aplicação processa entradas fornecidas pelo utilizador para construir comandos IMAP ou SMTP?
- [ ] Não — a aplicação **não** interage com servidores de correio através de inputs do utilizador  
- [ ] Sim — a aplicação interage com servidores de correio, mas utiliza uma API ou biblioteca segura  
- [ ] Sim — a aplicação constrói comandos de correio puros (raw) utilizando parâmetros controlados pelo utilizador  

### A entrada é validada para prevenir a injeção de sequências de Carriage Return (`\r`) e Line Feed (`\n`)?
- [ ] Sim — os caracteres CRLF são **estritamente** filtrados, codificados ou rejeitados  
- [ ] Sim — a entrada é validada contra uma allowlist rigorosa de caracteres  
- [ ] Não — os caracteres CRLF **não** são filtrados, permitindo a terminação e injeção de comandos  

### Pode um atacante injetar cabeçalhos de correio adicionais, tais como `Bcc:` ou `Cc:`?
- [ ] Não — a modificação de cabeçalhos **não é possível** devido a restrições rigorosas de input  
- [ ] Sim — a injeção de cabeçalhos adicionais **é possível** através de sequências CRLF  
- [ ] Sim — o relay de e-mail para domínios externos **está ativado** e é explorável *(Crítico)*  

### A aplicação permite a manipulação de comandos IMAP para aceder a caixas de correio não autorizadas?
- [ ] Não — a aplicação **não** utiliza IMAP ou limita estritamente o acesso à caixa de correio ao utilizador autenticado  
- [ ] Sim — a injeção de comandos IMAP **é possível**, mas limitada à enumeração de metadados  
- [ ] Sim — a injeção de comandos IMAP permite a leitura ou eliminação de e-mails de outros utilizadores  

### O servidor de correio de backend é vulnerável a pipelining de comandos?
- [ ] Não — o servidor de correio rejeita comandos em lote (batched) ou múltiplos comandos numa única sessão  
- [ ] Sim — o servidor de correio processa múltiplos comandos injetados através de um único campo de entrada

---

## WSTG-INPV-11 — Code Injection

A injeção de código (Code Injection) ocorre quando uma aplicação avalia trechos de código fornecidos pelo usuário dentro de seu ambiente de execução, geralmente por meio de funções inseguras específicas da linguagem, como `eval()`, `exec()` ou `system()`. Esta vulnerabilidade difere da injeção de comando (Command Injection) porque o alvo é o contexto de execução da linguagem de programação (ex: PHP, Python, Node.js) em vez do shell do sistema operacional subjacente. Atacantes exploram esses pontos para executar lógica arbitrária, exfiltrar variáveis de ambiente ou obter a execução remota de código (RCE) completa no servidor. Pentesters devem investigar funcionalidades que processam expressões matemáticas, templates do lado do servidor (Server-Side Templates) ou parâmetros de configuração dinâmica onde a entrada pode ser interpretada como código executável.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Alguma funcionalidade da aplicação avalia entradas dinâmicas por meio de funções de avaliação específicas da linguagem?
- [ ] Não — funções de avaliação dinâmica **não estão** presentes na base de código  
- [ ] Sim — funções como `eval()`, `exec()` ou `include()` são utilizadas, mas **não** aceitam entrada do usuário  
- [ ] Sim — as funções são utilizadas e processam entradas fornecidas pelo usuário  

### Mecanismos de validação ou sanitização de entrada são aplicados antes da avaliação?
- [ ] Sim — a entrada é rigorosamente validada contra uma **whitelist** estática  
- [ ] Sim — a entrada é sanitizada via blacklisting, mas o **bypass** é possível  
- [ ] Não — a entrada é passada diretamente para a função de avaliação e **nenhum controle** é aplicado  

### O ambiente de execução está isolado (sandboxed) ou restrito para impedir o acesso ao nível do sistema?
- [ ] Sim — a execução ocorre em um sandbox altamente restrito onde chamadas de sistema **não podem** ser feitas  
- [ ] Sim — existem algumas restrições, mas o **sandbox escape** é possível  
- [ ] Não — o ambiente permite acesso total às APIs da linguagem e funções do sistema operacional subjacentes *(Crítica)*  

### A injeção de código pode ser usada para exfiltrar informações sensíveis?
- [ ] Não — a execução é cega (blind) e nenhuma comunicação fora de banda (out-of-band) **é possível**  
- [ ] Sim — variáveis de ambiente ou arquivos locais **podem** ser exfiltrados via requisições HTTP/DNS  
- [ ] Sim — os resultados do código executado são **refletidos diretamente** na resposta da aplicação  

### A aplicação permite a inclusão remota de arquivos (Remote File Inclusion) ou o carregamento dinâmico de bibliotecas?
- [ ] Não — apenas caminhos de código locais e predefinidos podem ser executados  
- [ ] Sim — a aplicação **pode** ser forçada a carregar e executar código de uma fonte remota ou controlada pelo atacante

---

## WSTG-INPV-12 — Command Injection

A injeção de comando (Command Injection) ocorre quando uma aplicação passa dados inseguros fornecidos pelo usuário, como entradas de formulário, cabeçalhos HTTP ou cookies, para um shell do sistema. Esta vulnerabilidade permite que um atacante execute comandos arbitrários do sistema operacional no servidor, tipicamente com os privilégios do processo da aplicação web. Do ponto de vista de um atacante, isso é explorado utilizando metacaracteres de shell como ponto e vírgula, pipes ou crases (backticks) para encadear comandos maliciosos ao fluxo de execução pretendido. O impacto é frequentemente catastrófico, levando ao comprometimento total do sistema, exfiltração não autorizada de dados ou movimentação lateral dentro da rede interna.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Ferramentas:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### A aplicação invoca comandos de nível de sistema ou executáveis de shell?
- [ ] Não — a aplicação **não** interage com o shell do SO  
- [ ] Sim — a aplicação utiliza APIs internas ou bibliotecas de alto nível para tarefas do sistema  
- [ ] Sim — a aplicação invoca comandos de shell diretamente via chamadas de sistema como `system()`, `exec()` ou `popen()`  

### A entrada do usuário é validada ou sanitizada antes de ser passada para chamadas de sistema?
- [ ] Sim — lista de permissões (allow-listing) estrita e validação de entrada **são aplicadas**  
- [ ] Sim — sanitização ou escape (escaping) são utilizados, mas o bypass **é possível**  
- [ ] Não — a entrada do usuário é passada diretamente para o shell **sem** validação  

### Metacaracteres de shell podem ser usados para manipular o fluxo de execução de comandos?
- [ ] Não — os metacaracteres são corretamente filtrados ou escapados  
- [ ] Sim — a execução em banda (in-band), onde a saída do comando é refletida na resposta, **é possível**  
- [ ] Sim — a execução cega (blind), onde nenhuma saída é refletida, **é possível**  

### Exfiltração fora de banda (OOB) é possível a partir do host?
- [ ] Não — o servidor não possui tráfego de saída (egress) ou sinais OOB (DNS/HTTP) falham  
- [ ] Sim — a exfiltração de dados via sinais OOB baseados em DNS ou HTTP **é possível**  

### O processo da aplicação é executado com privilégios de sistema elevados?
- [ ] Não — a aplicação é executada com uma conta de serviço dedicada de baixo privilégio  
- [ ] Sim — a aplicação é executada como `root`, `Administrator` ou um usuário de alto privilégio *(Crítico)*

---

## WSTG-INPV-13 — Format String Injection

A injeção de string de formato (Format String Injection) ocorre quando uma aplicação web passa entrada controlada pelo usuário diretamente para o argumento de string de formato de uma função variádica, como a família `printf` da linguagem C ou wrappers de log de baixo nível. Embora seja menos comum em frameworks web modernos de código gerenciado, esta vulnerabilidade permanece crítica em aplicações CGI legadas, extensões nativas ou sistemas de log em casos específicos que interagem com bibliotecas de sistema de baixo nível. Atacantes exploram isso fornecendo especificadores de formato como `%x` ou `%p` para vazar endereços de memória sensíveis e dados da stack, ou o especificador `%n` para realizar escritas arbitrárias na memória. A exploração bem-sucedida geralmente resulta no comprometimento completo do sistema via Remote Code Execution (RCE) ou, no mínimo, em uma condição de Denial-of-Service (DoS) impactante através de falhas (crashes) nos processos.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### A aplicação processa entrada de usuário por meio de código nativo ou interfaces CGI legadas?
- [ ] Não — a aplicação é construída inteiramente em frameworks de código gerenciado (ex: JVM, CLR)  
- [ ] Sim — a aplicação utiliza JNI, extensões nativas C/C++, ou CGIs binários legados onde vulnerabilidades de format string **podem** existir  

### As entradas fornecidas pelo usuário são passadas como o argumento principal para funções sensíveis a formato?
- [ ] Não — as entradas são passadas como argumentos de dados, e as strings de formato são **estáticas** e **hardcoded**  
- [ ] Sim — a entrada do usuário é concatenada diretamente no argumento da string de formato, mas a sanitização **é aplicada**  
- [ ] Sim — a entrada do usuário é utilizada diretamente como a string de formato e nenhuma sanitização **é aplicada**  

### É possível vazar conteúdos da memória utilizando especificadores de formato?
- [ ] Não — a entrada é validada e especificadores como `%p`, `%x` ou `%s` **não são processados**  
- [ ] Sim — o fornecimento dos especificadores `%p` ou `%x` resulta na reflexão de endereços de memória da stack ou heap  
- [ ] Sim — dados sensíveis (ex: ponteiros, stack cookies) **podem** ser exfiltrados via especificadores de formato  

### A aplicação pode sofrer crash ou ser manipulada utilizando o especificador `%n`?
- [ ] Não — especificadores que permitem escritas na memória estão **desativados** ou bloqueados pelo compilador/ambiente  
- [ ] Sim — a aplicação sofre crash (DoS) quando `%s%s%s%s` ou strings similares são fornecidas  
- [ ] Sim — escritas arbitrárias na memória via `%n` são **possíveis**, levando potencialmente a um Remote Code Execution (RCE)  

### As proteções binárias modernas (ASLR, DEP, Stack Canaries) são contornáveis através desta injeção?
- [ ] Não — o ambiente é endurecido (hardened) e o vazamento de memória **não pode** ser usado para burlar as proteções  
- [ ] Sim — a injeção de format string **pode** ser usada para vazar endereços base, tornando o bypass de ASLR/DEP **possível**

---

## WSTG-INPV-14 — Teste de Vulnerabilidades Incubadas

Vulnerabilidades incubadas, também referidas como vulnerabilidades persistentes ou de segunda ordem, ocorrem quando um payload malicioso é armazenado pela aplicação em um estado "frio" (cold state) e, posteriormente, executado ou processado em um contexto diferente. Este padrão de ataque geralmente visa sistemas de back-end, consoles administrativos ou ferramentas de relatórios automatizados que recuperam dados de um banco de dados (database) ou sistema de arquivos (filesystem) compartilhado sem a revalidação adequada ou codificação de saída (output encoding). Os pentesters devem rastrear o ciclo de vida da entrada do usuário desde o ponto de entrada (a "incubadora") até cada local onde esses dados são eventualmente renderizados ou utilizados por outros componentes ou aplicações secundárias. A exploração bem-sucedida frequentemente leva a resultados de alto impacto, como o sequestro de sessão administrativa (administrative session hijacking) via stored XSS ou exfiltração de dados (data exfiltration) por meio de second-order SQL injection em módulos de relatórios internos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Ferramentas:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Existem endpoints onde a entrada controlada pelo usuário é armazenada para recuperação subsequente por outros usuários ou processos?
- [ ] Não — a aplicação **não** armazena a entrada do usuário para uso posterior  
- [ ] Sim — a entrada **é** armazenada em campos de banco de dados, arquivos de log ou configurações de sistema  

### A validação ou sanitização de entrada é aplicada antes que os dados sejam gravados na camada de armazenamento persistente?
- [ ] Sim — a validação rigorosa de lista de permissão (allow-list) **é aplicada** no ponto de entrada  
- [ ] Sim — a sanitização genérica **é aplicada**, mas o bypass **é possível** via codificação (encoding) ou caracteres não padronizados  
- [ ] Não — a entrada é armazenada em sua forma bruta e não validada  

### Os dados armazenados são devidamente codificados ou sanitizados quando são renderizados em interfaces administrativas ou secundárias?
- [ ] Sim — a codificação de saída sensível ao contexto (context-aware output encoding) **é aplicada** em todos os locais onde os dados são renderizados  
- [ ] Sim — a codificação **é aplicada** em alguns locais, mas está **ausente** em outros (ex: painel administrativo interno)  
- [ ] Não — os dados são renderizados sem qualquer codificação ou sanitização, permitindo a execução de payloads  

### Payloads armazenados no banco de dados podem afetar processos em lote (batch processes) de back-end ou sistemas de monitoramento fora de banda (out-of-band)?
- [ ] Não — os processos de back-end utilizam métodos de parsing seguros ou consultas parametrizadas (parameterized queries)  
- [ ] Sim — payloads armazenados **podem** desencadear interações na lógica de back-end (ex: CSV injection, command injection em logs)  
- [ ] Sim — payloads armazenados **podem** desencadear requisições fora de banda (OOB) para servidores controlados pelo atacante

---

## WSTG-INPV-15 — HTTP Splitting Smuggling

Vulnerabilidades de HTTP Splitting e Smuggling surgem de discrepâncias em como proxies de frontend e servidores de backend interpretam e processam os limites de requisições HTTP, especificamente em relação aos cabeçalhos `Content-Length` e `Transfer-Encoding`. Ao criar requisições ambíguas, um atacante pode fazer o "smuggling" de uma requisição oculta para o backend ou injetar sequências CRLF para dividir uma resposta, levando a cache poisoning, request hijacking ou bypass de controles de segurança. Essas falhas normalmente se manifestam em ambientes complexos que utilizam reverse proxies, load balancers ou CDNs que possuem uma lógica de parsing inconsistente. Do ponto de vista de um atacante, isso permite o redirecionamento do tráfego de usuários, roubo de tokens de sessão sensíveis e a execução de ações não autorizadas no contexto das sessões de outros usuários.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Ferramentas:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### O ambiente é suscetível a request smuggling via discrepâncias CL.TE ou TE.CL?
- [ ] Não — os servidores de frontend e backend manipulam os limites das requisições de forma consistente  
- [ ] Sim — existem discrepâncias, mas a exploração **não é possível** devido a mitigações na infraestrutura  
- [ ] Sim — o smuggling CL.TE ou TE.CL **é possível**, permitindo que requisições ocultas alcancem o backend *(Alta)*  
- [ ] Sim — TE.TE (codificação dupla/ofuscação) **é possível** para realizar bypass de filtros de frontend *(Crítica)*  

### O HTTP Response Splitting pode ser alcançado através de injeção de CRLF nos cabeçalhos?
- [ ] Não — a aplicação sanitiza adequadamente as sequências CRLF em todas as entradas de cabeçalho  
- [ ] Sim — sequências CRLF são refletidas nos cabeçalhos, mas a divisão de resposta **não é possível**  
- [ ] Sim — a injeção de CRLF **é possível**, permitindo a injeção de cabeçalhos ou cache poisoning  

### Controles de segurança (WAF/ACLs) sofrem bypass usando requisições em smuggling?
- [ ] Não — os controles de segurança aplicam-se tanto às requisições externas quanto às requisições em smuggling  
- [ ] Sim — requisições em smuggling **podem** realizar bypass de regras de WAF de frontend ou ACLs baseadas em IP  

### É possível realizar o hijack de sessões de outros usuários ou redirecionar o tráfego deles?
- [ ] Não — os fluxos de requisição/resposta estão isolados e não podem ser cruzados  
- [ ] Sim — o request smuggling **é possível** e permite a captura de requisições de outros usuários *(Crítica)*  
- [ ] Sim — o response splitting **é possível** e permite cache poisoning no lado do navegador ou XSS

---

## WSTG-INPV-16 — HTTP Request Smuggling

O HTTP Request Smuggling (HRS) ocorre quando uma cadeia de servidores HTTP (como um balanceador de carga e um servidor web de back-end) interpreta os limites de uma requisição de forma diferente, geralmente devido a cabeçalhos `Content-Length` e `Transfer-Encoding` conflitantes. Um atacante explora esta dessincronização para realizar o "smuggling" de uma entrada no buffer de requisições do servidor back-end, efetivamente prefixando a requisição do próximo usuário legítimo com um segmento controlado pelo atacante. Esta técnica permite ataques graves, incluindo Session Hijacking, bypass de controles de segurança e Cache Poisoning. A exploração é geralmente realizada por meio de análise baseada em tempo (timing-based analysis) ou pela observação de respostas inesperadas do back-end quando requisições subsequentes são enviadas ao servidor.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Crítica / Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Ferramentas:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Os servidores front-end e back-end tratam o cabeçalho `Transfer-Encoding: chunked` de forma consistente?
- [ ] Sim — ambos os servidores tratam o chunked encoding de forma idêntica e a dessincronização **não é possível**  
- [ ] Não — os servidores interpretam os cabeçalhos de forma diferente, mas a sanitização/normalização **é aplicada**  
- [ ] Não — a dessincronização CL.TE (Content-Length/Transfer-Encoding) **é possível** *(Crítica)*  
- [ ] Não — a dessincronização TE.CL (Transfer-Encoding/Content-Length) **é possível** *(Crítica)*  

### O atacante pode envenenar o cache do servidor ou do lado do cliente (Cache Poisoning) usando uma requisição "smuggled"?
- [ ] Não — o caching está **desativado** ou não é suscetível a envenenamento  
- [ ] Sim — requisições "smuggled" **podem** redirecionar usuários para domínios maliciosos ou injetar scripts via Cache Poisoning  

### É possível capturar requisições de outros usuários ou tokens de sessão (Session Tokens) via concatenação de requisições?
- [ ] Não — o smuggling **não** permite a exposição de dados entre usuários  
- [ ] Sim — o smuggling permite anexar a requisição do próximo usuário a um corpo `POST` controlado pelo atacante, tornando a exfiltração **possível**  

### A infraestrutura utiliza HTTP/2 e é suscetível à dessincronização H2.CL ou H2.TE?
- [ ] Não — a aplicação utiliza apenas HTTP/1.1 ou o HTTP/2 está implementado de forma segura sem downgrade  
- [ ] Sim — ocorrem downgrades de HTTP/2 para HTTP/1.1 em texto simples (cleartext) e a dessincronização **é possível**  

### Cabeçalhos malformados (ex: `Transfer-Encoding:  chunked` com um espaço) são tratados de forma segura?
- [ ] Sim — cabeçalhos malformados são rejeitados ou normalizados em todas as camadas  
- [ ] Não — a dessincronização TE.TE **é possível** devido ao tratamento inconsistente de cabeçalhos malformados ou obscurecidos

---

## WSTG-INPV-17 — Teste de Host Header Injection

A Host Header Injection ocorre quando uma aplicação confia no cabeçalho `Host` fornecido pelo usuário e o incorpora na lógica do lado do servidor, como na geração de links ou redirecionamentos, sem validação suficiente. Atacantes manipulam este cabeçalho para facilitar o envenenamento de cache web (Web Cache Poisoning), facilitar o envenenamento de redefinição de senha ao redirecionar tokens sensíveis para um domínio externo, ou burlar controles de segurança que dependem de roteamento baseado em cabeçalhos. A exploração bem-sucedida permite que um atacante realize a exfiltração de dados de sessão, execute o comprometimento de contas (Account Takeover) ou redirecione usuários para uma infraestrutura maliciosa. Esta vulnerabilidade é comumente encontrada em ambientes com balanceamento de carga ou aplicações que geram dinamicamente URLs absolutas baseadas no contexto da requisição recebida.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Médio / Alto* |

> *A severidade torna-se Alta se o cabeçalho Host for utilizado para gerar links sensíveis em e-mails, como redefinições de senha.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Ferramentas:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### A aplicação reflete o valor do cabeçalho `Host` em links, redirecionamentos ou scripts?
- [ ] Não — a aplicação utiliza URLs base estáticas (hardcoded) ou configurações estritamente validadas  
- [ ] Sim — o cabeçalho `Host` é refletido, mas validado rigorosamente contra uma whitelist  
- [ ] Sim — o cabeçalho `Host` é refletido e o bypass **é possível** através de valores malformados (ex: `expected.com:attacker.com`)  
- [ ] Sim — o cabeçalho `Host` é refletido **sem** qualquer validação  

### Cabeçalhos alternativos relacionados ao host são processados pela aplicação ou infraestrutura?
- [ ] Não — cabeçalhos como `X-Forwarded-Host`, `X-Host` ou `Forwarded` são ignorados  
- [ ] Sim — cabeçalhos alternativos são processados, mas **não** sobrescrevem a lógica interna  
- [ ] Sim — cabeçalhos alternativos **podem** ser usados para sobrescrever o valor do cabeçalho `Host` principal  

### O cabeçalho `Host` pode ser manipulado para envenenar e-mails gerados pelo sistema (ex: redefinições de senha)?
- [ ] Não — os links de e-mail utilizam um domínio estático e confiável da configuração do servidor  
- [ ] Sim — o cabeçalho `Host` influencia os links de e-mail, mas a validação **não** pode ser burlada  
- [ ] Sim — o cabeçalho `Host` **pode** ser manipulado para redirecionar tokens de redefinição de senha para um domínio controlado pelo atacante *(Crítico)*  

### A aplicação é suscetível a Web Cache Poisoning via cabeçalho `Host`?
- [ ] Não — não há mecanismo de cache presente ou o cabeçalho `Host` faz parte da chave de cache (cache key)  
- [ ] Sim — um cache está presente, mas ele valida rigorosamente ou ignora o cabeçalho `Host` para entradas não indexadas (unkeyed inputs)  
- [ ] Sim — um cache existe e **pode** ser envenenado por um cabeçalho `Host` injetado para servir conteúdo malicioso a outros usuários  

### O servidor aceita cabeçalhos `Host` arbitrários para roteamento de hosts virtuais (vhosts)?
- [ ] Não — o servidor retorna um erro ou uma página padrão para cabeçalhos `Host` não reconhecidos  
- [ ] Sim — o servidor aceita qualquer cabeçalho `Host`, o que **é** útil para reconhecimento (reconnaissance) ou enumeração de serviços internos

---

## WSTG-INPV-18 — Testing for Server-Side Template Injection

O Server-Side Template Injection (SSTI) ocorre quando uma aplicação incorpora entradas controladas pelo usuário em um mecanismo de template no lado do servidor (server-side template engine) sem a devida sanitização ou escape. Isso permite que um atacante injete diretivas de template maliciosas, que são então executadas no servidor, levando potencialmente ao comprometimento total do sistema através de Remote Code Execution (RCE). O SSTI normalmente se manifesta em funcionalidades como geração dinâmica de e-mails, páginas de perfil e sistemas de notificação que utilizam mecanismos como Jinja2, Twig ou FreeMarker. Do ponto de vista de um atacante, essa vulnerabilidade é frequentemente explorada para vazar variáveis de ambiente sensíveis, ler arquivos internos ou executar comandos do sistema aproveitando-se de objetos e métodos nativos do mecanismo de template.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Ferramentas:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### A aplicação utiliza um mecanismo de template no lado do servidor para processar a entrada do usuário?
- [ ] Não — a aplicação **não** utiliza templates no lado do servidor para conteúdo dinâmico  
- [ ] Sim — os templates são utilizados, mas a entrada do usuário **não** é incorporada em diretivas de template  
- [ ] Sim — a entrada do usuário é incorporada em templates **sem** a sanitização adequada  

### Expressões aritméticas básicas são avaliadas quando injetadas em campos de entrada?
- [ ] Não — payloads como `{{7*7}}` ou `${7*7}` são refletidos como strings literais  
- [ ] Sim — as expressões são avaliadas (ex: retornando `49`), confirmando que o mecanismo de template **é vulnerável**  

### É possível acessar os objetos ou métodos integrados (built-in) do mecanismo de template?
- [ ] Não — o acesso a objetos, métodos ou configurações perigosas é **restrito** ou isolado (sandboxed)  
- [ ] Sim — objetos integrados (ex: `self`, `config`, `__class__`) **podem** ser acessados para vazar dados sensíveis  
- [ ] Sim — a execução remota de código (RCE) via chamadas de sistema ou bibliotecas do SO **é possível**  

### Existe um mecanismo de sandbox ou lista de permissões (allowlist) que impeça a execução de diretivas perigosas?
- [ ] Sim — uma allowlist rigorosa ou sandbox seguro está em vigor e o bypass **não é possível**  
- [ ] Sim — o sandbox está presente, mas o bypass **é possível** através de payloads específicos dependentes do mecanismo  
- [ ] Não — nenhuma sandbox ou allowlist **é aplicada** ao mecanismo de template

---

## WSTG-INPV-19 — Teste para Server-Side Request Forgery (SSRF)

O Server-Side Request Forgery (SSRF) ocorre quando um atacante consegue influenciar uma aplicação web a realizar requisições não autorizadas a partir do servidor para recursos internos ou externos. Ao manipular parâmetros que esperam URLs, hostnames ou endereços IP, um atacante pode burlar controles de nível de rede, como firewalls e ACLs, para sondar segmentos de rede interna, acessar serviços de metadados de nuvem (IMDS) ou interagir com APIs e bancos de dados internos vulneráveis. Esta vulnerabilidade geralmente se manifesta em funcionalidades que envolvem a busca de imagens, geração de PDF, webhooks ou uploads de arquivos via URL. Do ponto de vista de um atacante, o SSRF é uma primitiva poderosa utilizada para realizar pivô em ambientes isolados, exfiltrar dados de configuração sensíveis ou, potencialmente, obter execução remota de código (Remote Code Execution - RCE) através da interação com serviços internos como Redis ou Memcached.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Ferramentas:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### A aplicação processa URLs ou hostnames fornecidos pelo usuário?
- [ ] Não — nenhuma funcionalidade aceita URLs ou hostnames externos  
- [ ] Sim — a funcionalidade existe, mas a entrada **é estritamente validada** contra uma allow-list  
- [ ] Sim — a funcionalidade existe e aceita URLs fornecidas pelo usuário  

### Controles de validação ou blacklists são aplicados ao destino solicitado?
- [ ] Sim — uma **allow-list** estrita de domínios/IPs confiáveis é aplicada  
- [ ] Sim — uma **deny-list** é utilizada (ex: bloqueando `127.0.0.1`, `169.254.169.254`)  
- [ ] Não — **nenhuma validação** ou filtragem é aplicada à entrada  

### É possível burlar os filtros através de encoding, redirecionamentos ou DNS rebinding?
- [ ] Não — os filtros são robustos e tratam redirecionamentos/resolução DNS de forma segura  
- [ ] Sim — o bypass **é possível** usando URL encoding ou formatos de IP alternativos (hex, octal)  
- [ ] Sim — o bypass **é possível** através de redirecionamentos HTTP 3xx para alvos internos  
- [ ] Sim — o bypass **é possível** através de ataques de DNS rebinding para resolver IPs internos  

### A aplicação consegue alcançar serviços de metadados internos ou interfaces locais?
- [ ] Não — rede local e endpoints de metadados de nuvem **não estão acessíveis**  
- [ ] Sim — o acesso ao localhost (`127.0.0.1`) ou serviços de loopback **é possível**  
- [ ] Sim — o acesso a serviços de metadados de nuvem (ex: AWS/GCP/Azure IMDS) **é possível** *(Crítica)*  

### Qual é a natureza da resposta do SSRF?
- [ ] Blind — nenhuma resposta ou metadado é retornado ao usuário  
- [ ] Parcial — metadados da resposta (headers, tamanho, tempo de resposta) são retornados ao usuário  
- [ ] Total — o corpo completo da resposta do recurso interno **é refletido** para o usuário

---

## WSTG-INPV-20 — Mass Assignment

Mass Assignment, também conhecido como Overposting ou Insecure Deserialization de parâmetros, ocorre quando uma aplicação web vincula automaticamente parâmetros de requisições HTTP recebidas a propriedades de objetos internos sem a filtragem adequada de atributos. Esta vulnerabilidade é prevalente em frameworks MVC modernos e APIs RESTful onde dados JSON, XML ou codificados em formulários são desserializados diretamente em modelos de dados do lado da aplicação. Atacantes exploram esse comportamento injetando parâmetros adicionais não listados em uma requisição — como `isAdmin`, `role` ou `account_balance` — para modificar campos sensíveis que os desenvolvedores não pretendiam expor ao controle do usuário. A exploração bem-sucedida normalmente resulta em escalonamento de privilégios (privilege escalation), modificação não autorizada de dados ou o bypass de fluxos de lógica de negócio críticos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Média* |

> *A severidade torna-se Alta se atributos administrativos, níveis de permissão ou campos de faturamento puderem ser modificados.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### A aplicação utiliza vinculação automática (automatic binding) para os parâmetros de requisição recebidos?
- [ ] Não — os parâmetros são mapeados manualmente para variáveis específicas ou objetos seguros  
- [ ] Sim — a vinculação automática é usada, mas **white-lists** rigorosas ou DTOs (Data Transfer Objects) são aplicados  
- [ ] Sim — a vinculação automática é usada e atributos internos sensíveis **podem** ser modificados  

### Atributos administrativos ou relacionados a permissões podem ser injetados com sucesso?
- [ ] Não — parâmetros injetados são ignorados, removidos ou causam um erro de validação  
- [ ] Sim — parâmetros extras são aceitos, mas **não** afetam o estado do objeto no backend  
- [ ] Sim — injetar parâmetros como `is_admin`, `role` ou `permissions` escala privilégios **com sucesso** *(Crítico)*  

### É possível modificar a lógica de negócio sensível ou campos financeiros via overposting?
- [ ] Não — campos como `price`, `balance` ou `status` estão protegidos contra mass assignment  
- [ ] Sim — atributos de negócio sensíveis **podem** ser modificados ao adicioná-los ao corpo da requisição JSON ou formulário  

### A aplicação revela estruturas de objetos internos que facilitam a adivinhação de parâmetros?
- [ ] Não — as respostas incluem apenas campos públicos pretendidos e a documentação é restrita  
- [ ] Sim — campos de objetos internos são visíveis em respostas GET, tornando a descoberta de parâmetros **possível**  
- [ ] Sim — a documentação da API (Swagger/OpenAPI) lista explicitamente propriedades internas que **podem** ser atribuídas

---

## WSTG-ERRH-01 — Teste de Manipulação Inadequada de Erros

A manipulação inadequada de erros ocorre quando uma aplicação web divulga detalhes técnicos sensíveis — como stack traces, informações de esquema de banco de dados ou caminhos de arquivos internos — por meio de suas respostas de erro. Atacantes provocam esses erros enviando entradas malformadas, acessando recursos inexistentes ou induzindo exceções no lado do servidor para mapear a arquitetura subjacente da aplicação e identificar possíveis vetores para exploração posterior. Esse vazamento de informações frequentemente serve como um precursor para ataques mais graves, incluindo SQL Injection ou Path Traversal, ao fornecer ao atacante especificações precisas do ambiente. Uma implementação segura deve fornecer mensagens genéricas amigáveis ao usuário, capturando diagnósticos detalhados apenas em logs seguros no lado do servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### A aplicação utiliza um manipulador de erros global e genérico para exceções não tratadas?
- [ ] Sim — páginas de erro genéricas estão **habilitadas** e não revelam **nenhum** detalhe técnico  
- [ ] Sim — páginas de erro personalizadas são utilizadas, mas o vazamento **é possível** por meio de cabeçalhos específicos  
- [ ] Não — páginas de erro padrão do servidor (ex: Tomcat, IIS, Nginx) estão **visíveis**  

### É possível enumerar informações técnicas por meio de entradas malformadas ou casos de borda?
- [ ] Não — os erros são manipulados de forma adequada com IDs de referência únicos para registro (logging)  
- [ ] Sim — stack traces ou consultas de banco de dados **são expostos** nas respostas  
- [ ] Sim — caminhos de arquivos internos, variáveis de ambiente ou strings de versão do servidor **são expostos**  

### As respostas de API estão vazando objetos de erro detalhados ou informações de depuração?
- [ ] Não — as APIs retornam códigos de erro padronizados e mensagens JSON sanitizadas  
- [ ] Sim — as APIs retornam objetos de depuração detalhados ou detalhes completos da exceção no corpo da resposta  
- [ ] Sim — stack traces estão incluídos nos campos `details` ou `exception` da resposta JSON  

### A aplicação se comporta de forma diferente (Baseada em tempo ou Baseada em conteúdo) quando ocorrem erros?
- [ ] Não — as assinaturas de resposta e os tempos (timings) são consistentes, independentemente do tipo de erro  
- [ ] Sim — respostas diferenciadas **podem** ser usadas para enumerar estados válidos vs. inválidos (ex: Enumeração de Usuários)

---

## WSTG-ERRH-02 — Teste de Stack Traces

Stack traces são gerados quando uma aplicação falha ao tratar uma exceção de forma adequada, resultando na exposição do estado de execução interno e da hierarquia de chamadas para o usuário final. Para um profissional de testes de invasão, esses rastros são inestimáveis, pois revelam o framework da aplicação, versões de bibliotecas, caminhos internos do sistema de arquivos e o fluxo lógico, o que reduz significativamente o esforço necessário para o reconhecimento (reconnaissance). A exploração (exploitation) envolve o acionamento intencional de erros por meio de entradas malformadas, tipos de dados inesperados ou violações de condições de contorno para forçar a aplicação a um estado instável e exfiltrar informações sobre a pilha tecnológica subjacente. Esse vazamento geralmente fornece o contexto necessário para identificar componentes de terceiros vulneráveis ou para criar exploits específicos para falhas lógicas descobertas nos caminhos de código expostos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média* |

> *A severidade aumenta para Média se o stack trace revelar detalhes arquiteturais sensíveis, consultas de banco de dados ou parâmetros de configuração internos.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Stack traces completos são retornados ao cliente em caso de erros na aplicação?
- [ ] Não — a aplicação retorna páginas de erro genéricas ou mensagens de erro personalizadas  
- [ ] Sim — stack traces estão **habilitados**, mas apenas para módulos específicos e não sensíveis  
- [ ] Sim — stack traces completos estão **habilitados** e visíveis no corpo da resposta HTTP  

### As mensagens de erro expõem informações sensíveis do ambiente?
- [ ] Não — as mensagens de erro não contêm detalhes técnicos ou ambientais  
- [ ] Sim — as mensagens revelam **caminhos de arquivos internos** ou **estruturas de diretórios** do servidor  
- [ ] Sim — as mensagens revelam **schemas de banco de dados**, **consultas SQL** ou **versões de bibliotecas de terceiros**  

### Os stack traces podem ser acionados manipulando parâmetros de entrada?
- [ ] Não — entradas malformadas são tratadas adequadamente ou rejeitadas pela validação  
- [ ] Sim — os rastros podem ser acionados por **tipos de dados inesperados** (ex: enviar um array onde uma string é esperada)  
- [ ] Sim — os rastros são acionados por **null bytes**, **caracteres especiais** ou **valores de contorno**  

### Um manipulador de erros global é aplicado de forma consistente em toda a aplicação?
- [ ] Sim — um manipulador de exceções global (global exception handler) está **sendo aplicado consistentemente** em todos os endpoints testados  
- [ ] Sim — um manipulador está presente, mas **não é aplicado** a endpoints legados, API ou recém-adicionados  
- [ ] Não — nenhum mecanismo de tratamento de erros global foi **detectado**, resultando em erros padrão do servidor

---

## WSTG-CRYP-01 — Teste de Segurança Fraca na Camada de Transporte

O teste de segurança fraca na camada de transporte (Transport Layer Security) envolve a análise da implementação e configuração do SSL/TLS para garantir a confidencialidade e integridade dos dados durante o trânsito. Atacantes visam falhas de configuração, como protocolos obsoletos (SSLv2, TLS 1.0), Cipher Suites fracos (NULL, RC4 ou anônimos) e certificados inválidos ou com assinaturas fracas para realizar ataques de Man-in-the-Middle (MitM). Este teste geralmente foca nos endpoints do servidor web ou balanceadores de carga da aplicação onde a comunicação criptografada é estabelecida. A exploração bem-sucedida permite que um adversário intercepte dados sensíveis, realize o sequestro de sessões (Session Hijacking) ou force o Downgrade de conexões para estados inseguros através de Protocol Downgrade ou descriptografia de tráfego.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Média* |

> *A Severidade é Alta se dados sensíveis (PII, credenciais, session tokens) forem transmitidos; Média para tráfego informacional geral.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Protocolos TLS/SSL obsoletos ou inseguros são suportados?
- [ ] Não — apenas TLS 1.2 e TLS 1.3 estão **habilitados**  
- [ ] Sim — TLS 1.0 ou TLS 1.1 estão **habilitados**  
- [ ] Sim — SSLv2 ou SSLv3 estão **habilitados** *(Crítico)*  

### Cipher Suites fracos ou inseguros são aceitos pelo servidor?
- [ ] Não — apenas ciphers fortes e modernos (ex: AES-GCM, ChaCha20) são **suportados**  
- [ ] Sim — ciphers com criptografia fraca (ex: 3DES, RC4) ou chaves curtas são **suportados**  
- [ ] Sim — ciphers NULL, anônimos (ADH) ou de exportação são **suportados** *(Crítico)*  

### O certificado do servidor é válido e confiável?
- [ ] Sim — o certificado é válido, corresponde ao domínio e é assinado por uma **CA confiável**  
- [ ] Não — o certificado está **expirado** ou utiliza um **algoritmo de assinatura fraco** (ex: SHA-1)  
- [ ] Não — o certificado é **autoassinado (self-signed)** ou ocorre divergência no nome do domínio (**mismatch**)  

### O servidor suporta Secure Renegotiation e previne ataques de Downgrade?
- [ ] Sim — Secure Renegotiation é **suportado** e o TLS Fallback SCSV está **habilitado**  
- [ ] Não — Secure Renegotiation **não é suportado**, permitindo potencialmente injeção via MitM  
- [ ] Não — O servidor está vulnerável a ataques **POODLE** ou **ROBOT**  

### O HTTP Strict Transport Security (HSTS) está implementado corretamente?

| Cabeçalho | Presente | Ausente |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Sim — o cabeçalho HSTS está presente com um `max-age` longo e `includeSubDomains` **habilitado**  
- [ ] Sim — o cabeçalho HSTS está presente, mas falta o `includeSubDomains` ou possui um **max-age curto**  
- [ ] Não — o cabeçalho HSTS **não está presente**

---

## WSTG-CRYP-02 — Testando Padding Oracle

Vulnerabilidades de Padding Oracle ocorrem quando o processo de decodificação de uma aplicação vaza informações sobre se o preenchimento (padding) de um texto cifrado (ciphertext) decifrado é válido ou inválido. Ao observar essas respostas de canal lateral (side-channel)—como códigos de status HTTP distintos, mensagens de erro específicas ou variações no tempo de resposta—um invasor pode decifrar dados sem conhecer a chave de criptografia e, potencialmente, re-criptografar texto simples (plaintext) arbitrário. Esta vulnerabilidade geralmente se manifesta em cookies de sessão criptografados, tokens de autenticação ou parâmetros de URL que utilizam cifras de bloco no modo Cipher Block Chaining (CBC) com preenchimento PKCS#7. A exploração permite a perda total da confidencialidade e pode levar ao comprometimento da integridade através da construção de blocos criptografados forjados.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica* |

> *A severidade é Crítica se a vulnerabilidade residir no gerenciamento de sessão, tokens de identidade ou criptografia de PII (Informações de Identificação Pessoal).

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Uma cifra de bloco é usada no modo CBC para dados sensíveis?
- [ ] Não — a aplicação utiliza criptografia autenticada (AES-GCM/ChaCha20-Poly1305)  
- [ ] Sim — a aplicação utiliza cifras de bloco, mas o preenchimento (padding) **não** é necessário (ex: cifras de fluxo ou modo CTR)  
- [ ] Sim — a aplicação utiliza o modo CBC e o preenchimento (padding) **é aplicado**  

### O servidor revela a validade do preenchimento através de canais laterais?
- [ ] Não — o servidor retorna mensagens de erro uniformes e tempos de resposta consistentes  
- [ ] Sim — o servidor retorna diferentes códigos de status HTTP (ex: 200 vs 500) para erros de preenchimento  
- [ ] Sim — o servidor retorna mensagens de erro específicas (ex: "Invalid Padding", "Decryption failed")  
- [ ] Sim — o servidor apresenta diferenças de tempo mensuráveis durante falhas de decodificação  

### A decodificação automatizada do texto cifrado é possível?
- [ ] Não — os canais laterais **não** são estáveis o suficiente para exploração  
- [ ] Sim — o texto cifrado (ciphertext) **pode** ser parcialmente decifrado (últimos blocos)  
- [ ] Sim — a recuperação total do texto simples (plaintext) **é possível** utilizando ferramentas como `PadBuster`  

### O invasor pode realizar bit-flipping ou forjar textos cifrados válidos?
- [ ] Não — as verificações de integridade (HMAC) são validadas **antes** da decodificação  
- [ ] Sim — não há verificação de integridade presente, mas a construção do payload **não** é viável  
- [ ] Sim — a construção de texto cifrado arbitrário **é possível**, permitindo escalonamento de privilégios ou injeção de dados

---

## WSTG-CRYP-03 — Teste para Informações Sensíveis Enviadas via Canais Não Criptografados

A transmissão de dados sensíveis por canais não criptografados, como HTTP simples, expõe as informações à interceptação e modificação por meio de ataques Man-in-the-Middle (MITM). Esta vulnerabilidade ocorre tipicamente quando as aplicações web falham em aplicar o Transport Layer Security (TLS) em todos os endpoints, particularmente em componentes legados, formulários de login ou durante a transição inicial de HTTP para HTTPS. Do ponto de vista de um atacante, isso permite o sniffing passivo de credenciais de autenticação, tokens de sessão e informações de identificação pessoal (PII) em segmentos de rede comprometidos ou não confiáveis, como Wi-Fi público. Garantir que todos os dados sensíveis sejam encapsulados em um túnel TLS robusto é fundamental para manter a confidencialidade e integridade das interações do usuário e prevenir o session hijacking.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Média |

> *A severidade torna-se Alta se credenciais de autenticação, identificadores de sessão ou PII forem transmitidos em texto claro (cleartext).

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### A aplicação permite comunicação via HTTP não criptografado para endpoints sensíveis?
- [ ] Não — todo o tráfego da aplicação é estritamente forçado via HTTPS *(Mais seguro)*  
- [ ] Sim — páginas não sensíveis são acessíveis via HTTP, mas endpoints sensíveis **não são**  
- [ ] Sim — endpoints sensíveis (login, checkout, perfil) **estão** acessíveis via HTTP não criptografado *(Alto Risco)*  

### Existe um redirecionamento global de HTTP para HTTPS?
- [ ] Sim — a aplicação realiza um redirecionamento 301/302 para HTTPS para todas as requisições HTTP  
- [ ] Sim — o redirecionamento é realizado apenas para endpoints sensíveis específicos  
- [ ] No — nenhum redirecionamento de HTTP para HTTPS **é aplicado**  

### O HTTP Strict Transport Security (HSTS) está implementado?
- [ ] Sim — o HSTS está **habilitado** com um `max-age` longo e inclui as diretivas `includeSubDomains` e `preload`  
- [ ] Sim — o HSTS está **habilitado**, mas carece das diretivas `includeSubDomains` ou `preload`  
- [ ] Não — o HSTS **não está habilitado** no servidor da aplicação  

### Os cookies sensíveis estão protegidos contra transmissão em texto claro?

| Nome do Cookie | Flag Secure Presente | Flag Secure Ausente |
|----------------|:--------------------:|:-------------------:|
| `SessionID`    |                      |                     |
| `AuthToken`    |                      |                     |
| `CSRF-Token`   |                      |                     |

### A aplicação transmite dados sensíveis para APIs de terceiros via canais não criptografados?
- [ ] Não — todas as chamadas de API externas são realizadas através de túneis criptografados (HTTPS)  
- [ ] Sim — algumas telemetrias não sensíveis são enviadas via HTTP  
- [ ] Sim — API keys, PII ou credenciais **são** enviadas para terceiros via HTTP não criptografado

---

## WSTG-CRYP-04 — Teste de Primitivas Criptográficas Fracas

Primitivas criptográficas fracas envolvem o uso de algoritmos obsoletos ou inseguros, como MD5, SHA-1, DES ou RC4, que são suscetíveis a ataques de colisão, análise de frequência ou descriptografia por força bruta (Brute Force). Essas fraquezas ocorrem tipicamente em hashing de senhas, criptografia de dados em repouso, geração de tokens de sessão ou verificação de assinatura digital no backend da aplicação. Do ponto de vista de um atacante, a identificação dessas primitivas permite o comprometimento da integridade e confidencialidade dos dados, como a quebra (cracking) de hashes interceptados para recuperar senhas em texto claro ou a forja de tokens de autenticação. Aplicações modernas devem, em vez disso, utilizar primitivas padrão da indústria, resistentes a colisões e de alta entropia, como Argon2, Bcrypt ou AES-GCM, para garantir uma proteção robusta contra criptoanálise.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Algoritmos de hashing depreciados, como MD5 ou SHA-1, são utilizados para o armazenamento de dados sensíveis?
- [ ] Não — a aplicação utiliza algoritmos modernos resistentes a colisões, como Argon2 ou Bcrypt  
- [ ] Sim — algoritmos depreciados são utilizados, mas **apenas** para verificações de integridade não sensíveis à segurança  
- [ ] Sim — algoritmos depreciados **são utilizados** para senhas ou tokens sensíveis *(Alta)*  

### Cifras de criptografia simétrica fracas, como DES, 3DES ou RC4, estão sendo empregadas no momento?
- [ ] Não — é utilizado AES (128 bits ou superior) em um modo seguro, como GCM ou CBC  
- [ ] Sim — cifras legadas estão **habilitadas** para compatibilidade reversa, mas **não** são o padrão  
- [ ] Sim — cifras legadas são o método principal para proteger dados em repouso ou em trânsito  

### A aplicação implementa lógica criptográfica "própria" (roll-your-own) ou não padronizada?
- [ ] Não — a aplicação adere estritamente a bibliotecas criptográficas padrão e revisadas por pares  
- [ ] Sim — existem wrappers personalizados, mas as primitivas subjacentes **são** padrão da indústria  
- [ ] Sim — algoritmos criptográficos proprietários ou lógica personalizada **são aplicados** a dados sensíveis *(Crítica)*  

### Salts criptográficos e vetores de inicialização (IVs) são gerenciados de acordo com as melhores práticas?
- [ ] Sim — os salts são exclusivos por usuário e os IVs são imprevisíveis para cada operação  
- [ ] Sim — salts são utilizados, mas **não** são exclusivos em todos os registros  
- [ ] Não — os salts são estáticos, hardcoded ou ausentes, e os IVs **são reutilizados** em múltiplas sessões  

### O comprimento de bits das chaves assimétricas (RSA, Diffie-Hellman) é suficiente para os padrões modernos?
- [ ] Sim — as chaves RSA/DH possuem 2048 bits ou mais, ou Curva Elíptica (ECC) é utilizada  
- [ ] Não — as chaves possuem menos de 2048 bits, mas o bypass **não** é trivial  
- [ ] Não — as chaves possuem 1024 bits ou menos, tornando-as **vulneráveis** a fatoração ou ataques computacionais

---

## WSTG-BUSL-01 — Teste de Validação de Dados da Lógica de Negócio

O teste de validação de dados da lógica de negócio foca na capacidade da aplicação de identificar e rejeitar dados que são sintaticamente corretos, mas logicamente inválidos dentro do contexto do domínio de negócio. Atacantes exploram essas falhas enviando valores inesperados — como quantidades negativas em um carrinho de compras, datas no passado para eventos futuros ou inteiros extremamente grandes — para contornar restrições e manipular o estado da aplicação. Essas vulnerabilidades residem frequentemente em workflows de múltiplas etapas, processos de checkout e módulos de gerenciamento de contas, onde a lógica do lado do servidor (server-side logic) falha em verificar a integridade contextual dos dados fornecidos pelo usuário. A exploração bem-sucedida pode levar a perdas financeiras, comprometimento da integridade e acesso não autorizado a recursos ou dados restritos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### A aplicação impõe limites de intervalo lógico em entradas numéricas?
- [ ] Sim — a aplicação rejeita corretamente valores negativos, zero ou excessivamente grandes onde for inapropriado *(Mais seguro)*  
- [ ] Sim — a aplicação rejeita alguns intervalos inválidos, mas **é** vulnerável a integer overflow ou boundary bypass  
- [ ] Não — a aplicação aceita qualquer valor numérico, independentemente do contexto de negócio *(Crítico)*  

### As verificações de validação do lado do servidor são consistentes em todas as camadas da aplicação?
- [ ] Sim — a validação é aplicada consistentemente tanto nas camadas de API/serviço quanto no banco de dados  
- [ ] Sim — a validação **é aplicada** na UI/Frontend, mas o bypass via requisição direta à API **é possível**  
- [ ] Não — a validação **não é aplicada** ou é inconsistente, permitindo a corrupção lógica de dados  

### A lógica de negócio pode ser subvertida pela manipulação de dados em workflows de múltiplas etapas?
- [ ] Não — o estado é mantido de forma segura no servidor e os dados de etapas anteriores **não podem** ser adulterados  
- [ ] Sim — a validação ocorre nas etapas iniciais, mas a submissão final **não** verifica novamente a integridade dos dados  
- [ ] Sim — o bypass de workflow **é possível** ao pular etapas ou enviar dados fora de ordem  

### A aplicação lida adequadamente com dados de "casos extremos" (edge cases) que contornam as restrições de negócio?
- [ ] Sim — as restrições são robustas e lidam com entradas incomuns, mas sintaticamente válidas (ex: anos bissextos, mudanças de fuso horário)  
- [ ] Sim — alguns casos extremos são tratados, mas combinações específicas **podem** levar a comportamentos inesperados  
- [ ] Não — casos extremos levam diretamente a falhas de lógica, como compras gratuitas ou alterações de status não autorizadas

---

## WSTG-BUSL-02 — Testar a Capacidade de Forjar Requisições

Forjar requisições dentro de um contexto de lógica de negócio envolve a manipulação de parâmetros definidos pela aplicação para realizar ações fora do fluxo de trabalho pretendido ou do nível de autorização. Atacantes visam processos de várias etapas, como sistemas de checkout ou registro de contas, onde o estado é mantido via parâmetros no lado do cliente (client-side) que o servidor falha em revalidar contra uma fonte de backend confiável. Ao alterar campos ocultos, valores JSON ou parâmetros REST, como preços, quantidades ou flags de status interno, um atacante pode burlar requisitos de pagamento, realizar escalada de privilégios ou acionar mudanças de estado não pretendidas. Esta vulnerabilidade normalmente se manifesta em formulários web ou APIs com estado (stateful), onde o servidor confia implicitamente na representação do estado de negócio feita pelo cliente durante uma transação.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica* |

> *A severidade torna-se Crítica se a requisição forjada permitir perda financeira, ações administrativas não autorizadas ou o comprometimento total da conta (full account takeover).

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### A aplicação valida os parâmetros transacionais contra uma "fonte da verdade" no lado do servidor (server-side)?
- [ ] Sim — todos os parâmetros sensíveis (preço, cargo/role, ID) são validados contra o banco de dados e o bypass **não é possível**  
- [ ] Sim — a validação é aplicada, mas pode ser burlada via parameter pollution ou codificações alternativas  
- [ ] Não — a aplicação confia nos valores do lado do cliente para determinar o custo ou o resultado de uma transação *(Crítica)*  

### Valores críticos para o negócio (ex: quantidades, montantes) podem ser modificados para valores não autorizados?
- [ ] Não — valores negativos, valores zero ou valores excessivamente altos são rejeitados pelo servidor  
- [ ] Sim — o servidor aceita valores modificados, mas apenas dentro de uma faixa limitada  
- [ ] Sim — valores negativos ou zero **podem** ser enviados, levando a prejuízos financeiros ou bypass de lógica  

### Campos ocultos ou parâmetros de API são utilizados para manter o estado em processos de múltiplas etapas?
- [ ] Não — o estado é mantido de forma segura via sessões no servidor ou tokens assinados  
- [ ] Sim — o estado é passado via cliente, mas verificações de integridade (HMAC/Assinaturas) estão **habilitadas** e são robustas  
- [ ] Sim — o estado é passado via cliente e as verificações de integridade estão **desabilitadas** ou podem ser removidas  

### É possível forjar requisições que pulam etapas intermediárias obrigatórias em um fluxo de trabalho?
- [ ] Não — o servidor impõe uma sequência estrita de operações para cada transação  
- [ ] Sim — as etapas **podem** ser puladas, mas a ação final ainda requer dados válidos de etapas anteriores  
- [ ] Sim — a requisição final de "enviar" (submit) ou "concluir" **pode** ser forjada diretamente para burlar etapas de autorização ou pagamento

---

## WSTG-BUSL-03 — Teste de Verificações de Integridade

O teste de verificação de integridade foca-se em verificar se a aplicação garante que os dados permanecem inalterados e confiáveis à medida que se movem através de vários processos de negócio ou entre o cliente e o servidor. Os atacantes visam esta lógica através da manipulação de parâmetros críticos — tais como preços de produtos, quantidades, privilégios de utilizador ou estados de transação — dentro de pedidos HTTP, campos ocultos ou cookies. Se a aplicação carecer de verificação no lado do servidor (server-side) ou de controlos de integridade robustos como Hashing ou HMACs, um adversário pode manipular estes valores para cometer fraude, escalar a sua própria autoridade ou contornar as restrições de negócio pretendidas. Este teste é vital para qualquer aplicação que processe transações financeiras, transições de estado sensíveis ou fluxos de trabalho de várias etapas, onde a saída de uma fase serve como a entrada confiável para a próxima.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Estado do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a falha de integridade permitir o roubo financeiro, escalada de privilégios não autorizada ou modificação de registos persistentes.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Os parâmetros de negócio sensíveis estão expostos à manipulação no lado do cliente (client-side tampering)?
- [ ] Não — os valores sensíveis são geridos exclusivamente no lado do servidor  
- [ ] Sim — valores como preço, quantidade ou sinalizadores de estado (status flags) **estão** expostos, mas encriptados  
- [ ] Sim — os valores **estão** expostos em texto simples (plain text) dentro de campos ocultos, parâmetros de URL ou cookies  

### É aplicado um mecanismo de integridade (ex: HMAC, Checksum) aos parâmetros controlados pelo cliente?
- [ ] Sim — uma verificação de integridade robusta **é aplicada** e verificada em cada pedido (request)  
- [ ] Sim — uma verificação de integridade **é aplicada**, mas utiliza um algoritmo fraco/previsível ou um segredo codificado rigidamente (hardcoded)  
- [ ] Não — não **são aplicadas** verificações de integridade a parâmetros sensíveis  

### A verificação de integridade pode ser contornada (bypassed) ou recalculada por um atacante?
- [ ] Não — a verificação de assinatura é estritamente aplicada e o bypass **não é possível**  
- [ ] Sim — a verificação de assinatura pode ser contornada ao omitir o parâmetro de assinatura  
- [ ] Sim — a assinatura **pode** ser recalculada porque a chave secreta ou a lógica está exposta/é fraca  

### A aplicação realiza a revalidação de backend dos dados contra uma fonte confiável?
- [ ] Sim — o servidor revalida todos os valores contra a base de dados e o bypass **não é possível**  
- [ ] Sim — o servidor realiza validação parcial, mas o bypass **é possível** através de casos extremos (edge cases) específicos  
- [ ] Não — o servidor confia nos dados fornecidos pelo cliente sem verificação de backend *(Crítico)*  

### É possível manipular o estado de um processo de múltiplas etapas (multi-step process)?
- [ ] Não — o estado da sessão (session state) é gerido no lado do servidor e não pode ser manipulado  
- [ ] Sim — a informação de estado é passada via cliente e a manipulação (tampering) **é possível** para saltar etapas ou repetir ações

---

## WSTG-BUSL-04 — Test for Process Timing

Ataques de tempo de processamento (*process timing attacks*) envolvem a medição do tempo que uma aplicação leva para processar solicitações específicas para inferir informações sensíveis sobre estados internos ou a existência de dados. Ao analisar discrepâncias de milissegundos nos tempos de resposta — frequentemente causadas por lógica condicional de saída antecipada (*early-exit*), consultas ao banco de dados ou comparações criptográficas de tempo não constante — atacantes podem realizar análise de canal lateral (*side-channel analysis*) para enumerar nomes de usuário válidos, verificar fragmentos de senhas ou confirmar a existência de registros específicos. Essas vulnerabilidades ocorrem tipicamente em *authentication endpoints*, formulários de redefinição de senha e filtros de API que consomem muitos recursos, onde a lógica de *backend* se ramifica com base na validade da entrada. Da perspectiva de um atacante, essas diferenças de tempo servem como um "oráculo booleano" (*boolean oracle*) que revela verdades sobre os dados do *backend*, mesmo quando a aplicação retorna mensagens de erro genéricas e idênticas ao usuário.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Médio |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Ferramentas:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### A aplicação apresenta tempos de resposta variáveis para solicitações de recursos válidos vs. inválidos?
- [ ] Não — os tempos de resposta são idênticos, independentemente da validade da entrada  
- [ ] Sim — existem diferenças de tempo insignificantes, mas **não** são estatisticamente significativas  
- [ ] Sim — diferenças de tempo consistentes são **observáveis** e fornecem um oráculo confiável  

### Funções de comparação de tempo constante ou atrasos artificiais foram implementados para normalizar os tempos de resposta?
- [ ] Sim — algoritmos de tempo constante são **aplicados** a todas as operações de comparação sensíveis *(Mais seguro)*  
- [ ] Sim — atrasos artificiais ou *jitter* estão **habilitados**, mas a lógica subjacente permanece de tempo não constante  
- [ ] Não — nenhuma mitigação de tempo foi **aplicada**, permitindo a medição direta de ramos lógicos  

### Um atacante pode usar análise estatística para isolar o sinal de tempo do ruído de rede?
- [ ] Não — o *network jitter* e o ruído da aplicação tornam a análise de tempo **impossível**  
- [ ] Sim — com um tamanho de amostra suficiente, o sinal de tempo **pode** ser isolado do ruído  
- [ ] Sim — o delta de tempo é grande o suficiente para que a normalização estatística **não** seja necessária  

### A enumeração de contas é possível através da funcionalidade de login ou redefinição de senha?
- [ ] Não — a existência da conta não pode ser determinada através do tempo  
- [ ] Sim — as discrepâncias de tempo permitem que um atacante remoto **enumere** nomes de usuário ou e-mails válidos  

### Operações intensivas em recursos (ex: processamento de arquivos, buscas complexas) vazam a existência de dados via tempo?
- [ ] Não — o consumo de recursos é uniforme, independentemente de um registro ser encontrado ou não  
- [ ] Sim — a existência de registros sensíveis **pode** ser inferida medindo a duração do processamento no *backend*

---

## WSTG-BUSL-05 — Teste de Limites do Número de Vezes que uma Função Pode Ser Utilizada

Este teste avalia se uma aplicação impõe restrições de lógica de negócio sobre a frequência e o número total de vezes que uma ação ou função específica pode ser executada por um único usuário, sessão ou endereço IP. A falha na implementação desses limites permite que atacantes abusem de funções de alto valor — como o envio de SMS OTPs, disparo de e-mails de redefinição de senha, aplicação de códigos de desconto ou execução de exportações de dados pesadas — resultando em perdas financeiras, exaustão de recursos ou danos à reputação. Essas vulnerabilidades são normalmente encontradas em endpoints transacionais ou funcionalidades de comunicação e são exploradas através de scripts automatizados que repetem a ação muito além dos limites de negócio pretendidos. Do ponto de vista do atacante, este é um alvo principal para SMS pumping, mail bombing ou workflows de Brute Force (Força Bruta) que carecem de Rate Limiting (Limitação de Taxa) explícito ou controles baseados em contagem.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Média / Alta* |

> *A gravidade torna-se Alta se a função envolver transações financeiras, custos de SMS ou impactar a disponibilidade global do sistema.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### A aplicação define e impõe limites em funções de negócio sensíveis?
- [ ] Sim — os limites são rigorosamente aplicados e o bypass **não é possível**  
- [ ] Sim — os limites são aplicados, mas são excessivamente altos para prevenir abusos  
- [ ] Não — nenhum limite é aplicado à execução da função  

### Os limites de taxa (Rate Limiting) e contadores de uso são aplicados no lado do servidor (Server-side)?
- [ ] Sim — a imposição é estritamente no server-side e **não pode** ser manipulada  
- [ ] Não — a imposição depende de lógica no client-side (ex: JavaScript ou campos ocultos)  
- [ ] Não — não existe imposição em nenhum nível  

### Os limites de uso podem ser contornados através de manipulação de cabeçalhos (headers) ou de sessão?
- [ ] Não — bypass via `X-Forwarded-For`, `Origin` ou rotação de sessão **não é possível**  
- [ ] Sim — os limites **podem** ser contornados através de spoofing de cabeçalhos de IP ou limpeza de cookies  
- [ ] Sim — os limites **podem** ser contornados através da criação de múltiplas contas ou sessões  

### Exceder o limite aciona uma resposta defensiva apropriada?
- [ ] Sim — a aplicação retorna `429 Too Many Requests` e registra (logs) o evento *(Mais seguro)*  
- [ ] Sim — a aplicação retorna um erro, mas **não** registra o potencial abuso  
- [ ] Não — a aplicação continua a processar requisições, mas ignora a saída  
- [ ] Não — a aplicação não fornece resposta ou falha (crashes) sob carga  

### O impacto do abuso da função é significativo para o negócio?
- [ ] Não — o abuso da função tem custo ou impacto de recursos insignificantes  
- [ ] Sim — o abuso **pode** levar ao consumo menor de recursos ou incômodo ao usuário  
- [ ] Sim — o abuso **pode** levar a custos financeiros significativos (ex: taxas de SMS) ou negação de serviço *(Crítico)*

---

## WSTG-BUSL-06 — Teste para a Circunvenção de Fluxos de Trabalho

A circunvenção de fluxos de trabalho envolve a manipulação da lógica de uma aplicação para ignorar sequências pretendidas ou pré-requisitos dentro de processos de múltiplas etapas. Atacantes identificam endpoints sensíveis — como confirmações de pagamento, aprovações administrativas ou telas de conclusão de MFA — e tentam acessá-los diretamente, pulando etapas intermediárias obrigatórias. Esta vulnerabilidade ocorre quando a lógica do lado do servidor falha em manter uma máquina de estados robusta, confiando, em vez disso, na suposição de que o usuário segue o caminho orientado pela UI. A exploração bem-sucedida pode levar a falhas críticas de lógica de negócio, incluindo transações não autorizadas, escalonamento de privilégios e a burlagem de mecanismos de verificação de identidade.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a circunvenção permitir transações financeiras não autorizadas, escalonamento de privilégios ou acesso administrativo.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### A aplicação contém processos de negócio em múltiplas etapas?
- [ ] Não — a aplicação consiste apenas em ações de requisição única  
- [ ] Sim — processos de múltiplas etapas (ex: carrinhos de compras, formulários assistidos/wizards, registro) **estão** presentes  

### A sequência de etapas é estritamente imposta pelo servidor?
- [ ] Sim — o rastreamento de estado no lado do servidor garante que as etapas **não possam** ser ignoradas  
- [ ] Sim — controles no lado do cliente sugerem uma sequência, mas o servidor **não** valida o progresso  
- [ ] Não — a aplicação **não** impõe nenhuma ordem específica de operações  

### Um atacante pode alcançar a etapa final de um processo solicitando diretamente a URL ou endpoint terminal?
- [ ] Não — o acesso direto às etapas terminais **não é possível** sem a conclusão prévia  
- [ ] Sim — etapas terminais (ex: `/api/v1/checkout/complete`) **podem** ser alcançadas por requisição direta  

### A aplicação verifica se os dados pré-requisitos das etapas anteriores estão presentes e são válidos?
- [ ] Sim — o servidor valida todos os dados das etapas anteriores antes de prosseguir  
- [ ] Não — o servidor **é** vulnerável a "pular" etapas onde os dados são esperados, mas não verificados  

### Controles de segurança como MFA ou verificação de identidade podem ser burlados através da manipulação do fluxo de trabalho?
- [ ] Não — controles críticos **não podem** ser contornados via manipulação de fluxo  
- [ ] Sim — a burlagem de MFA ou verificações de identidade **é possível** ao saltar para a etapa pós-verificação

---

## WSTG-BUSL-07 — Testar Defesas Contra o Uso Indevido da Aplicação

As defesas contra o uso indevido da aplicação avaliam a capacidade da aplicação de identificar, registrar e responder a comportamentos anômalos de usuários que se desviam da lógica de negócios pretendida. Ao contrário da segurança tradicional baseada em assinaturas, essas defesas focam na detecção de padrões como requisições em ráfaga (rapid-fire), Credential Stuffing ou enumeração sequencial de recursos que sinalizam abuso automatizado. Do ponto de vista de um atacante, este teste determina a resiliência da aplicação contra automação e ataques "low and slow" projetados para exfiltrar dados ou esgotar recursos sem disparar alertas de segurança padrão. A falha na implementação dessas defesas geralmente resulta em Data Scraping massivo, Account Takeovers ou Denial of Service (DoS) por meio de funcionalidades legítimas, porém intensivas em recursos.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Mecanismos de rate-limiting ou throttling são aplicados em endpoints sensíveis?
- [ ] Sim — Rate Limiting rigoroso **é aplicado** e forçado em todos os endpoints sensíveis *(Mais seguro)*  
- [ ] Sim — Rate Limiting **é aplicado**, mas pode ser burlado via rotação de IP ou manipulação de cabeçalhos  
- [ ] Sim — Rate Limiting **é aplicado** apenas aos endpoints de autenticação, deixando outros expostos  
- [ ] Não — nenhum Rate Limiting ou Throttling **é aplicado** a qualquer funcionalidade da aplicação  

### A aplicação detecta e bloqueia padrões de interação automatizada?
- [ ] Sim — análise comportamental ou CAPTCHAs **estão habilitados** e bloqueiam bots de forma eficaz  
- [ ] Sim — anti-automação **está habilitado**, mas o bypass **é possível** usando navegadores headless ou ciclo de sessões  
- [ ] Não — a aplicação **não consegue** distinguir entre um usuário humano e um script automatizado  

### O comportamento anômalo é registrado e seguido por uma resposta defensiva?
- [ ] Sim — o uso indevido dispara bloqueio automatizado e alerta a equipe de segurança  
- [ ] Sim — o uso indevido é registrado para auditoria, mas **nenhuma** ação defensiva automatizada é tomada  
- [ ] Não — a aplicação **não registra** nem responde a padrões de atividade incomuns  

### As defesas podem ser burladas manipulando identificadores ou cabeçalhos do lado do cliente?
- [ ] Não — as defesas dependem do estado do lado do servidor e de telemetria verificada  
- [ ] Sim — os controles **podem** ser burlados limpando cookies ou rotacionando strings de `User-Agent`  
- [ ] Sim — os controles **podem** ser burlados injetando cabeçalhos como `X-Forwarded-For` ou `X-Real-IP`

---

## WSTG-BUSL-08 — Teste de Upload de Tipos de Arquivos Inesperados

O upload de tipos de arquivos inesperados permite que atacantes ignorem restrições de lógica de negócios ao enviar arquivos que a aplicação não pretende processar, potencialmente levando a Remote Code Execution, Cross-Site Scripting (XSS) ou Denial of Service. Os atacantes testam essas vulnerabilidades modificando extensões de arquivos, MIME types e magic bytes para enganar a lógica de validação do lado do servidor (server-side) e fazê-la aceitar conteúdo malicioso. Isso ocorre tipicamente em uploads de fotos de perfil, sistemas de gerenciamento de documentos ou anexos de tickets de suporte onde existe validação insuficiente no back-end. A exploração bem-sucedida pode comprometer o servidor hospedeiro se o arquivo enviado for executado ou levar a ataques do lado do cliente (client-side) se o arquivo for servido a outros usuários com um Content-Type incorreto.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### A aplicação restringe o upload de arquivos a um conjunto específico de extensões?
- [ ] Sim — uma whitelist estrita de extensões é aplicada e o bypass **não é possível**  
- [ ] Sim — uma whitelist de extensões está presente, mas o bypass **é possível** via extensões duplas ou null bytes  
- [ ] Não — qualquer extensão de arquivo **pode** ser carregada  

### O conteúdo do arquivo é validado além da extensão do arquivo?
- [ ] Sim — a lógica server-side verifica Magic Bytes e realiza inspeção profunda de conteúdo  
- [ ] Sim — a lógica server-side verifica o MIME type apenas através do cabeçalho `Content-Type`  
- [ ] Não — nenhuma validação de conteúdo **é aplicada** após a verificação da extensão  

### Um atacante pode executar código enviando um web shell ou script?
- [ ] Não — os arquivos enviados são armazenados em um diretório não executável ou em um bucket de armazenamento (S3/Azure Blob)  
- [ ] Sim — os arquivos são armazenados em um diretório acessível pela web, mas a execução **está desativada** via configuração do servidor  
- [ ] Sim — scripts maliciosos enviados **podem** ser executados diretamente através de um caminho de URL previsível *(Crítica)*  

### Ataques client-side como XSS são possíveis através de tipos de arquivos enviados?
- [ ] Não — os arquivos são servidos com `Content-Disposition: attachment` e `X-Content-Type-Options: nosniff`  
- [ ] Sim — arquivos SVG ou HTML **podem** ser enviados e renderizados no navegador, levando a XSS  
- [ ] Sim — metadados de imagem (EXIF) **são processados** e refletidos na UI sem sanitização

---

## WSTG-BUSL-09 — Testar Upload de Arquivos Maliciosos

A funcionalidade de upload de arquivos permite que os usuários enviem dados para o servidor, fornecendo um vetor direto para a introdução de conteúdo malicioso no ambiente da aplicação. Atacantes exploram a lógica de validação fraca para realizar o upload de web shells, malware ou arquivos especialmente criados — como SVG ou HTML — para obter Remote Code Execution (RCE) ou Cross-Site Scripting (XSS). Esta vulnerabilidade é tipicamente encontrada em recursos como configurações de perfil, sistemas de gerenciamento de documentos e manipuladores de anexos, onde o servidor falha em verificar adequadamente os tipos de arquivos, conteúdos e permissões de execução. A exploração bem-sucedida geralmente leva ao comprometimento total do sistema, exfiltração de dados ou ao uso do servidor como um ponto de pivô para ataques na rede interna.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### A aplicação fornece funcionalidade para upload de arquivos?
- [ ] Não — não existe funcionalidade de upload de arquivos  
- [ ] Sim — a funcionalidade existe para usuários autenticados  
- [ ] Sim — a funcionalidade está disponível para usuários **não autenticados** *(Crítico)*  

### A validação da extensão do arquivo é aplicada no lado do servidor (server-side)?
- [ ] Sim — uma allowlist rigorosa de extensões é **aplicada** e o bypass **não é possível**  
- [ ] Sim — uma allowlist é usada, mas o bypass **é possível** via case sensitivity ou extensões duplas  
- [ ] Sim — uma blocklist é usada, a qual **pode** ser burlada com extensões alternativas (ex: `.phtml`, `.asa`)  
- [ ] Não — nenhuma validação de extensão é **aplicada** no lado do servidor  

### O conteúdo do arquivo é validado além da extensão ou do MIME type?
- [ ] Sim — a aplicação verifica magic bytes e realiza inspeção profunda de conteúdo  
- [ ] Sim — a aplicação apenas verifica o cabeçalho `Content-Type`, o qual **é** facilmente forjado (spoofed)  
- [ ] Não — a aplicação depende inteiramente da extensão do arquivo para validação  

### Os arquivos carregados são armazenados em um diretório com permissões de execução?
- [ ] Não — os arquivos são armazenados em um serviço de armazenamento dedicado (ex: S3) ou em um diretório não executável  
- [ ] Sim — os arquivos são armazenados no servidor web, mas a execução está **desativada** via configuração  
- [ ] Sim — os arquivos são armazenados em um diretório acessível via web e a execução **está ativada** *(Crítico)*  

### Os filtros de segurança podem ser burlados via manipulação do nome do arquivo?
- [ ] Não — os nomes dos arquivos são sanitizados ou regenerados aleatoriamente pelo servidor  
- [ ] Sim — os filtros **podem** ser burlados usando null bytes (ex: `shell.php%00.jpg`)  
- [ ] Sim — caracteres de path traversal (ex: `../../`) **podem** ser usados para fazer upload de arquivos para diretórios arbitrários

---

## WSTG-BUSL-10 — Testar a Funcionalidade de Pagamento

O teste da funcionalidade de pagamento envolve a identificação de falhas de lógica de negócio que permitem a um atacante contornar gateways de pagamento, manipular totais de pedidos ou falsificar sinais de transação bem-sucedida. Essas vulnerabilidades geralmente residem na comunicação entre a aplicação, o navegador do cliente e processadores de pagamento de terceiros, como Stripe, PayPal ou sistemas de contabilidade internos. Um atacante pode tentar modificar parâmetros de preço em trânsito, enviar quantidades negativas para reduzir o custo total ou realizar o replay de callbacks de transações bem-sucedidas para processar pedidos sem o pagamento real. A exploração bem-sucedida leva a perdas financeiras diretas, discrepâncias de inventário e potencial comprometimento da integridade do e-commerce da aplicação.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### O valor da transação é validado no lado do servidor antes do processamento?
- [ ] Sim — o valor é estritamente validado contra o banco de dados de produtos no servidor *(Mais seguro)*  
- [ ] Sim — o valor é validado, mas o bypass da lógica **é possível** via parameter pollution ou troca de moeda  
- [ ] Não — o valor da transação **pode** ser modificado na requisição do lado do cliente e é aceito pelo backend  

### A quantidade de itens pode ser manipulada para resultar em um total zero ou negativo?
- [ ] Não — quantidades negativas ou zero são rejeitadas pela lógica de validação da aplicação  
- [ ] Sim — quantidades negativas são aceitas no carrinho, mas o total é corrigido no checkout  
- [ ] Sim — quantidades negativas **podem** ser usadas para reduzir o total geral do pedido ou obter um crédito *(Crítico)*  

### A aplicação gerencia de forma segura callbacks ou webhooks de gateways de pagamento?
- [ ] Sim — os callbacks exigem uma assinatura criptográfica válida que **não pode** ser falsificada  
- [ ] Sim — as assinaturas são verificadas, mas a chave secreta é fraca ou a verificação **pode** ser contornada  
- [ ] Não — a aplicação depende de callbacks não autenticados ou não verificados para confirmar o status do pagamento  

### A aplicação é vulnerável a race conditions durante a fase de checkout ou pagamento?
- [ ] Não — integridade transacional e mecanismos de bloqueio de banco de dados estão **habilitados**  
- [ ] Sim — race conditions **são possíveis**, mas não impactam o preço final ou o processamento do pedido  
- [ ] Sim — requisições simultâneas **podem** levar a múltiplos processamentos para um único pagamento ou "double-spending" de vales-presente  

### Códigos de desconto ou pontos de fidelidade estão sujeitos a manipulação ou reutilização?
- [ ] Não — os códigos são validados para uso único e restrições de lógica são **aplicadas**  
- [ ] Sim — os códigos podem ser reutilizados ou aplicados a itens não elegíveis, mas o impacto é limitado  
- [ ] Sim — atacantes **podem** aplicar múltiplos códigos conflitantes ou burlar prazos de validade e limites de uso

---

## WSTG-CLNT-01 — Teste para Cross-Site Scripting Baseado em DOM (DOM XSS)

O Cross-Site Scripting baseado em DOM (DOM XSS) ocorre quando uma aplicação contém JavaScript no lado do cliente (client-side) que processa dados de uma fonte não confiável de maneira insegura, geralmente escrevendo os dados de volta no Document Object Model (DOM) através de um sink perigoso. Diferente do XSS refletido ou armazenado, a vulnerabilidade existe inteiramente no código do lado do cliente; o Payload muitas vezes não é enviado ao servidor, pois é processado por sinks como `innerHTML`, `eval()` ou `document.write()`. Atacantes exploram isso criando URLs com fragmentos ou parâmetros maliciosos que, quando carregados pelo navegador da vítima, executam JavaScript arbitrário no contexto da sessão do usuário. Isso pode levar à exfiltração de tokens de sessão, roubo de dados sensíveis e ações não autorizadas realizadas em nome do usuário autenticado.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Ferramentas:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Fontes não confiáveis são utilizadas para capturar dados controlados pelo usuário no JavaScript?
- [ ] Não — O JavaScript **não** utiliza `location.hash`, `location.search` ou `document.referrer`  
- [ ] Sim — Fontes não confiáveis são utilizadas, mas os dados **não** são passados para nenhum sink de execução  
- [ ] Sim — Fontes não confiáveis são utilizadas e os dados fluem diretamente para a lógica do lado do cliente  

### Dados controlados pelo usuário são passados para sinks perigosos do DOM?
- [ ] Não — Sinks como `innerHTML`, `outerHTML`, `document.write()` ou `eval()` **não** são utilizados  
- [ ] Sim — Sinks perigosos estão presentes, mas os dados são rigorosamente **sanitizados** utilizando uma biblioteca segura como o DOMPurify  
- [ ] Sim — Sinks perigosos estão presentes e os dados são processados com filtragem **insuficiente** ou baseada em regex customizada  
- [ ] Sim — Sinks perigosos estão presentes e os dados são passados **sem** qualquer sanitização ou codificação (Encoding) *(Crítico)*  

### O sink de execução pode ser alcançado através de um payload baseado em URL?
- [ ] Não — O fluxo da fonte (source) para o sink **não** pode ser disparado via entrada externa  
- [ ] Sim — O fluxo é **possível**, mas requer interação complexa do usuário  
- [ ] Sim — O fluxo é **possível** e pode ser disparado via um fragmento de URL simples ou parâmetro de consulta (query parameter)  

### Cabeçalhos de segurança modernos ou mitigações baseadas no navegador estão neutralizando o risco?
- [ ] Sim — Uma `Content-Security-Policy` (CSP) estrita com `script-src` e `trusted-types` está **habilitada** e impede a execução  
- [ ] Sim — Uma CSP está presente, mas `unsafe-inline` ou hashes fracos tornam um bypass **possível**  
- [ ] Não — Nenhuma CSP está presente para mitigar a execução baseada em DOM  

### A aplicação utiliza frameworks de front-end com proteções integradas?
- [ ] Sim — O framework (ex: Angular, React) codifica automaticamente os dados e o bypass **não é possível**  
- [ ] Sim — O framework é utilizado, mas "escape hatches" como `dangerouslySetInnerHTML` estão **habilitados**  
- [ ] Não — A aplicação utiliza JavaScript puro (vanilla) ou bibliotecas legadas (ex: versões antigas do jQuery) sem auto-escaping

---

## WSTG-CLNT-02 — Teste de Execução de JavaScript

O teste de execução de JavaScript foca na identificação de instâncias onde uma aplicação lida incorretamente com dados fornecidos pelo usuário, levando à execução de scripts não autorizados no contexto do navegador da vítima. Esta vulnerabilidade normalmente resulta em Cross-Site Scripting (XSS), o qual permite que atacantes exfiltrem cookies de sessão, manipulem o Document Object Model (DOM) ou realizem ações em nome do usuário. Pentesters examinam sinks (sumidouros) como `innerHTML`, `document.write()` e `eval()` onde dados não sanitizados de sources (origens) como fragmentos de URL, `localStorage` ou `window.name` podem ser processados. A exploração bem-sucedida compromete a integridade e a confidencialidade da sessão do usuário e pode servir como um pivô para ataques client-side mais complexos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Ferramentas:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### A aplicação processa dados fornecidos pelo usuário em sinks de JavaScript perigosos?
- [ ] Não — todos os dados são sanitizados ou utilizados em sinks seguros como `textContent`  
- [ ] Sim — os dados são processados em sinks perigosos, mas a sanitização/codificação **é aplicada**  
- [ ] Sim — os dados são processados em sinks perigosos e a sanitização **não é aplicada**  

### Uma Content Security Policy (CSP) está presente para mitigar a execução de scripts?
- [ ] Sim — uma CSP restritiva está **habilitada** e o bypass **não é possível**  
- [ ] Sim — uma CSP está **habilitada**, mas o bypass **é possível** via `unsafe-inline` ou CDNs inseguras  
- [ ] Não — nenhuma CSP está **habilitada**  

### O JavaScript pode ser executado via parâmetros de URL ou fragmentos (DOM-based XSS)?
- [ ] Não — a entrada é devidamente codificada e a execução **não é possível**  
- [ ] Sim — a entrada é refletida no DOM, mas a execução **é prevenida** por proteções de frameworks modernos  
- [ ] Sim — a entrada é executada diretamente no navegador, e a exploração **é possível**  

### Os manipuladores de eventos (event handlers) ou esquemas de URI são vulneráveis à injeção de script?
- [ ] Não — a entrada é sanitizada para remover atributos de eventos e esquemas `javascript:`  
- [ ] Sim — manipuladores de eventos **podem** ser injetados, mas filtros limitam a complexidade do payload  
- [ ] Sim — a execução arbitrária de JavaScript via atributos `onerror`, `onload` ou `href` **é possível**

---

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

## WSTG-CLNT-04 — Testing for Client-Side URL Redirect

O redirecionamento de URL no lado do cliente (Client-side URL redirection) ocorre quando uma aplicação web aceita entrada controlada pelo usuário — geralmente através de parâmetros de URL ou fragmentos de hash — e a utiliza dentro de um sink de redirecionamento no lado do cliente, como `window.location` ou `document.location.href`. Esta vulnerabilidade é explorada principalmente por atacantes para conduzir campanhas sofisticadas de phishing, pois a vítima confia inicialmente no domínio legítimo antes de ser redirecionada silenciosamente para um site malicioso. Além do phishing, redirecionamentos no lado do cliente podem frequentemente ser encadeados para exfiltrar dados sensíveis, como tokens de acesso OAuth ou identificadores de sessão, se o redirecionamento ocorrer enquanto esses valores estiverem presentes na URL ou no cabeçalho `Referer`. Em Single Page Applications (SPAs) modernas, essa vulnerabilidade aparece frequentemente em lógicas de roteamento, parâmetros "return to" após a autenticação ou funcionalidades de troca de idioma.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média* |

> *A severidade torna-se Alta se o redirecionamento puder ser usado para exfiltrar tokens OAuth, IDs de sessão ou contornar controles de segurança em um fluxo de autenticação.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Ferramentas:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### A aplicação utiliza scripts no lado do cliente para lidar com redirecionamentos baseados na entrada do usuário?
- [ ] Não — o redirecionamento é tratado exclusivamente no lado do servidor via códigos de status HTTP `3xx`  
- [ ] Sim — a aplicação utiliza sinks de JavaScript como `window.location` para navegar com base em parâmetros de URL  
- [ ] Sim — a aplicação utiliza um roteador de framework no lado do cliente (ex: React Router, Vue Router) que processa caminhos fornecidos pelo usuário  

### Existem controles de validação de entrada ou whitelisting implementados para a URL de destino?
- [ ] Sim — uma **whitelist** rigorosa de domínios/caminhos permitidos é aplicada e o bypass **não é possível**  
- [ ] Sim — a validação está presente, mas depende de regex fracas ou blacklists, e o bypass **é possível**  
- [ ] Não — a aplicação aceita qualquer string arbitrária como destino de redirecionamento  

### A lógica de redirecionamento pode ser contornada utilizando técnicas comuns de ofuscação?
- [ ] Não — os controles tratam corretamente caracteres codificados, URLs relativas ao protocolo (`//`) e barras invertidas  
- [ ] Sim — o bypass **é possível** utilizando URL encoding ou double-encoding  
- [ ] Sim — o bypass **é possível** utilizando URLs relativas ao protocolo ou caracteres `@` (ex: `https://expected.com@attacker.com`)  
- [ ] Sim — o bypass **é possível** utilizando comportamentos específicos do navegador (browser quirks) ou injeção de null byte  

### O processo de redirecionamento expõe dados sensíveis ao site de destino?
- [ ] Não — nenhum dado sensível está presente na URL ou é transmitido via cabeçalhos durante o redirecionamento  
- [ ] Sim — dados sensíveis (ex: tokens de sessão, chaves de API) **são vazados** via cabeçalho `Referer` para o site externo  
- [ ] Sim — dados sensíveis presentes no fragmento da URL (`#`) **estão acessíveis** para os scripts do site de destino  

### É possível executar JavaScript através do sink de redirecionamento (DOM-based XSS)?
- [ ] Não — o sink permite apenas navegação para os protocolos `http` ou `https`  
- [ ] Sim — o sink de redirecionamento permite URIs `javascript:` ou `data:`, tornando o DOM-based XSS **possível**

---

## WSTG-CLNT-05 — Teste de Injeção de CSS (CSS Injection)

A Injeção de CSS (CSS Injection) ocorre quando uma aplicação permite que a entrada fornecida pelo usuário influencie as Folhas de Estilo em Cascata (CSS) de uma página web sem a devida sanitização ou escape. Embora muitas vezes seja percebida como um problema estético, atacantes podem aproveitar a injeção de CSS para exfiltrar dados sensíveis, como tokens CSRF ou IDs de sessão, utilizando seletores de atributos e propriedades de imagem de fundo (`background-image`) para disparar requisições externas. Esta vulnerabilidade ocorre tipicamente em endpoints onde as preferências do usuário (ex: temas personalizados, cores de fonte) são refletidas dentro de blocos `<style>` ou atributos `style` inline. Do ponto de vista de um atacante, a exploração bem-sucedida pode levar ao UI redressing, phishing por meio de modificação não autorizada do layout, ou exfiltração furtiva de dados em ambientes onde Políticas de Segurança de Conteúdo (CSP) rígidas poderiam bloquear ataques baseados em JavaScript.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Média / Alta* |

> *A gravidade torna-se Alta se o ponto de injeção permitir a exfiltração de atributos sensíveis, como tokens CSRF, ou se puder ser utilizado para UI redressing em fluxos de trabalho sensíveis.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### A aplicação reflete entrada controlada pelo usuário dentro de contextos CSS?
- [ ] Não — a entrada **não** é refletida em tags de estilo, atributos ou arquivos CSS  
- [ ] Sim — a entrada é refletida, mas estritamente limitada a valores alfanuméricos seguros  
- [ ] Sim — a entrada é refletida dentro de um atributo `style` ou bloco `<style>`  

### Os metacaracteres e funções específicas de CSS são devidamente sanitizados?
- [ ] Sim — caracteres como `{`, `}`, `:` e funções como `url()` estão **devidamente escapados**  
- [ ] Sim — a sanitização existe, mas o bypass **é possível** via codificação ou comentários CSS  
- [ ] Não — nenhuma sanitização é aplicada aos metacaracteres CSS  

### A exfiltração de dados é possível utilizando seletores CSS?
- [ ] Não — os seletores **não podem** disparar requisições externas ou vazar dados  
- [ ] Sim — a exfiltração parcial de dados **é possível** via seletores de atributo e `background-image`  
- [ ] Sim — a exfiltração total de tokens sensíveis **é possível** utilizando técnicas automatizadas de força bruta (Brute Force)  

### Uma Política de Segurança de Conteúdo (CSP) mitiga o risco de injeção de CSS?
- [ ] Sim — o `style-src` está restrito a 'self' e o `img-src` ou `connect-src` **estão restritos**  
- [ ] Sim — a CSP está presente, mas utiliza 'unsafe-inline' ou permite domínios externos curinga (wildcards)  
- [ ] Não — nenhuma CSP foi implementada para prevenir o carregamento de recursos externos via CSS  

### A vulnerabilidade pode ser usada para realizar UI redressing ou phishing?
- [ ] Não — o escopo da injeção é muito limitado para modificar o layout significativamente  
- [ ] Sim — a modificação da UI **é possível**, permitindo a sobreposição de elementos maliciosos  
- [ ] Sim — o controle total do layout da página **é possível**, facilitando ataques de phishing altamente convincentes

---

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

## WSTG-CLNT-09 — Testing for Clickjacking

O Clickjacking, também conhecido como UI redressing, ocorre quando um atacante utiliza camadas transparentes ou opacas para enganar um usuário, levando-o a clicar em um botão ou link em outra página quando sua intenção era clicar na página de nível superior. Esta vulnerabilidade compromete a integridade das ações do usuário, potencialmente levando à modificação não autorizada de dados, alterações de conta ou transferências financeiras realizadas em nome da vítima. Os atacantes normalmente exploram isso incorporando o site alvo dentro de um `<iframe>` em um site malicioso controlado e sobrepondo-o com conteúdo enganoso ou iscas atrativas. Ocorre com maior frequência em páginas que executam ações sensíveis de mudança de estado, como botões de "Excluir Conta", formulários de redefinição de senha ou painéis de configuração administrativa.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se ações sensíveis (ex: transferências de fundos, exclusão de conta ou escalação de privilégios) puderem ser acionadas via clickjacking.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Ferramentas:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### A aplicação implementa cabeçalhos no lado do servidor para prevenir o enquadramento (framing) não autorizado?
- [ ] Sim — `X-Frame-Options` ou `Content-Security-Policy` **estão aplicados** e previnem o framing *(Mais seguro)*  
- [ ] Sim — os cabeçalhos estão presentes, mas **mal configurados** (ex: `X-Frame-Options: ALLOW-FROM`, que carece de amplo suporte dos navegadores)  
- [ ] Não — nenhum cabeçalho de proteção contra framing **está aplicado**  

### A diretiva `frame-ancestors` da `Content-Security-Policy` (CSP) está implementada?
- [ ] Sim — `frame-ancestors 'none'` ou `'self'` **está aplicado**  
- [ ] Sim — `frame-ancestors` está presente, mas **permite** origens não confiáveis ou excessivamente amplas  
- [ ] Não — a diretiva está **ausente** ou a CSP não está implementada  

### O cabeçalho `X-Frame-Options` (XFO) está configurado corretamente para suporte a navegadores legados?
- [ ] Sim — `DENY` ou `SAMEORIGIN` **está aplicado**  
- [ ] Sim — o XFO está presente, mas é **ignorado** por navegadores modernos porque existe uma diretiva CSP `frame-ancestors`  
- [ ] Não — o cabeçalho XFO está **ausente** ou definido com um valor inseguro  

### As proteções contra framing podem ser burladas utilizando técnicas conhecidas?
- [ ] Não — o bypass **não é possível** via técnicas padrão (double-framing, etc.)  
- [ ] Sim — a proteção **pode** ser burlada devido a regex ou lógica fraca na white-list do `frame-ancestors`  
- [ ] Sim — a proteção **pode** ser burlada porque a aplicação depende exclusivamente de frame-busting baseado em JavaScript  

### Uma ação sensível é explorável através de uma prova de conceito (PoC) de clickjacking?
- [ ] Não — o framing está bloqueado ou nenhuma ação sensível é alcançável  
- [ ] Sim — o UI redressing **é possível**, mas requer interação complexa do usuário  
- [ ] Sim — o UI redressing **é possível** e permite ações imediatas de mudança de estado *(Crítico)*

---

## WSTG-CLNT-10 — Testes de WebSockets

Os testes de WebSocket concentram-se no canal de comunicação full-duplex estabelecido entre o cliente e o servidor, o qual ignora os padrões tradicionais de requisição-resposta HTTP. Pentesters avaliam a segurança do processo de handshake, a presença de proteções contra Cross-Site WebSocket Hijacking (CSWSH) e o rigor da validação de entrada aplicada aos payloads de mensagens. A exploração normalmente envolve o sequestro (hijacking) de uma sessão ativa para exfiltrar dados em tempo real ou a injeção de payloads maliciosos que acionam vulnerabilidades do lado do servidor, como Command Injection ou SQLi através do socket persistente. Como muitos Web Application Firewalls (WAFs) tradicionais não inspecionam o tráfego WebSocket de forma eficaz, este protocolo frequentemente fornece um vetor furtivo para contornar defesas de perímetro.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Ferramentas:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### A conexão WebSocket é estabelecida através de um canal seguro?
- [ ] Sim — `wss://` (WebSocket Secure) é obrigatório para todas as conexões  
- [ ] Não — `ws://` é utilizado, permitindo a interceptação por person-in-the-middle (PITM)  

### A aplicação está protegida contra Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] Não — o servidor valida estritamente o cabeçalho `Origin` durante o handshake  
- [ ] Sim — a validação do cabeçalho `Origin` é fraca ou **pode** ser contornada (bypass) através de origens nulas ou forjadas (spoofed)  
- [ ] Sim — nenhuma validação do cabeçalho `Origin` é realizada, permitindo que atacantes cross-site iniciem uma conexão *(Crítico)*  

### A validação e sanitização de entrada são aplicadas aos payloads das mensagens WebSocket?
- [ ] Sim — todas as mensagens de entrada são sanitizadas e validadas contra um esquema (schema)  
- [ ] Sim — existe alguma validação, mas o bypass **é possível** utilizando payloads codificados ou não padronizados  
- [ ] Não — as mensagens são processadas diretamente, permitindo injeção (XSS, SQLi, RCE) ou manipulação lógica  

### As verificações de autenticação e autorização são realizadas em cada mensagem WebSocket?
- [ ] Sim — a sessão é verificada para cada mensagem ou o protocolo é stateless com tokens  
- [ ] Sim — a autenticação ocorre no handshake, mas a autorização para ações específicas **não é** aplicada por mensagem  
- [ ] Não — a autenticação é verificada apenas no handshake, permitindo que o sequestro de sessão (session hijacking) conceda acesso total  

### O servidor implementa rate limiting ou restrições de recursos no WebSocket?
- [ ] Sim — rate limiting e tamanhos máximos de mensagem estão **habilitados** e são aplicados  
- [ ] Não — não existem limites, permitindo Denial of Service (DoS) via flooding de mensagens ou tamanhos de payload elevados

---

## WSTG-CLNT-11 — Testar Web Messaging

O Web Messaging, ou `postMessage`, permite a comunicação cross-origin entre objetos window, como uma página pai e um popup gerado ou um iframe incorporado. Este mecanismo é fundamental para a funcionalidade da web moderna, mas introduz riscos significativos se a aplicação receptora falhar em verificar a identidade do remetente ou manipular incorretamente o payload da mensagem. Atacantes exploram essas vulnerabilidades hospedando um site malicioso que incorpora a aplicação alvo e envia mensagens manipuladas para disparar ações não intencionais, exfiltrar dados sensíveis ou executar Cross-Site Scripting (XSS) baseado em DOM. A exploração bem-sucedida ocorre quando os listeners de mensagens não validam estritamente a propriedade `origin` ou passam o `message.data` diretamente para sinks de execução perigosos.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Ferramentas:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### A aplicação utiliza a API `postMessage` para comunicação entre documentos?
- [ ] Não — listeners de `postMessage` **não** estão presentes no código client-side  
- [ ] Sim — o `postMessage` é utilizado para comunicação interna de componentes  
- [ ] Sim — o `postMessage` é utilizado para comunicar com domínios de terceiros ou widgets  

### A origem das mensagens recebidas é estritamente validada?
- [ ] Sim — a origem é verificada em relação a uma whitelist estrita usando `===` ou comparação idêntica *(Mais seguro)*  
- [ ] Sim — a origem é validada usando regex, mas a lógica **é** suscetível a bypass (ex: `^https://trusted.com`)  
- [ ] Não — a propriedade `origin` **não** é verificada, permitindo que qualquer domínio envie mensagens  
- [ ] Não — um wildcard `*` é usado no `targetOrigin` do remetente, potencialmente vazando dados para listeners maliciosos  

### O payload da mensagem é sanitizado antes de ser processado pela aplicação?
- [ ] Sim — os dados são tratados como texto simples e nenhum sink perigoso é utilizado  
- [ ] Sim — os dados são analisados (ex: `JSON.parse`) e validados antes do uso, e o bypass **não é possível**  
- [ ] Não — os dados da mensagem são passados diretamente para sinks perigosos como `innerHTML`, `eval()` ou `setTimeout()`  
- [ ] Não — os dados da mensagem são usados para navegar na janela ou alterar o `location.href` sem validação  

### Um atacante pode disparar ações não autorizadas ou exfiltrar dados através de uma mensagem maliciosa?
- [ ] Não — mesmo com uma mensagem forjada, nenhuma ação ou dado sensível pode ser acessado  
- [ ] Sim — uma mensagem manipulada **pode** disparar mudanças de estado sensíveis (ex: alteração de senha, atualização de perfil)  
- [ ] Sim — uma mensagem manipulada **pode** resultar em Cross-Site Scripting (XSS) baseado em DOM ou exfiltração de tokens CSRF/dados de sessão

---

## WSTG-CLNT-12 — Test Browser Storage

O teste de armazenamento do navegador envolve analisar como uma aplicação utiliza mecanismos no lado do cliente (client-side), tais como LocalStorage, SessionStorage, IndexedDB e WebSQL para persistir dados. Informações sensíveis armazenadas de forma insegura — incluindo PII, tokens de autenticação ou identificadores de sessão — podem ser comprometidas se um atacante realizar um Cross-Site Scripting (XSS) ou obtiver acesso físico ao dispositivo do usuário. Pentesters devem determinar se os dados são armazenados em texto claro (cleartext), se permanecem acessíveis após o logout e se o ciclo de vida do armazenamento é gerenciado adequadamente para evitar vazamento de dados. Do ponto de vista de um atacante, esses mecanismos de armazenamento são alvos lucrativos para a exfiltração de informações de manutenção de estado que permitem o Session Hijacking ou a persistência de conta a longo prazo.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Médio / Alto* |

> *A severidade torna-se Alta se tokens de sessão ou PII sensíveis forem armazenados no LocalStorage e estiverem acessíveis via XSS.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### A aplicação armazena informações sensíveis no armazenamento do navegador?
- [ ] Não — a aplicação **não** utiliza armazenamento do navegador ou apenas armazena estados de UI não sensíveis  
- [ ] Sim — dados sensíveis são armazenados, mas **estão criptografados** utilizando criptografia robusta no lado do cliente  
- [ ] Sim — dados sensíveis (tokens, PII, segredos) são armazenados em **texto claro (cleartext)** *(Médio)*  

### Os dados armazenados estão acessíveis a scripts não autorizados (risco de XSS)?
- [ ] Não — os dados são armazenados em cookies HttpOnly (não no armazenamento do navegador) ou o armazenamento **não é utilizado**  
- [ ] Sim — LocalStorage/SessionStorage é utilizado, tornando os dados **completamente acessíveis** através de qualquer payload de XSS *(Alto)*  

### O armazenamento do navegador é limpo adequadamente após o logout do usuário?
- [ ] Sim — todo o armazenamento relacionado à aplicação é **explicitamente limpo** durante o processo de logout  
- [ ] Não — o armazenamento persiste após o logout, mas **não** contém dados de sessão sensíveis  
- [ ] Não — tokens de autenticação ou PII **persistem** no armazenamento após o encerramento da sessão *(Médio)*  

### A aplicação utiliza IndexedDB ou WebSQL para conjuntos de dados grandes/sensíveis?
- [ ] Não — esses mecanismos de armazenamento **não são utilizados**  
- [ ] Sim — os dados são armazenados e **devidamente sanitizados** antes de serem renderizados no DOM  
- [ ] Sim — conjuntos de dados sensíveis são armazenados em **texto claro (cleartext)** dentro de estruturas IndexedDB/WebSQL  

### A integridade dos dados armazenados pode ser manipulada para afetar a lógica da aplicação?
- [ ] Não — a aplicação valida ou assina os dados antes de processá-los a partir do armazenamento  
- [ ] Sim — a modificação de valores de armazenamento (ex: funções de usuário, flags) **é possível** e resulta em comportamento alterado da aplicação

---

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

## WSTG-CLNT-14 — Teste de Reverse Tabnabbing

O Reverse Tabnabbing é uma vulnerabilidade client-side onde uma página aberta via `target="_blank"` mantém uma referência funcional para a página de origem (parent page) através do objeto `window.opener`. Isso permite que um site de destino controlado por um atacante redirecione programaticamente a aba original e confiável para uma URL maliciosa, geralmente uma página de phishing projetada para capturar credenciais. O ataque explora a confiança intrínseca do usuário na aba original, pois é improvável que ele perceba que a página que acabou de deixar navegou para um domínio diferente. As práticas de segurança modernas exigem o uso dos atributos `rel="noopener"` ou `rel="noreferrer"` em tags de âncora, ou a configuração da propriedade `opener` como null em JavaScript, para evitar essa comunicação entre janelas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média* |

> *A severidade é Baixa na maioria dos navegadores modernos, pois agora eles definem `target="_blank"` como `rel="noopener"` por padrão. A severidade torna-se Média se a aplicação for direcionada a navegadores legados ou utilizar `window.open()` sem definir a propriedade `opener` como null.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Existem links que abrem em uma nova janela ou aba?
- [ ] Não — nenhum link utiliza `target="_blank"` ou redirecionamento equivalente baseado em JavaScript  
- [ ] Sim — `target="_blank"` é usado exclusivamente em links internos e confiáveis  
- [ ] Sim — `target="_blank"` é usado em links externos ou URLs fornecidas pelo usuário  

### A relação `window.opener` está restrita nas tags de âncora?
- [ ] Sim — `rel="noopener"` ou `rel="noreferrer"` é **aplicado consistentemente** a todos os links com `target="_blank"` *(Mais seguro)*  
- [ ] Sim — os atributos são aplicados, mas estão **ausentes** em links de alto risco ou gerados pelo usuário  
- [ ] Não — os atributos **não são aplicados**, permitindo que a página filha acesse `window.opener`  

### A aplicação está vulnerável ao Reverse Tabnabbing via JavaScript `window.open()`?
- [ ] Não — `window.open()` não é utilizado ou define explicitamente a propriedade `opener` como null  
- [ ] Sim — `window.open()` é utilizado e a referência `opener` **permanece acessível** para a janela filha  

### Um atacante pode controlar o destino de um link que abre em uma nova aba?
- [ ] Não — todos os destinos de links estão codificados (hardcoded) e apontam para domínios confiáveis  
- [ ] Sim — os destinos dos links são fornecidos pelo usuário (ex: perfis de redes sociais, links de bio) e **não** são sanitizados para proteções contra tabnabbing

---

## WSTG-APIT-01 — Reconhecimento de API

O reconhecimento de API é o processo sistemático de identificar e mapear a superfície de ataque da API para descobrir endpoints, métodos suportados e estruturas de dados subjacentes. Atacantes realizam esta fase para descobrir APIs não documentadas (Shadow APIs), versões obsoletas com vulnerabilidades legadas e arquivos de documentação expostos, como definições Swagger ou OpenAPI, que revelam a lógica de negócio interna. Ao analisar padrões de URL previsíveis, arquivos JavaScript do lado do cliente e resultados de Brute Force de diretórios, um adversário pode construir um mapa abrangente da API para identificar alvos para ataques de Broken Object Level Authorization (BOLA) ou Mass Assignment. Este reconhecimento normalmente visa caminhos comuns, como `/api/v1/`, `/swagger.json` ou `/graphql`, para obter um roteiro da funcionalidade de backend da aplicação.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Informativa / Baixa |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Ferramentas:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Os arquivos de documentação da API (Swagger, OpenAPI, WSDL) estão acessíveis publicamente?
- [ ] Não — A documentação da API **não** está acessível ou é restrita por autenticação  
- [ ] Sim — A documentação está acessível, mas requer credenciais válidas  
- [ ] Sim — A documentação da API (ex: `/swagger-ui.html`) está **acessível publicamente** sem autenticação  

### Endpoints de API não documentados ou "shadow" podem ser descobertos via fuzzing?
- [ ] Não — O Fuzzing de diretórios e endpoints não revelou caminhos não documentados  
- [ ] Sim — Endpoints não documentados foram encontrados, mas **não** podem ser acessados sem autorização  
- [ ] Sim — Endpoints não documentados foram encontrados e **podem** ser acessados sem autorização válida  

### A aplicação revela múltiplas versões da API (ex: /v1/, /v2/, /beta/)?
- [ ] Não — Apenas a versão atual e protegida (hardened) da API está acessível  
- [ ] Sim — Versões legadas existem, mas implementam os **mesmos** controles de segurança da versão atual  
- [ ] Sim — Versões legadas (ex: `/v1/`) estão acessíveis e **carecem** dos controles de segurança das versões mais recentes  

### Recursos do lado do cliente (JavaScript/Apps Móveis) vazam estruturas de endpoints ou chaves de API?
- [ ] Não — O código do lado do cliente **não** contém caminhos de API codificados (hardcoded) ou chaves sensíveis  
- [ ] Sim — O código do lado do cliente contém mapas de endpoints de API, mas **nenhuma** chave sensível  
- [ ] Sim — Chaves de API, secrets ou URLs de endpoints apenas internos **estão** codificados (hardcoded) em recursos do lado do cliente  

### Parâmetros ou cabeçalhos ocultos são detectáveis em endpoints conhecidos?
- [ ] Não — O Fuzzing de parâmetros (ex: via `Arjun`) **não** revelou entradas ocultas  
- [ ] Sim — Parâmetros ocultos (ex: `debug=true`, `admin=1`) foram descobertos, mas **não** são funcionais  
- [ ] Sim — Parâmetros ocultos foram descobertos e **podem** ser usados para alterar o comportamento da aplicação

---

## WSTG-APIT-02 — API Broken Object Level Authorization

A Quebra de Autorização em Nível de Objeto (Broken Object Level Authorization - BOLA), também conhecida como Referência Direta Insegura a Objetos (Insecure Direct Object Reference - IDOR), ocorre quando uma API falha ao validar se um utilizador possui as permissões adequadas para aceder ou manipular um recurso específico identificado por um ID. Os atacantes exploram esta vulnerabilidade através da enumeração sistemática ou adivinhação de identificadores — tais como IDs numéricos ou UUIDs — em caminhos de pedidos (request paths), parâmetros de consulta (query parameters) ou corpos JSON para aceder a dados pertencentes a outros utilizadores. Esta vulnerabilidade é o problema mais comum e impactante na segurança de APIs modernas, podendo levar à exfiltração massiva de dados, modificação não autorizada ou ao comprometimento total de contas (account takeover). Do ponto de vista de um atacante, o objetivo é identificar endpoints que aceitem identificadores de objetos e testar se a lógica de autorização no lado do servidor (server-side) está ausente ou pode ser contornada através da manipulação desses IDs.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Estado do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Ferramentas:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Os identificadores de objetos são previsíveis ou enumeráveis nos pedidos da API?
- [ ] Não — os identificadores são longos, aleatórios e criptograficamente seguros (ex.: UUIDv4)  
- [ ] Sim — os identificadores são números inteiros sequenciais (ex.: `101`, `102`)  
- [ ] Sim — os identificadores seguem um padrão previsível ou são derivados de informações públicas  

### A API realiza a validação do proprietário do objeto no lado do servidor para todos os pedidos?
- [ ] Sim — as verificações de autorização são aplicadas em todos os pedidos e o bypass **não é possível**  
- [ ] Sim — as verificações são aplicadas, mas o bypass **é possível** através de parameter pollution ou method tunneling  
- [ ] Não — a aplicação baseia-se exclusivamente na autenticação do utilizador sem verificar a propriedade do recurso específico  

### É possível aceder ou modificar o recurso de outro utilizador alterando o identificador?
- [ ] Não — o acesso não autorizado a recursos de outros utilizadores **não é possível**  
- [ ] Sim — o acesso de **leitura** não autorizado (Horizontal BOLA) **é possível**  
- [ ] Sim — a **modificação** ou **eliminação** não autorizada (Horizontal BOLA) **é possível**  

### A API permite aceder a objetos de nível administrativo ou de sistema através da substituição de IDs?
- [ ] Não — os recursos administrativos estão protegidos por camadas de autorização secundárias  
- [ ] Sim — o acesso ou modificação de recursos de nível de sistema (Vertical BOLA) **é possível**  

### A verificação de autorização pode ser contornada movendo o identificador para uma parte diferente do pedido?
- [ ] Não — a lógica de autorização é consistente, independentemente da localização do parâmetro  
- [ ] Sim — o bypass **é possível** quando o ID é movido do caminho do URL para o corpo JSON ou cabeçalhos (headers)  
- [ ] Sim — o bypass **é possível** quando são fornecidos múltiplos IDs e o servidor processa o ID não autorizado

---

## WSTG-APIT-99 — Teste de GraphQL

GraphQL é uma linguagem de consulta para APIs que permite aos clientes solicitar estruturas de dados específicas, mas configurações incorretas frequentemente levam a riscos de segurança significativos, incluindo exposição de informações e exaustão de recursos. Vulnerabilidades geralmente surgem de introspecção habilitada, falta de limites de profundidade/complexidade de consulta e falha de autorização em nível de objeto (Broken Object-Level Authorization - BOLA) dentro das funções de resolução (resolvers). Atacantes exploram esses endpoints utilizando a introspecção para mapear todo o esquema (schema), aproveitando fragmentos circulares para causar Negação de Serviço (Denial of Service - DoS) ou ignorando a autorização ao acessar campos não autorizados por meio de consultas aninhadas. Os testes focam nos endpoints `/graphql`, `/v1/graphql` ou `/graphiql` para garantir que a implementação aplique controles de acesso rigorosos e gerenciamento de recursos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta / Crítica* |

> *A Gravidade torna-se Crítica se a falha de autorização em nível de objeto (BOLA) permitir acesso não autorizado a PII ou mutações administrativas.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Ferramentas:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### O sistema de introspecção do GraphQL está habilitado?
- [ ] Não — a introspecção está **desabilitada** e o esquema não pode ser mapeado  
- [ ] Sim — a introspecção está **habilitada**, mas requer autenticação administrativa  
- [ ] Sim — a introspecção está **habilitada** e é publicamente acessível *(Alta)*  

### Limites de recursos (profundidade e complexidade) são aplicados nas consultas?
- [ ] Sim — limites rigorosos de profundidade e complexidade de consulta são **aplicados** e impostos  
- [ ] Sim — limites são **aplicados**, mas podem ser burlados usando aliases ou fragmentos  
- [ ] Não — nenhum limite é **aplicado**, permitindo consultas recursivas e DoS  

### A falha de autorização em nível de objeto (BOLA) está presente nos resolvers?
- [ ] Não — a autorização é **aplicada** no nível de campo e objeto para cada consulta  
- [ ] Sim — a autorização é **aplicada** a alguns campos, mas objetos sensíveis estão expostos  
- [ ] Sim — a autorização **não é aplicada**, permitindo o acesso a qualquer objeto via manipulação de ID  

### As mutações do GraphQL estão devidamente protegidas contra uso não autorizado?
- [ ] Não — mutações **não** estão presentes no esquema  
- [ ] Sim — mutações exigem autenticação válida e verificações rigorosas de autorização  
- [ ] Sim — mutações são **possíveis** para usuários não autenticados ou carecem de autorização  

### As mensagens de erro vazam detalhes sensíveis da implementação ou do esquema?
- [ ] Não — as mensagens de erro são genéricas e **não** revelam a lógica interna  
- [ ] Sim — as mensagens de erro revelam tipos de banco de dados subjacentes ou sugestões via "Did you mean...?"  
- [ ] Sim — stack traces completos e dados de configuração sensíveis são **revelados** nas respostas