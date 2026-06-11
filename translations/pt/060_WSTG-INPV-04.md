## WSTG-INPV-04 — Testing for HTTP Parameter Pollution

A HTTP Parameter Pollution (HPP) ocorre quando uma aplicação recebe múltiplos parâmetros HTTP com o mesmo nome e os processa de maneira inconsistente ou insegura. Ao fornecer parâmetros duplicados, um atacante pode manipular a lógica interna da aplicação, potencialmente contornando Web Application Firewalls (WAF), filtros de validação de entrada ou mecanismos de controle de acesso. Esta vulnerabilidade é altamente dependente do servidor web ou framework da aplicação subjacente, que pode escolher a primeira, a última ou uma versão concatenada dos parâmetros repetidos. A exploração geralmente visa endpoints que passam parâmetros para APIs de back-end, consultas a bancos de dados ou mecanismos de redirecionamento de URL, resultando em impactos que variam de Cross-Site Scripting (XSS) até a exfiltração não autorizada de dados.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Como a aplicação lida com múltiplos parâmetros HTTP com o mesmo nome?
- [ ] Não — parâmetros duplicados são **rejeitados** ou ignorados pelo servidor  
- [ ] Sim — o servidor seleciona consistentemente apenas a **primeira** ou a **última** ocorrência  
- [ ] Sim — o servidor **concatena** valores, permitindo potencialmente injeções  

### A HPP pode ser utilizada para contornar controles de segurança, como um WAF ou filtro de entrada?
- [ ] Não — os controles de segurança inspecionam corretamente **todas** as ocorrências de um parâmetro  
- [ ] Sim — um WAF ou filtro inspeciona apenas a **primeira** ocorrência, permitindo que uma **segunda** ocorrência maliciosa passe  
- [ ] Sim — a concatenação permite que um atacante **divida** um Payload entre múltiplos parâmetros para evitar a detecção  

### A aplicação reflete parâmetros poluídos na resposta sem a devida sanitização?
- [ ] Não — os parâmetros são sanitizados ou codificados antes de serem refletidos na UI  
- [ ] Sim — a poluição resulta em conteúdo refletido, mas a sanitização **é aplicada**  
- [ ] Sim — a poluição resulta em conteúdo refletido e a sanitização **não é aplicada**, levando a XSS ou erros de lógica  

### Os parâmetros passados para chamadas de API internas ou sistemas de back-end são vulneráveis à poluição?
- [ ] Não — as requisições de back-end utilizam métodos de construção seguros e não dinâmicos  
- [ ] Sim — a HPP permite que um atacante **injete** ou **sobrescreva** parâmetros na estrutura da requisição de back-end  

### A aplicação apresenta comportamento diferente quando os parâmetros são poluídos em requisições GET versus POST?
- [ ] Não — o comportamento é consistente em todos os métodos HTTP  
- [ ] Sim — apenas parâmetros GET são vulneráveis à poluição  
- [ ] Sim — apenas parâmetros POST (corpo) são vulneráveis à poluição  

---