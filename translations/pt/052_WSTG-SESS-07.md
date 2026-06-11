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