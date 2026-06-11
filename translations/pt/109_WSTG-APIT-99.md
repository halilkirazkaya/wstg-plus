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

---