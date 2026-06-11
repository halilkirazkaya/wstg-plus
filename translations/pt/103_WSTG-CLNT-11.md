## WSTG-CLNT-11 — Testar Web Messaging

O Web Messaging, ou `postMessage`, permite a comunicação cross-origin entre objetos window, como uma página pai e um popup gerado ou um iframe incorporado. Este mecanismo é fundamental para a funcionalidade da web moderna, mas introduz riscos significativos se a aplicação receptora falhar em verificar a identidade do remetente ou manipular incorretamente o payload da mensagem. Atacantes exploram essas vulnerabilidades hospedando um site malicioso que incorpora a aplicação alvo e envia mensagens manipuladas para disparar ações não intencionais, exfiltrar dados sensíveis ou executar Cross-Site Scripting (XSS) baseado em DOM. A exploração bem-sucedida ocorre quando os listeners de mensagens não validam estritamente a propriedade `origin` ou passam o `message.data` diretamente para sinks de execução perigosos.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Ferramentas:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### A aplicação utiliza a API `postMessage` para comunicação entre documentos?
- [ ] Não — listeners de `postMessage` **não** estão presentes no código client-side  
- [ ] Sim — o `postMessage` é utilizado para comunicação interna de componentes  
- [ ] Sim — o `postMessage` é utilizado para comunicar com domínios de terceiros ou widgets  

### A origem das mensagens recebidas é estritamente validada?
- [ ] Sim — a origem é verificada em relação a uma whitelist estrita usando `===` ou comparação idêntica *(Mais seguro)*  
- [ ] Sim — a origem é validada usando regex, mas a lógica **é** suscetível a bypass (ex: `^https://trusted.com`)  
- [ ] Não — a propriedade `origin` **não** é verificada, permitindo que qualquer domínio envie mensagens  
- [ ] Não — um wildcard `*` é usado no `targetOrigin` do remetente, potencialmente vazando dados para listeners maliciosos  

### O payload da mensagem é sanitizado antes de ser processado pela aplicação?
- [ ] Sim — os dados são tratados como texto simples e nenhum sink perigoso é utilizado  
- [ ] Sim — os dados são analisados (ex: `JSON.parse`) e validados antes do uso, e o bypass **não é possível**  
- [ ] Não — os dados da mensagem são passados diretamente para sinks perigosos como `innerHTML`, `eval()` ou `setTimeout()`  
- [ ] Não — os dados da mensagem são usados para navegar na janela ou alterar o `location.href` sem validação  

### Um atacante pode disparar ações não autorizadas ou exfiltrar dados através de uma mensagem maliciosa?
- [ ] Não — mesmo com uma mensagem forjada, nenhuma ação ou dado sensível pode ser acessado  
- [ ] Sim — uma mensagem manipulada **pode** disparar mudanças de estado sensíveis (ex: alteração de senha, atualização de perfil)  
- [ ] Sim — uma mensagem manipulada **pode** resultar em Cross-Site Scripting (XSS) baseado em DOM ou exfiltração de tokens CSRF/dados de sessão  

---