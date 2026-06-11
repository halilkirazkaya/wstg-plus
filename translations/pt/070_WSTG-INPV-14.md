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