## WSTG-ATHN-11 — Testando Autenticação Multifator (MFA)

O teste de Autenticação Multifator (MFA) avalia a robustez da camada secundária de segurança projetada para impedir o acesso não autorizado, mesmo quando as credenciais primárias estão comprometidas. Atacantes frequentemente tentam burlar o MFA identificando endpoints onde ele é aplicado de forma inconsistente, como versões legadas de API, backends móveis ou fluxos de redefinição de senha. A exploração geralmente envolve a manipulação de respostas do servidor, brute-forcing de códigos de curta duração (OTP) ou a exploração de race conditions e falhas de gerenciamento de sessão que permitem que um usuário transite para um estado autenticado sem fornecer o segundo fator.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### O MFA é aplicado de forma consistente em todos os portais de autenticação?
- [ ] Sim — O MFA é obrigatório para todas as tentativas de login via web, mobile e baseadas em API  
- [ ] Sim — mas o MFA **não é aplicado** em endpoints específicos (ex: `/api/v1/login` legado ou portais específicos para mobile)  
- [ ] Não — O MFA **não está implementado** na aplicação *(Crítico)*  

### A etapa de verificação de MFA pode ser burlada via navegação direta por URL ou manipulação de resposta?
- [ ] Não — a navegação direta ou a adulteração de parâmetros (parameter tampering) **não podem** burlar o desafio  
- [ ] Sim — navegar diretamente para URLs de dashboards internos **é possível** sem concluir o desafio de MFA  
- [ ] Sim — manipular a resposta do servidor (ex: alterar um `401 Unauthorized` para `200 OK`) **é possível** para obter acesso  

### O processo de verificação de código MFA está protegido contra ataques de brute-force?
- [ ] Sim — rate limiting rigoroso ou bloqueio de conta **é aplicado** após múltiplas tentativas falhas de OTP  
- [ ] Sim — o rate limiting existe, mas **é possível** burlá-lo via rotação de IP ou manipulação de headers  
- [ ] Não — nenhum rate limiting é aplicado, e o brute-forcing automatizado de códigos **é possível**  

### A aplicação mantém um estado de sessão seguro durante a transição de MFA?
- [ ] Sim — tokens de sessão de alto privilégio são emitidos **apenas após** a conclusão bem-sucedida do MFA  
- [ ] Não — um cookie de sessão totalmente funcional é emitido **antes** da conclusão do MFA, permitindo acesso a alguns recursos  
- [ ] Não — a aplicação utiliza um identificador estático ou uma transição de sessão previsível que **pode** ser sequestrada (hijacked)  

### Fatores alternativos de MFA (SMS, E-mail, Códigos de Backup) são vulneráveis a exploração?
- [ ] Não — todos os fatores utilizam valores seguros, não previsíveis e com tempo limitado  
- [ ] Sim — os códigos de backup são previsíveis ou **podem** ser enumerados  
- [ ] Sim — a funcionalidade "Reenviar Código" pode ser abusada para realizar SMS/Email flooding ou revelar detalhes parciais de contato  

---