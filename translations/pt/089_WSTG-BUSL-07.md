## WSTG-BUSL-07 — Testar Defesas Contra o Uso Indevido da Aplicação

As defesas contra o uso indevido da aplicação avaliam a capacidade da aplicação de identificar, registrar e responder a comportamentos anômalos de usuários que se desviam da lógica de negócios pretendida. Ao contrário da segurança tradicional baseada em assinaturas, essas defesas focam na detecção de padrões como requisições em ráfaga (rapid-fire), Credential Stuffing ou enumeração sequencial de recursos que sinalizam abuso automatizado. Do ponto de vista de um atacante, este teste determina a resiliência da aplicação contra automação e ataques "low and slow" projetados para exfiltrar dados ou esgotar recursos sem disparar alertas de segurança padrão. A falha na implementação dessas defesas geralmente resulta em Data Scraping massivo, Account Takeovers ou Denial of Service (DoS) por meio de funcionalidades legítimas, porém intensivas em recursos.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Mecanismos de rate-limiting ou throttling são aplicados em endpoints sensíveis?
- [ ] Sim — Rate Limiting rigoroso **é aplicado** e forçado em todos os endpoints sensíveis *(Mais seguro)*  
- [ ] Sim — Rate Limiting **é aplicado**, mas pode ser burlado via rotação de IP ou manipulação de cabeçalhos  
- [ ] Sim — Rate Limiting **é aplicado** apenas aos endpoints de autenticação, deixando outros expostos  
- [ ] Não — nenhum Rate Limiting ou Throttling **é aplicado** a qualquer funcionalidade da aplicação  

### A aplicação detecta e bloqueia padrões de interação automatizada?
- [ ] Sim — análise comportamental ou CAPTCHAs **estão habilitados** e bloqueiam bots de forma eficaz  
- [ ] Sim — anti-automação **está habilitado**, mas o bypass **é possível** usando navegadores headless ou ciclo de sessões  
- [ ] Não — a aplicação **não consegue** distinguir entre um usuário humano e um script automatizado  

### O comportamento anômalo é registrado e seguido por uma resposta defensiva?
- [ ] Sim — o uso indevido dispara bloqueio automatizado e alerta a equipe de segurança  
- [ ] Sim — o uso indevido é registrado para auditoria, mas **nenhuma** ação defensiva automatizada é tomada  
- [ ] Não — a aplicação **não registra** nem responde a padrões de atividade incomuns  

### As defesas podem ser burladas manipulando identificadores ou cabeçalhos do lado do cliente?
- [ ] Não — as defesas dependem do estado do lado do servidor e de telemetria verificada  
- [ ] Sim — os controles **podem** ser burlados limpando cookies ou rotacionando strings de `User-Agent`  
- [ ] Sim — os controles **podem** ser burlados injetando cabeçalhos como `X-Forwarded-For` ou `X-Real-IP`  

---