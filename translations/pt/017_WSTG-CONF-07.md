## WSTG-CONF-07 — Testar HTTP Strict Transport Security

O HTTP Strict Transport Security (HSTS) é um mecanismo de segurança que permite que um servidor web informe aos navegadores que ele só deve ser acessado via HTTPS, prevenindo ataques de downgrade de protocolo e sequestro de cookies (cookie hijacking). Ao forçar uma conexão criptografada, o HSTS mitiga ataques de Man-in-the-Middle (MitM), como o SSLStrip, onde um invasor intercepta uma requisição HTTP inicial insegura para impedir a transição para TLS. Do ponto de vista de um atacante, a ausência deste cabeçalho ou uma duração curta de `max-age` fornece uma janela de oportunidade para interceptar tokens de sessão sensíveis ou credenciais durante o handshake inicial em texto claro. Este teste foca em verificar a presença, duração e escopo do cabeçalho `Strict-Transport-Security` em todos os endpoints sensíveis da aplicação.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média* |

> *A severidade torna-se Média se a aplicação manipular PII, tokens de sessão ou credenciais, pois a falta de HSTS aumenta a taxa de sucesso de ataques MitM.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### O cabeçalho `Strict-Transport-Security` está presente nas respostas HTTPS?
- [ ] Sim — o cabeçalho **está presente** em todos os endpoints sensíveis  
- [ ] Sim — o cabeçalho **está presente**, mas apenas na página inicial (landing page)  
- [ ] Não — o cabeçalho **está ausente** em toda a aplicação  

### A diretiva `max-age` está configurada com uma duração suficiente?
- [ ] Sim — `max-age` está configurado para um ano ou mais (ex: `31536000`)  
- [ ] Sim — `max-age` está configurado, mas é **muito curto** (ex: menos de 6 meses)  
- [ ] Não — a diretiva `max-age` **está ausente** ou configurada como `0` *(Crítico)*  

### A política HSTS cobre todos os subdomínios?
- [ ] Sim — a diretiva `includeSubDomains` **está aplicada**  
- [ ] Não — a diretiva `includeSubDomains` **está ausente**, deixando os subdomínios vulneráveis a downgrade  
- [ ] Não — a funcionalidade não se aplica, pois não existem subdomínios  

### A aplicação está registrada para HSTS Preloading?
- [ ] Sim — a diretiva `preload` **está presente** e o domínio está na lista de preload de HSTS  
- [ ] Sim — a diretiva `preload` **está presente**, mas o domínio **ainda não** está na lista de preload  
- [ ] Não — a diretiva `preload` **está ausente**, permitindo uma janela para MitM na primeira visita  

### O cabeçalho HSTS é enviado incorretamente via HTTP em texto claro?
- [ ] Não — o cabeçalho é enviado **apenas** através de conexões HTTPS seguras  
- [ ] Sim — o cabeçalho **é enviado** via HTTP, o que é ignorado pelos navegadores e pode vazar detalhes de configuração  

---