## WSTG-CLNT-12 — Test Browser Storage

O teste de armazenamento do navegador envolve analisar como uma aplicação utiliza mecanismos no lado do cliente (client-side), tais como LocalStorage, SessionStorage, IndexedDB e WebSQL para persistir dados. Informações sensíveis armazenadas de forma insegura — incluindo PII, tokens de autenticação ou identificadores de sessão — podem ser comprometidas se um atacante realizar um Cross-Site Scripting (XSS) ou obtiver acesso físico ao dispositivo do usuário. Pentesters devem determinar se os dados são armazenados em texto claro (cleartext), se permanecem acessíveis após o logout e se o ciclo de vida do armazenamento é gerenciado adequadamente para evitar vazamento de dados. Do ponto de vista de um atacante, esses mecanismos de armazenamento são alvos lucrativos para a exfiltração de informações de manutenção de estado que permitem o Session Hijacking ou a persistência de conta a longo prazo.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Médio / Alto* |

> *A severidade torna-se Alta se tokens de sessão ou PII sensíveis forem armazenados no LocalStorage e estiverem acessíveis via XSS.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### A aplicação armazena informações sensíveis no armazenamento do navegador?
- [ ] Não — a aplicação **não** utiliza armazenamento do navegador ou apenas armazena estados de UI não sensíveis  
- [ ] Sim — dados sensíveis são armazenados, mas **estão criptografados** utilizando criptografia robusta no lado do cliente  
- [ ] Sim — dados sensíveis (tokens, PII, segredos) são armazenados em **texto claro (cleartext)** *(Médio)*  

### Os dados armazenados estão acessíveis a scripts não autorizados (risco de XSS)?
- [ ] Não — os dados são armazenados em cookies HttpOnly (não no armazenamento do navegador) ou o armazenamento **não é utilizado**  
- [ ] Sim — LocalStorage/SessionStorage é utilizado, tornando os dados **completamente acessíveis** através de qualquer payload de XSS *(Alto)*  

### O armazenamento do navegador é limpo adequadamente após o logout do usuário?
- [ ] Sim — todo o armazenamento relacionado à aplicação é **explicitamente limpo** durante o processo de logout  
- [ ] Não — o armazenamento persiste após o logout, mas **não** contém dados de sessão sensíveis  
- [ ] Não — tokens de autenticação ou PII **persistem** no armazenamento após o encerramento da sessão *(Médio)*  

### A aplicação utiliza IndexedDB ou WebSQL para conjuntos de dados grandes/sensíveis?
- [ ] Não — esses mecanismos de armazenamento **não são utilizados**  
- [ ] Sim — os dados são armazenados e **devidamente sanitizados** antes de serem renderizados no DOM  
- [ ] Sim — conjuntos de dados sensíveis são armazenados em **texto claro (cleartext)** dentro de estruturas IndexedDB/WebSQL  

### A integridade dos dados armazenados pode ser manipulada para afetar a lógica da aplicação?
- [ ] Não — a aplicação valida ou assina os dados antes de processá-los a partir do armazenamento  
- [ ] Sim — a modificação de valores de armazenamento (ex: funções de usuário, flags) **é possível** e resulta em comportamento alterado da aplicação  

---