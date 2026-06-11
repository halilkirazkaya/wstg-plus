## WSTG-INPV-18 — Testing for Server-Side Template Injection

O Server-Side Template Injection (SSTI) ocorre quando uma aplicação incorpora entradas controladas pelo usuário em um mecanismo de template no lado do servidor (server-side template engine) sem a devida sanitização ou escape. Isso permite que um atacante injete diretivas de template maliciosas, que são então executadas no servidor, levando potencialmente ao comprometimento total do sistema através de Remote Code Execution (RCE). O SSTI normalmente se manifesta em funcionalidades como geração dinâmica de e-mails, páginas de perfil e sistemas de notificação que utilizam mecanismos como Jinja2, Twig ou FreeMarker. Do ponto de vista de um atacante, essa vulnerabilidade é frequentemente explorada para vazar variáveis de ambiente sensíveis, ler arquivos internos ou executar comandos do sistema aproveitando-se de objetos e métodos nativos do mecanismo de template.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Ferramentas:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### A aplicação utiliza um mecanismo de template no lado do servidor para processar a entrada do usuário?
- [ ] Não — a aplicação **não** utiliza templates no lado do servidor para conteúdo dinâmico  
- [ ] Sim — os templates são utilizados, mas a entrada do usuário **não** é incorporada em diretivas de template  
- [ ] Sim — a entrada do usuário é incorporada em templates **sem** a sanitização adequada  

### Expressões aritméticas básicas são avaliadas quando injetadas em campos de entrada?
- [ ] Não — payloads como `{{7*7}}` ou `${7*7}` são refletidos como strings literais  
- [ ] Sim — as expressões são avaliadas (ex: retornando `49`), confirmando que o mecanismo de template **é vulnerável**  

### É possível acessar os objetos ou métodos integrados (built-in) do mecanismo de template?
- [ ] Não — o acesso a objetos, métodos ou configurações perigosas é **restrito** ou isolado (sandboxed)  
- [ ] Sim — objetos integrados (ex: `self`, `config`, `__class__`) **podem** ser acessados para vazar dados sensíveis  
- [ ] Sim — a execução remota de código (RCE) via chamadas de sistema ou bibliotecas do SO **é possível**  

### Existe um mecanismo de sandbox ou lista de permissões (allowlist) que impeça a execução de diretivas perigosas?
- [ ] Sim — uma allowlist rigorosa ou sandbox seguro está em vigor e o bypass **não é possível**  
- [ ] Sim — o sandbox está presente, mas o bypass **é possível** através de payloads específicos dependentes do mecanismo  
- [ ] Não — nenhuma sandbox ou allowlist **é aplicada** ao mecanismo de template  

---