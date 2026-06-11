## WSTG-SESS-11 — Teste de Sessões Simultâneas

O teste de sessões simultâneas avalia se uma aplicação permite que uma única conta de utilizador mantenha múltiplas sessões ativas simultâneas em diferentes navegadores, dispositivos ou endereços IP. A falha em restringir sessões simultâneas aumenta a janela de oportunidade para atacantes utilizarem tokens de sessão sequestrados (hijacked session tokens) ou credenciais comprometidas sem alertar o utilizador legítimo ou terminar as sessões existentes. Este comportamento é tipicamente avaliado através da autenticação a partir de múltiplas fontes e da monitorização da estabilidade da sessão, revelando frequentemente fragilidades na lógica de Session Management ou a ausência de regras de negócio focadas em segurança. Do ponto de vista de um atacante, a falta de controlo de concorrência permite a persistência furtiva após um comprometimento inicial e facilita ataques automatizados paralelos.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### As sessões simultâneas são limitadas pela política da aplicação?
- [ ] Sim — apenas uma sessão **é permitida** por utilizador; novos logins invalidam os antigos *(Mais seguro)*  
- [ ] Sim — um número fixo de sessões simultâneas **é imposto** e não pode ser excedido  
- [ ] Não — um número ilimitado de sessões simultâneas **é possível**  

### Como a aplicação responde quando o limite de sessões é atingido?
- [ ] A sessão ativa mais antiga **é automaticamente terminada** (mitigação de session fixation/hijacking)  
- [ ] O utilizador **é solicitado** a terminar as sessões existentes antes que a nova sessão seja autorizada  
- [ ] A nova tentativa de login **é bloqueada** até que ocorra um logout manual noutro dispositivo  
- [ ] Nenhuma ação é tomada — múltiplas sessões permanecem **ativadas** simultaneamente  

### A aplicação fornece uma interface de gestão de sessões para o utilizador?
- [ ] Sim — os utilizadores **podem** visualizar sessões ativas (IP, dispositivo, hora) e terminá-las remotamente  
- [ ] Sim — os utilizadores **podem** visualizar sessões ativas, mas **não podem** terminá-las remotamente  
- [ ] Não — os utilizadores **não podem** ver ou gerir outras sessões ativas associadas à sua conta  

### O utilizador é notificado quando é detetada atividade de login simultânea?
- [ ] Sim — um alerta (e-mail, SMS ou in-app) **é acionado** para logins de novos dispositivos ou localizações  
- [ ] Não — nenhuma notificação **é fornecida** quando sessões simultâneas são estabelecidas  

---