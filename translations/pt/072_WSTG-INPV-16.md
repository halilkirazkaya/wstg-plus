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