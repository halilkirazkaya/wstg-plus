## WSTG-ATHN-05 — Teste para "Lembrar Senha" Vulnerável

A funcionalidade "Lembrar-me" ou login persistente é projetada para manter o estado autenticado de um usuário após o reinício do navegador, armazenando um token ou credencial no lado do cliente. Se esse recurso for mal implementado — como o armazenamento de senhas em texto claro (plaintext), hashes fracos ou tokens de baixa entropia em cookies — ele expõe o usuário ao account takeover por meio de Cross-Site Scripting (XSS), acesso físico ou interceptação de rede. Pentesters avaliam a entropia desses tokens de persistência, as flags de segurança aplicadas ao mecanismo de armazenamento e o ciclo de vida da sessão para garantir que a conveniência do acesso persistente não ignore controles de segurança críticos, como autenticação de múltiplos fatores (MFA) ou invalidação de sessão. A exploração geralmente envolve a coleta desses tokens de longa duração para obter acesso não autorizado a contas sem exigir as credenciais originais.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Médio / Alto* |

> *A severidade torna-se Alta se credenciais em texto claro ou hashes sem salt forem armazenados no cookie persistente, ou se o token permitir o bypass de Autenticação de Múltiplos Fatores (MFA).

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Ferramentas:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### O aplicativo implementa uma funcionalidade "Lembrar-me" ou "Manter Conectado"?
- [ ] Não — o recurso **não** existe no aplicativo  
- [ ] Sim — o recurso existe e utiliza tokens criptograficamente fortes e não previsíveis  
- [ ] Sim — o recurso existe, mas utiliza tokens previsíveis ou de baixa entropia *(Médio)*  
- [ ] Sim — o recurso existe e armazena credenciais em texto claro (plaintext) ou senhas codificadas em base64 *(Alto)*  

### As flags de segurança estão aplicadas corretamente ao cookie persistente?
- [ ] Sim — as flags `HttpOnly`, `Secure` e `SameSite` estão **aplicadas**  
- [ ] Sim — algumas flags estão aplicadas, mas a `HttpOnly` está **ausente**  
- [ ] Sim — algumas flags estão aplicadas, mas a `Secure` está **ausente** em conexões HTTPS  
- [ ] Não — nenhuma flag de segurança está **aplicada** ao cookie persistente  

### O token de "Lembrar-me" permanece válido após um logout manual?
- [ ] Não — o token é invalidado no lado do servidor imediatamente após o logout  
- [ ] Sim — o token permanece válido, mas exige uma senha para ações sensíveis  
- [ ] Sim — o token permanece válido e permite acesso total à conta **sem** reautenticação *(Médio)*  

### A sessão persistente é encerrada após uma alteração de senha?
- [ ] Sim — todos os tokens persistentes são revogados quando o usuário altera sua senha  
- [ ] Não — o token de "Lembrar-me" permanece válido após uma redefinição ou alteração de senha **ser realizada** *(Alto)*  

### O token persistente ignora a Autenticação de Múltiplos Fatores (MFA) em visitas subsequentes?
- [ ] Não — o MFA é exigido mesmo quando um token de "Lembrar-me" está presente  
- [ ] Sim — o MFA é ignorado, mas o token está vinculado a um fingerprint de dispositivo específico  
- [ ] Sim — o MFA é **totalmente ignorado** usando apenas o cookie persistente de qualquer dispositivo *(Alto)*  

---