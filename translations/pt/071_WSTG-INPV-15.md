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