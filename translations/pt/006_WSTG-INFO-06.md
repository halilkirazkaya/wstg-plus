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