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