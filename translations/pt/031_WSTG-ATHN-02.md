## WSTG-ATHN-02 — Teste de Credenciais Padrão (Default Credentials)

O teste de credenciais padrão envolve a identificação de interfaces administrativas, serviços ou componentes de aplicação que ainda utilizam nomes de usuário e senhas configurados de fábrica ou fornecidos pelo fornecedor. Esta vulnerabilidade é um alvo de alta prioridade para atacantes porque frequentemente fornece um caminho imediato para o comprometimento total do sistema, acesso administrativo ou execução remota de código com esforço mínimo. Ela ocorre tipicamente em softwares comerciais (off-the-shelf), plataformas CMS, ferramentas de gerenciamento de banco de dados e hardware integrado, como dispositivos IoT ou dispositivos de rede. A exploração é geralmente realizada cruzando as versões de software identificadas com bancos de dados públicos de senhas padrão ou utilizando ferramentas automatizadas de Credential Stuffing durante as fases iniciais de reconhecimento e exploração.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Crítica / Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Ferramentas:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### As interfaces administrativas ou de gerenciamento estão expostas à internet ou a segmentos de rede não confiáveis?
- [ ] Não — nenhuma interface de gerenciamento está acessível  
- [ ] Sim — as interfaces estão presentes, mas restritas por controles de nível de rede (ex: VPN, IP Allowlist)  
- [ ] Sim — as interfaces **estão** publicamente acessíveis e exigem autenticação  

### As credenciais padrão fornecidas pelo fornecedor foram alteradas para todos os componentes identificados?
- [ ] Sim — todas as credenciais padrão **foram alteradas** em todos os componentes *(Mais seguro)*  
- [ ] Sim — as credenciais **foram alteradas** para a aplicação principal, mas componentes secundários (ex: CMS, DB) permanecem com o padrão  
- [ ] Não — as credenciais padrão **estão ativas** em uma ou mais interfaces identificáveis *(Crítico)*  

### A aplicação força a alteração da senha no primeiro login para contas novas ou administrativas?
- [ ] Sim — a alteração de senha **é obrigatória** antes que qualquer ação administrativa possa ser tomada  
- [ ] Não — a aplicação **permite** o uso de credenciais padrão indefinidamente  

### Mecanismos de bloqueio (lockout) ou controles de Rate Limiting estão ativos para prevenir testes automatizados de credenciais padrão?
- [ ] Sim — Rate Limiting agressivo ou bloqueio de conta **é aplicado**  
- [ ] Sim — os controles estão presentes, mas o bypass **é possível** via manipulação de cabeçalho ou rotação de IP  
- [ ] Não — nenhum mecanismo de Rate Limiting ou bloqueio **está habilitado**  

### Plugins de terceiros, middleware ou ferramentas administrativas (ex: phpMyAdmin, Tomcat Manager) utilizam credenciais únicas?
- [ ] Não — componentes de terceiros não existem no ambiente  
- [ ] Sim — todos os componentes de terceiros utilizam credenciais únicas e não padronizadas  
- [ ] Não — pelo menos um componente de terceiros **está acessível** via credenciais padrão conhecidas  

---