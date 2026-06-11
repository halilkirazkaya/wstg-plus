## WSTG-SESS-10 — Teste de JSON Web Tokens

JSON Web Tokens (JWTs) são comumente utilizados para gerenciamento de sessão stateless e propagação de identidade, mas sua segurança depende inteiramente da implementação correta de algoritmos de assinatura e da verificação rigorosa de claims. Atacantes frequentemente tentam burlar a autenticação explorando falhas no algoritmo "none", realizando Brute Force em chaves secretas HS256 fracas ou executando ataques de troca de algoritmo (algorithm-switching), onde uma chave pública assimétrica é utilizada como um segredo HMAC simétrico. Essas vulnerabilidades geralmente se manifestam em cookies de sessão ou cabeçalhos de Authorization, permitindo potencialmente que um atacante realize escalonamento de privilégios ou personifique qualquer usuário ao modificar claims como `role` ou `user_id` sem invalidar a integridade criptográfica do token.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Ferramentas:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### O algoritmo "none" é aceito pelo servidor?
- [ ] Não — o servidor **rejeita** tokens utilizando `alg: none` no cabeçalho  
- [ ] Sim — o servidor **aceita** tokens com `alg: none` e uma assinatura vazia *(Crítico)*  

### Como a assinatura do JWT é verificada e protegida contra Brute Force?
- [ ] Sim — são utilizadas chaves assimétricas fortes (RS256/ES256) ou segredos HMAC de alta entropia  
- [ ] Sim — o HS256 é utilizado, mas o segredo **pode** sofrer Brute Force devido à baixa entropia ou valores padrão  
- [ ] Não — a verificação de assinatura **não é aplicada** ou **pode** ser burlada via troca de algoritmo (RS256 para HS256)  

### As claims padrão (exp, nbf, iat) são aplicadas rigorosamente pelo backend?
- [ ] Sim — o `exp` (expiração) está presente e **não pode** ser burlado  
- [ ] Sim — o `exp` está presente, mas o servidor **não** o aplica durante a validação  
- [ ] Não — as claims de expiração estão **ausentes** ou são ignoradas, permitindo o Session Replay indefinido  

### A implementação impede a injeção de chaves baseada em cabeçalho (kid, jku, x5u)?
- [ ] Sim — os cabeçalhos são sanitizados e apenas chaves/fontes internas confiáveis são **permitidas**  
- [ ] Não — o cabeçalho `kid` (Key ID) está vulnerável a Directory Traversal ou SQL Injection para apontar para um arquivo conhecido  
- [ ] Não — os cabeçalhos `jku` ou `x5u` **podem** ser apontados para URLs controladas pelo atacante para carregar chaves maliciosas  

---