## WSTG-ATHN-09 — Teste de Funcionalidades Fracas de Alteração ou Redefinição de Senha

Mecanismos de alteração e redefinição de senha são componentes críticos do gerenciamento de autenticação que, se implementados incorretamente, permitem que atacantes assumam o controle de contas de usuários. Essas vulnerabilidades geralmente envolvem tokens de redefinição previsíveis, falta de bloqueios de conta durante tentativas de Brute Force, entrega insegura de tokens ou a capacidade de alterar senhas sem verificar a senha atual. Atacantes exploram essas falhas interceptando ou adivinhando links de recuperação, realizando Host Header Injection para redirecionar e-mails de redefinição ou utilizando fixação de sessão para manter o acesso após uma alteração de senha. Garantir que esses processos exijam uma verificação forte e utilizem aleatoriedade criptográfica é essencial para manter a integridade da conta e evitar o acesso não autorizado.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### A senha atual é obrigatória para definir uma nova senha durante uma alteração padrão?
- [ ] Sim — a senha atual **é obrigatória** e verificada  
- [ ] Sim — a senha atual **é obrigatória**, mas pode ser burlada via Parameter Pollution ou manipulação de parâmetros  
- [ ] Não — a senha atual **não é** solicitada, permitindo Account Takeover via Session Hijacking *(Alta)*  

### Os tokens de redefinição de senha são criptograficamente seguros e imprevisíveis?
- [ ] Sim — os tokens possuem alta entropia, são aleatórios e **não são previsíveis**  
- [ ] Sim — os tokens são gerados, mas apresentam baixa entropia ou padrões previsíveis (ex: baseados em timestamp)  
- [ ] Não — os tokens são **previsíveis** ou baseados em dados estáticos do usuário, como e-mail/ID codificado em Base64 *(Crítica)*  

### O link de redefinição de senha pode ser redirecionado via Host Header Injection?
- [ ] Não — a aplicação ignora ou valida o cabeçalho `Host` para a geração de links  
- [ ] Sim — a aplicação utiliza o cabeçalho `Host` ou `X-Forwarded-Host` para construir links, mas a validação **está presente**  
- [ ] Sim — os links **podem** ser redirecionados para um domínio controlado pelo atacante via manipulação de cabeçalho *(Alta)*  

### O token de redefinição possui um ciclo de vida limitado e invalidação imediata?
- [ ] Sim — os tokens expiram rapidamente e são invalidados **imediatamente** após o uso ou alteração de senha  
- [ ] Sim — os tokens expiram após um longo período (ex: 24+ horas) ou permitem **múltiplos usos**  
- [ ] Não — os tokens **nunca expiram** ou permanecem válidos após a senha ter sido alterada com sucesso  

### Existem Rate Limiting ou bloqueios nos endpoints de redefinição e alteração de senha?
- [ ] Sim — Rate Limiting rigoroso e CAPTCHAs **são aplicados** para evitar enumeração e Brute Force  
- [ ] Sim — existem controles limitados, mas o bypass **é possível** via rotação de IP ou Header Spoofing  
- [ ] Não — nenhum Rate Limiting **é aplicado**, permitindo a enumeração de contas em massa ou Brute Force de tokens  

---