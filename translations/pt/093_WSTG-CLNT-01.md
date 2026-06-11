## WSTG-CLNT-01 — Teste para Cross-Site Scripting Baseado em DOM (DOM XSS)

O Cross-Site Scripting baseado em DOM (DOM XSS) ocorre quando uma aplicação contém JavaScript no lado do cliente (client-side) que processa dados de uma fonte não confiável de maneira insegura, geralmente escrevendo os dados de volta no Document Object Model (DOM) através de um sink perigoso. Diferente do XSS refletido ou armazenado, a vulnerabilidade existe inteiramente no código do lado do cliente; o Payload muitas vezes não é enviado ao servidor, pois é processado por sinks como `innerHTML`, `eval()` ou `document.write()`. Atacantes exploram isso criando URLs com fragmentos ou parâmetros maliciosos que, quando carregados pelo navegador da vítima, executam JavaScript arbitrário no contexto da sessão do usuário. Isso pode levar à exfiltração de tokens de sessão, roubo de dados sensíveis e ações não autorizadas realizadas em nome do usuário autenticado.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Ferramentas:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Fontes não confiáveis são utilizadas para capturar dados controlados pelo usuário no JavaScript?
- [ ] Não — O JavaScript **não** utiliza `location.hash`, `location.search` ou `document.referrer`  
- [ ] Sim — Fontes não confiáveis são utilizadas, mas os dados **não** são passados para nenhum sink de execução  
- [ ] Sim — Fontes não confiáveis são utilizadas e os dados fluem diretamente para a lógica do lado do cliente  

### Dados controlados pelo usuário são passados para sinks perigosos do DOM?
- [ ] Não — Sinks como `innerHTML`, `outerHTML`, `document.write()` ou `eval()` **não** são utilizados  
- [ ] Sim — Sinks perigosos estão presentes, mas os dados são rigorosamente **sanitizados** utilizando uma biblioteca segura como o DOMPurify  
- [ ] Sim — Sinks perigosos estão presentes e os dados são processados com filtragem **insuficiente** ou baseada em regex customizada  
- [ ] Sim — Sinks perigosos estão presentes e os dados são passados **sem** qualquer sanitização ou codificação (Encoding) *(Crítico)*  

### O sink de execução pode ser alcançado através de um payload baseado em URL?
- [ ] Não — O fluxo da fonte (source) para o sink **não** pode ser disparado via entrada externa  
- [ ] Sim — O fluxo é **possível**, mas requer interação complexa do usuário  
- [ ] Sim — O fluxo é **possível** e pode ser disparado via um fragmento de URL simples ou parâmetro de consulta (query parameter)  

### Cabeçalhos de segurança modernos ou mitigações baseadas no navegador estão neutralizando o risco?
- [ ] Sim — Uma `Content-Security-Policy` (CSP) estrita com `script-src` e `trusted-types` está **habilitada** e impede a execução  
- [ ] Sim — Uma CSP está presente, mas `unsafe-inline` ou hashes fracos tornam um bypass **possível**  
- [ ] Não — Nenhuma CSP está presente para mitigar a execução baseada em DOM  

### A aplicação utiliza frameworks de front-end com proteções integradas?
- [ ] Sim — O framework (ex: Angular, React) codifica automaticamente os dados e o bypass **não é possível**  
- [ ] Sim — O framework é utilizado, mas "escape hatches" como `dangerouslySetInnerHTML` estão **habilitados**  
- [ ] Não — A aplicação utiliza JavaScript puro (vanilla) ou bibliotecas legadas (ex: versões antigas do jQuery) sem auto-escaping  

---