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