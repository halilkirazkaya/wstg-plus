## WSTG-INPV-10 — IMAP SMTP Injection

As vulnerabilidades de injeção IMAP e SMTP surgem quando uma aplicação web filtra incorretamente os dados fornecidos pelo utilizador antes de os incorporar em comandos enviados para um servidor de correio. Ao injetar caracteres de Carriage Return e Line Feed (CRLF), um atacante pode terminar o comando pretendido e anexar instruções de correio arbitrárias, tais como a adição de destinatários adicionais (CC/BCC) ou a modificação do corpo da mensagem. Estas falhas são frequentemente encontradas em gateways web-to-mail, formulários de contacto e clientes de e-mail que comunicam diretamente com servidores de correio através de protocolos como IMAP4 ou SMTP. A exploração bem-sucedida permite o relay não autorizado de spam, a exfiltração de conteúdos de e-mail sensíveis e, em alguns casos, o comprometimento total da integridade do servidor de correio.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Estado do Teste** | Não Realizado |
| **Gravidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### A aplicação processa entradas fornecidas pelo utilizador para construir comandos IMAP ou SMTP?
- [ ] Não — a aplicação **não** interage com servidores de correio através de inputs do utilizador  
- [ ] Sim — a aplicação interage com servidores de correio, mas utiliza uma API ou biblioteca segura  
- [ ] Sim — a aplicação constrói comandos de correio puros (raw) utilizando parâmetros controlados pelo utilizador  

### A entrada é validada para prevenir a injeção de sequências de Carriage Return (`\r`) e Line Feed (`\n`)?
- [ ] Sim — os caracteres CRLF são **estritamente** filtrados, codificados ou rejeitados  
- [ ] Sim — a entrada é validada contra uma allowlist rigorosa de caracteres  
- [ ] Não — os caracteres CRLF **não** são filtrados, permitindo a terminação e injeção de comandos  

### Pode um atacante injetar cabeçalhos de correio adicionais, tais como `Bcc:` ou `Cc:`?
- [ ] Não — a modificação de cabeçalhos **não é possível** devido a restrições rigorosas de input  
- [ ] Sim — a injeção de cabeçalhos adicionais **é possível** através de sequências CRLF  
- [ ] Sim — o relay de e-mail para domínios externos **está ativado** e é explorável *(Crítico)*  

### A aplicação permite a manipulação de comandos IMAP para aceder a caixas de correio não autorizadas?
- [ ] Não — a aplicação **não** utiliza IMAP ou limita estritamente o acesso à caixa de correio ao utilizador autenticado  
- [ ] Sim — a injeção de comandos IMAP **é possível**, mas limitada à enumeração de metadados  
- [ ] Sim — a injeção de comandos IMAP permite a leitura ou eliminação de e-mails de outros utilizadores  

### O servidor de correio de backend é vulnerável a pipelining de comandos?
- [ ] Não — o servidor de correio rejeita comandos em lote (batched) ou múltiplos comandos numa única sessão  
- [ ] Sim — o servidor de correio processa múltiplos comandos injetados através de um único campo de entrada  

---