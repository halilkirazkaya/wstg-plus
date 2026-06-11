## WSTG-ATHN-03 — Teste de Mecanismo de Bloqueio Fraco

Um mecanismo de bloqueio de conta (account lockout) é um controle crítico de defesa em profundidade (defense-in-depth) projetado para mitigar ataques de força bruta (brute-force) e credential stuffing, desativando uma conta após um número especificado de tentativas de autenticação malsucedidas. Sem uma política de bloqueio robusta, atacantes podem usar ferramentas automatizadas para adivinhar senhas sistematicamente em múltiplas contas (password spraying) ou visar uma única conta de alto valor até obter sucesso. Esta vulnerabilidade geralmente se manifesta em portais de login, formulários de redefinição de senha e endpoints de API que carecem de limitação de taxa (rate limiting) ou proteção ao nível de conta. Do ponto de vista de um atacante, um mecanismo de bloqueio fraco ou ausente permite tentativas de senha infinitas, enquanto um excessivamente agressivo pode ser explorado para causar uma Negação de Serviço (DoS) distribuída contra usuários legítimos ao bloquear intencionalmente suas contas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a aplicação lida com PII (informações de identificação pessoal) sensíveis, dados financeiros, ou se contas administrativas forem suscetíveis a brute-force sem quaisquer controles secundários.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Ferramentas:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### O mecanismo de bloqueio de conta é aplicado após um número definido de tentativas falhas?
- [ ] Sim — a conta é bloqueada após um limite definido e razoável (ex: 5-10 tentativas)  
- [ ] Sim — a conta é bloqueada, mas apenas após um número excessivamente alto de tentativas  
- [ ] Não — a conta **não pode** ser bloqueada e permite brute-force indefinido  

### O mecanismo de bloqueio pode ser burlado usando técnicas comuns de evasão?
- [ ] Não — o bloqueio é aplicado no lado do servidor (server-side) e **não** é afetado por alterações no lado do cliente (client-side)  
- [ ] Sim — **é possível** burlar o bloqueio rotacionando endereços IP ou usando um pool de proxies  
- [ ] Sim — **é possível** burlar o bloqueio manipulando cabeçalhos HTTP como `X-Forwarded-For` or `User-Agent`  
- [ ] Sim — **é possível** burlar o bloqueio limpando cookies ou iniciando novas sessões  

### O mecanismo de bloqueio fornece um caminho para recuperação de conta ou expiração?
- [ ] Sim — a conta é desbloqueada automaticamente após uma duração definida ou via verificação por e-mail  
- [ ] Sim — a conta requer intervenção administrativa para desbloqueio  
- [ ] Não — o bloqueio é permanente sem um caminho de recuperação claro, aumentando o risco de DoS  

### A aplicação diferencia entre um nome de usuário válido e inválido durante o bloqueio?
- [ ] Não — a resposta da aplicação é idêntica tanto para usuários existentes quanto para inexistentes  
- [ ] Sim — a aplicação revela se uma conta está bloqueada, permitindo a **enumeração de usuários** (username enumeration)  

### O mecanismo de bloqueio é suscetível a um ataque de Negação de Serviço (DoS)?
- [ ] Não — controles como CAPTCHA ou rate limiting baseado em IP previnem o bloqueio massivo de contas  
- [ ] Sim — um atacante **pode** bloquear sistematicamente todos os usuários conhecidos fornecendo senhas incorretas  

---