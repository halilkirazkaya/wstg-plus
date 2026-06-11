## WSTG-INPV-09 — Testing for XPath Injection

A XPath Injection ocorre quando uma aplicação utiliza informações fornecidas pelo usuário para construir uma consulta XPath para dados XML, permitindo que um invasor interfira na lógica da consulta. Ao injetar sintaxes específicas, como aspas simples, colchetes e operadores lógicos, um invasor pode navegar pela estrutura do documento XML, burlar a autenticação ou exfiltrar nós de dados sensíveis. Esta vulnerabilidade é comumente encontrada em web services, interfaces de gerenciamento de configuração e sistemas legados que dependem de armazenamentos de dados baseados em XML em vez de bancos de dados SQL tradicionais. Do ponto de vista de um atacante, isso é frequentemente explorado usando técnicas de boolean-based ou error-based para mapear sistematicamente a árvore XML e recuperar valores ocultos do backend.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Ferramentas:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### As entradas do usuário utilizadas para construir consultas XPath são devidamente sanitizadas ou parametrizadas?
- [ ] Sim — as entradas **não** são usadas em consultas XPath ou são estritamente parametrizadas  
- [ ] Sim — uma validação de entrada/codificação robusta **é aplicada** e o bypass **não é possível**  
- [ ] Sim — alguma validação **é aplicada**, mas o bypass **é possível** utilizando caracteres codificados  
- [ ] Não — a entrada do usuário é diretamente concatenada em consultas XPath *(Crítico)*  

### A aplicação revela detalhes estruturais de XML/XPath por meio de mensagens de erro?
- [ ] Não — páginas de erro genéricas são exibidas e **nenhum** detalhe de XML é vazado  
- [ ] Sim — as mensagens de erro revelam a presença de processamento XML, mas **sem** detalhes do caminho (path)  
- [ ] Sim — mensagens de erro detalhadas revelam a sintaxe XPath, nomes de nós ou caminhos de arquivos  

### É possível burlar a lógica de autenticação ou autorização utilizando a lógica XPath?
- [ ] Não — a lógica de autenticação **não** depende de XML/XPath ou está implementada de forma segura  
- [ ] Sim — o login ou as verificações de permissão podem ser burlados utilizando payloads baseados em lógica como `' or 1=1 or '`  

### A estrutura do documento XML ou os dados podem ser enumerados via blind injection?
- [ ] Não — a aplicação **não** é suscetível à inferência baseada em booleanos ou baseada em tempo (time-based)  
- [ ] Sim — nomes de nós e dados **podem** ser exfiltrados usando inferência baseada em booleanos  
- [ ] Sim — a travessia total da árvore XML e a extração de dados **são possíveis**  

---