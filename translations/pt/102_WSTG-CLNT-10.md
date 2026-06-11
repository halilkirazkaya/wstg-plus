## WSTG-CLNT-10 — Testes de WebSockets

Os testes de WebSocket concentram-se no canal de comunicação full-duplex estabelecido entre o cliente e o servidor, o qual ignora os padrões tradicionais de requisição-resposta HTTP. Pentesters avaliam a segurança do processo de handshake, a presença de proteções contra Cross-Site WebSocket Hijacking (CSWSH) e o rigor da validação de entrada aplicada aos payloads de mensagens. A exploração normalmente envolve o sequestro (hijacking) de uma sessão ativa para exfiltrar dados em tempo real ou a injeção de payloads maliciosos que acionam vulnerabilidades do lado do servidor, como Command Injection ou SQLi através do socket persistente. Como muitos Web Application Firewalls (WAFs) tradicionais não inspecionam o tráfego WebSocket de forma eficaz, este protocolo frequentemente fornece um vetor furtivo para contornar defesas de perímetro.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Ferramentas:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### A conexão WebSocket é estabelecida através de um canal seguro?
- [ ] Sim — `wss://` (WebSocket Secure) é obrigatório para todas as conexões  
- [ ] Não — `ws://` é utilizado, permitindo a interceptação por person-in-the-middle (PITM)  

### A aplicação está protegida contra Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] Não — o servidor valida estritamente o cabeçalho `Origin` durante o handshake  
- [ ] Sim — a validação do cabeçalho `Origin` é fraca ou **pode** ser contornada (bypass) através de origens nulas ou forjadas (spoofed)  
- [ ] Sim — nenhuma validação do cabeçalho `Origin` é realizada, permitindo que atacantes cross-site iniciem uma conexão *(Crítico)*  

### A validação e sanitização de entrada são aplicadas aos payloads das mensagens WebSocket?
- [ ] Sim — todas as mensagens de entrada são sanitizadas e validadas contra um esquema (schema)  
- [ ] Sim — existe alguma validação, mas o bypass **é possível** utilizando payloads codificados ou não padronizados  
- [ ] Não — as mensagens são processadas diretamente, permitindo injeção (XSS, SQLi, RCE) ou manipulação lógica  

### As verificações de autenticação e autorização são realizadas em cada mensagem WebSocket?
- [ ] Sim — a sessão é verificada para cada mensagem ou o protocolo é stateless com tokens  
- [ ] Sim — a autenticação ocorre no handshake, mas a autorização para ações específicas **não é** aplicada por mensagem  
- [ ] Não — a autenticação é verificada apenas no handshake, permitindo que o sequestro de sessão (session hijacking) conceda acesso total  

### O servidor implementa rate limiting ou restrições de recursos no WebSocket?
- [ ] Sim — rate limiting e tamanhos máximos de mensagem estão **habilitados** e são aplicados  
- [ ] Não — não existem limites, permitindo Denial of Service (DoS) via flooding de mensagens ou tamanhos de payload elevados  

---