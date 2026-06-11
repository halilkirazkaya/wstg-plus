## WSTG-INPV-07 — XML Injection

A XML Injection ocorre quando uma aplicação incorpora dados fornecidos pelo usuário em um documento ou stream XML sem a devida validação, sanitização ou escape de metacaracteres XML. Ao injetar tags XML específicas ou elementos estruturais, um atacante pode manipular a lógica do documento, contornar mecanismos de autenticação ou interferir na integridade dos dados processados pelo backend. Esta vulnerabilidade é comumente encontrada em web services baseados em SOAP, APIs REST que aceitam XML, asserções SAML e aplicações que geram arquivos de configuração XML ou relatórios dinamicamente. Do ponto de vista de um atacante, o objetivo é sair do contexto de dados pretendido para alterar a estrutura do documento, potencialmente levando à escalada de privilégios não autorizada ou à execução de lógica de negócios não pretendida.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Ferramentas:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### A aplicação aceita e processa entrada formatada em XML vinda do usuário?
- [ ] Não — a aplicação **não** processa entrada XML  
- [ ] Sim — a entrada XML é processada via APIs (SOAP/REST)  
- [ ] Sim — o XML é processado via uploads de arquivos (SVG, DOCX, etc.)  

### Os metacaracteres XML (ex: `<`, `>`, `&`, `'`, `"`) são devidamente escapados ou sanitizados?
- [ ] Sim — todos os metacaracteres são **devidamente** escapados ou sanitizados *(Mais seguro)*  
- [ ] Sim — algum filtro é aplicado, mas o bypass **é possível** utilizando encoding  
- [ ] Não — a entrada bruta é inserida diretamente em estruturas XML **sem** sanitização  

### A validação de esquema no lado do servidor (XSD/DTD) é aplicada para todas as entradas XML?
- [ ] Sim — a validação estrita de esquema está **ativada** e impede mudanças estruturais  
- [ ] Sim — a validação está **ativada**, mas **não** impede a injeção de tags dentro de campos de dados  
- [ ] Não — a validação de esquema está **desativada** ou não foi implementada  

### Um atacante consegue injetar novas tags XML para alterar a estrutura ou lógica do documento?
- [ ] Não — a estrutura permanece intacta independentemente da entrada  
- [ ] Sim — tags podem ser injetadas, mas **não podem** alterar a lógica de negócios  
- [ ] Sim — a injeção de tags **é possível** e permite o bypass de lógica ou modificação de dados *(Crítico)*  

### O parser XML pode ser manipulado utilizando seções CDATA ou comentários XML?
- [ ] Não — o parser trata corretamente ou rejeita marcadores de CDATA/comentários  
- [ ] Sim — a injeção de `<![CDATA[...]]>` ou `<!-- ... -->` **é possível** para ocultar ou burlar filtros  

---