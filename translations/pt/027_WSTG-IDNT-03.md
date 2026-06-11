## WSTG-IDNT-03 — Teste do Processo de Provisionamento de Contas

O processo de provisionamento de contas rege como novas identidades são criadas, verificadas e como lhes são atribuídos privilégios dentro de um ambiente de aplicação. Atacantes visam este fluxo de trabalho para realizar registros em massa automatizados para fins de spam ou Denial-of-Service (DoS), contornar etapas de verificação de identidade para criar contas fraudulentas ou manipular parâmetros para conceder a si mesmos permissões elevadas durante a fase de cadastro. Vulnerabilidades manifestam-se frequentemente em formulários de registro por autoatendimento, sistemas apenas para convidados ou consoles de provisionamento administrativo onde falhas de lógica de negócio permitem a criação não autorizada de contas ou a Escalada de Privilégios (Privilege Escalation). Do ponto de vista de um atacante, comprometer o fluxo de provisionamento fornece uma base legítima que pode ser usada como ponto de partida para explorações mais profundas, exfiltração de dados ou ataques de engenharia social.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Crítica se um atacante puder provisionar contas administrativas ou contornar restrições globais de registro.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### O auto-registro é estritamente controlado ou limitado a usuários autorizados?
- [ ] Sim — o registro está **desabilitado** ou requer aprovação administrativa manual  
- [ ] Sim — o registro está aberto, mas restrito a domínios de e-mail **autorizados** ou tokens de convite  
- [ ] Não — o registro está **aberto** a qualquer usuário sem restrições de domínio ou token  

### É necessário um mecanismo robusto de verificação de identidade (ex: e-mail/SMS) antes da ativação da conta?
- [ ] Sim — a verificação é **obrigatória** e **não pode** ser contornada  
- [ ] Sim — a verificação é **obrigatória**, mas **pode** ser contornada via manipulação de parâmetros ou tokens previsíveis  
- [ ] Não — as contas ficam **ativas imediatamente** após o envio, sem qualquer verificação  

### O usuário pode manipular parâmetros de função (role) ou permissão durante a requisição de registro?
- [ ] Não — as funções são **atribuídas no lado do servidor** e não existem parâmetros no lado do cliente  
- [ ] Sim — existem parâmetros de função, mas a manipulação **não é possível** devido à validação no lado do servidor  
- [ ] Sim — parâmetros de função/permissão (ex: `role=admin`, `is_staff=true`) **podem** ser modificados para obter acesso elevado *(Crítico)*  

### Estão implementados controles de Rate Limiting ou anti-automação para evitar a criação de contas em massa?
- [ ] Sim — Rate Limiting e CAPTCHAs estão **ativos** e são eficazes  
- [ ] Sim — existem controles, mas o **bypass é possível** via spoofing de cabeçalho ou serviços de resolução de CAPTCHA  
- [ ] Não — nenhum Rate Limiting ou controle anti-automação é **aplicado**  

### O processo de provisionamento vaza informações sobre contas existentes (User Enumeration)?
- [ ] Não — as mensagens de resposta são **idênticas** para usuários existentes e não existentes  
- [ ] Sim — respostas diferentes ou variações de tempo **permitem** a enumeração de usuários existentes  

---