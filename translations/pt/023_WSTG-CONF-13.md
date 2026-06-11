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