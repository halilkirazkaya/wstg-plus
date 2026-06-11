## WSTG-CLNT-02 — Teste de Execução de JavaScript

O teste de execução de JavaScript foca na identificação de instâncias onde uma aplicação lida incorretamente com dados fornecidos pelo usuário, levando à execução de scripts não autorizados no contexto do navegador da vítima. Esta vulnerabilidade normalmente resulta em Cross-Site Scripting (XSS), o qual permite que atacantes exfiltrem cookies de sessão, manipulem o Document Object Model (DOM) ou realizem ações em nome do usuário. Pentesters examinam sinks (sumidouros) como `innerHTML`, `document.write()` e `eval()` onde dados não sanitizados de sources (origens) como fragmentos de URL, `localStorage` ou `window.name` podem ser processados. A exploração bem-sucedida compromete a integridade e a confidencialidade da sessão do usuário e pode servir como um pivô para ataques client-side mais complexos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Ferramentas:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### A aplicação processa dados fornecidos pelo usuário em sinks de JavaScript perigosos?
- [ ] Não — todos os dados são sanitizados ou utilizados em sinks seguros como `textContent`  
- [ ] Sim — os dados são processados em sinks perigosos, mas a sanitização/codificação **é aplicada**  
- [ ] Sim — os dados são processados em sinks perigosos e a sanitização **não é aplicada**  

### Uma Content Security Policy (CSP) está presente para mitigar a execução de scripts?
- [ ] Sim — uma CSP restritiva está **habilitada** e o bypass **não é possível**  
- [ ] Sim — uma CSP está **habilitada**, mas o bypass **é possível** via `unsafe-inline` ou CDNs inseguras  
- [ ] Não — nenhuma CSP está **habilitada**  

### O JavaScript pode ser executado via parâmetros de URL ou fragmentos (DOM-based XSS)?
- [ ] Não — a entrada é devidamente codificada e a execução **não é possível**  
- [ ] Sim — a entrada é refletida no DOM, mas a execução **é prevenida** por proteções de frameworks modernos  
- [ ] Sim — a entrada é executada diretamente no navegador, e a exploração **é possível**  

### Os manipuladores de eventos (event handlers) ou esquemas de URI são vulneráveis à injeção de script?
- [ ] Não — a entrada é sanitizada para remover atributos de eventos e esquemas `javascript:`  
- [ ] Sim — manipuladores de eventos **podem** ser injetados, mas filtros limitam a complexidade do payload  
- [ ] Sim — a execução arbitrária de JavaScript via atributos `onerror`, `onload` ou `href` **é possível**  

---