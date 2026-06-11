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