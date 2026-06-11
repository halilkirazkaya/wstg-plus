## WSTG-SESS-08 — Session Puzzling

O Session Puzzling, também conhecido como Session Variable Overloading (Sobrecarga de Variável de Sessão), ocorre quando uma aplicação utiliza a mesma variável de sessão para múltiplos propósitos em diferentes módulos ou falha ao limpar corretamente os estados da sessão durante transições de fluxo de trabalho. Atacantes exploram esse comportamento populando variáveis de sessão em um contexto — como uma visualização de perfil não autenticada ou um registro de múltiplas etapas — e então navegando para uma área protegida onde a aplicação confia incorretamente nessas variáveis pré-existentes. Isso pode levar a impactos críticos, incluindo bypass de autenticação, escalada de privilégios (Privilege Escalation) ou acesso não autorizado a dados sensíveis, enganando a aplicação para que ela acredite que uma sessão está em um estado de privilégio mais elevado do que realmente está. A vulnerabilidade é mais prevalente em aplicações complexas e com estado (stateful), onde a lógica de gerenciamento de sessão é aplicada de forma inconsistente em diferentes componentes funcionais.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### A aplicação utiliza os mesmos nomes de variáveis de sessão para propósitos diferentes em módulos distintos?
- [ ] Não — os nomes das variáveis são únicos ou os escopos são isolados  
- [ ] Sim — existem nomes de variáveis compartilhados, mas o estado é limpo entre os módulos  
- [ ] Sim — existem nomes de variáveis compartilhados e o estado **não** é limpo  

### Ações não autenticadas podem influenciar variáveis de sessão usadas em contextos autenticados?
- [ ] Não — as variáveis de sessão são inicializadas apenas **após** a autenticação bem-sucedida  
- [ ] Sim — variáveis de sessão podem ser definidas antes do login, mas **não** afetam o estado pós-login  
- [ ] Sim — variáveis de sessão definidas durante a pré-autenticação **são** confiadas no pós-autenticação *(Crítico)*  

### A aplicação reinicializa o identificador de sessão e suas variáveis associadas após uma mudança no nível de privilégio?
- [ ] Sim — a sessão é totalmente resetada e um novo ID **é emitido**  
- [ ] Parcial — um novo ID é emitido, mas algumas variáveis de sessão **persistem**  
- [ ] Não — o ID de sessão e todas as variáveis **permanecem** inalterados  

### Privilégios administrativos podem ser obtidos manipulando variáveis de sessão através de uma conta sem privilégios?
- [ ] Não — as variáveis de privilégio **não podem** ser modificadas pelos usuários  
- [ ] Sim — as variáveis podem ser modificadas, mas a aplicação as **valida** contra o banco de dados no backend  
- [ ] Sim — as variáveis podem ser modificadas e a aplicação **confia** no estado da sessão implicitamente *(Crítico)*  

### O estado da sessão é limpo imediatamente após a conclusão de um fluxo de trabalho específico (ex: redefinição de senha, checkout)?
- [ ] Sim — as variáveis de sessão para o fluxo de trabalho são destruídas após a conclusão  
- [ ] Não — as variáveis do fluxo de trabalho **persistem** e podem ser reutilizadas em outras áreas da aplicação  

---