## WSTG-IDNT-05 — Teste para Política de Nome de Usuário Fraca ou Não Aplicada

O teste de políticas de nomes de usuário fracas ou não aplicadas foca nas regras que regem a criação de identificadores de conta e sua suscetibilidade a enumeração ou spoofing. Políticas fracas frequentemente permitem sequências previsíveis, palavras comuns de dicionário ou identificadores que espelham IDs de funcionários internos, o que reduz significativamente o espaço de busca para ataques de Brute Force e Credential Stuffing. Do ponto de vista de um atacante, identificar esses padrões é o primeiro passo para a descoberta de contas em larga escala, muitas vezes alcançada analisando mensagens de erro de registro, o comportamento de redefinição de senha ou metadados em perfis públicos. Garantir uma política robusta impede que atacantes mapeiem sistematicamente a base de usuários e facilita a defesa contra phishing direcionado e tentativas automatizadas de Authentication Bypass.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Os nomes de usuário são baseados em padrões altamente previsíveis ou sequenciais?
- [ ] Não — os nomes de usuário são aleatórios, definidos pelo usuário ou strings de alta entropia  
- [ ] Sim — os nomes de usuário seguem um formato previsível (ex: `user1001`, `user1002`), mas a autenticação é robusta  
- [ ] Sim — os nomes de usuário são **estritamente sequenciais** ou seguem um formato corporativo conhecido, facilitando a enumeração  

### A aplicação impõe requisitos de comprimento mínimo e complexidade para nomes de usuário?
- [ ] Sim — políticas rígidas de comprimento e conjunto de caracteres **são aplicadas** durante o registro  
- [ ] Sim — existem políticas, mas permitem nomes de usuário extremamente curtos (ex: 1-2 caracteres) ou excessivamente simples  
- [ ] Não — **nenhuma política** é aplicada em relação ao comprimento do nome de usuário ou tipos de caracteres  

### Nomes de usuário válidos podem ser enumerados por meio de respostas da aplicação?
- [ ] Não — a aplicação retorna mensagens de erro genéricas e exibe um timing consistente para todas as tentativas  
- [ ] Sim — a aplicação retorna mensagens de erro distintas (ex: "Nome de usuário já em uso"), mas o Rate Limiting **é aplicado**  
- [ ] Sim — a aplicação **vaza** nomes de usuário válidos através de erros de registro, redefinições de senha ou diferenças de timing  

### A política impede o registro de nomes de usuário administrativos comuns ou reservados?
- [ ] Sim — a aplicação bloqueia nomes comuns (ex: `admin`, `root`, `support`) e termos reservados do sistema  
- [ ] Não — os usuários **podem** registrar contas com nomes sensíveis ou administrativos  

### A política de nome de usuário é consistente em todos os pontos de entrada (API, Mobile, Web)?
- [ ] Sim — as políticas **são aplicadas** de forma consistente em todas as interfaces e versões  
- [ ] Não — endpoints legados ou versões específicas de API **não impõem** as mesmas restrições de nome de usuário  

---