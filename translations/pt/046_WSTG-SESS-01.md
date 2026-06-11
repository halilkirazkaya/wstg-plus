## WSTG-SESS-01 — Teste de Esquema de Gerenciamento de Sessão

O teste do esquema de gerenciamento de sessão foca na análise de como a aplicação lida com o ciclo de vida e a estrutura dos identificadores de sessão para garantir que sejam robustos contra predição e sequestro (hijacking). Atacantes capturam múltiplos tokens de sessão para realizar análises estatísticas, buscando padrões, baixa entropia ou incrementos previsíveis que permitam forjar sessões válidas para outros usuários. Fraquezas no esquema frequentemente se manifestam como vulnerabilidades de fixação de sessão (Session Fixation) ou o uso de valores insuficientemente aleatórios, tipicamente encontrados em cookies HTTP, parâmetros de URL ou implementações de cabeçalhos personalizados. Comprometer o esquema de sessão é um ataque de alto impacto que concede a um adversário acesso completo ao estado autenticado de uma vítima sem a necessidade de suas credenciais primárias.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Ferramentas:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### O identificador de sessão é suficientemente complexo e aleatório?
- [ ] Sim — o token passa em testes estatísticos de aleatoriedade (alta entropia) e o bypass **não é possível**  
- [ ] Sim — o token é longo, mas contém padrões previsíveis ou segmentos estáticos  
- [ ] Não — o token é sequencial, baseado em timestamps previsíveis ou possui entropia **criticamente baixa** *(Crítico)*  

### Onde o identificador de sessão é transmitido e armazenado?
- [ ] O identificador é armazenado em um cookie `Secure` e `HttpOnly` *(Mais seguro)*  
- [ ] O identificador é armazenado em um cookie, mas faltam as flags `Secure` ou `HttpOnly`  
- [ ] O identificador é transmitido **apenas** via parâmetros de URL ou requisições `GET` *(Alto Risco)*  

### A aplicação emite um novo identificador de sessão após a autenticação bem-sucedida?
- [ ] Sim — um novo ID de sessão exclusivo **é gerado** imediatamente após o login  
- [ ] Não — o ID de sessão permanece o mesmo antes e depois do login *(Possível Session Fixation)*  

### O identificador de sessão está vinculado a um endereço IP ou User-Agent específico para evitar o reuso?
- [ ] Sim — a sessão é invalidada se o fingerprint do cliente mudar significativamente  
- [ ] Não — a sessão **pode** ser reutilizada em diferentes endereços IP ou dispositivos sem verificação adicional  

### Os identificadores de sessão são devidamente invalidados no lado do servidor após o logout ou timeout?
- [ ] Sim — o estado da sessão no lado do servidor (server-side) é **completamente destruído** após o logout  
- [ ] Não — a sessão permanece válida no servidor mesmo se o cookie do lado do cliente for excluído  
- [ ] Não — a sessão **nunca expira** ou possui um período de timeout excessivamente longo  

---