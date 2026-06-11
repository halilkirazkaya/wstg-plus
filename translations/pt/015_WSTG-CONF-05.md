## WSTG-CONF-05 — Enumerar Interfaces de Administração de Infraestrutura e Aplicação

As interfaces administrativas representam o plano de controle de gerenciamento de uma aplicação e de sua infraestrutura subjacente, frequentemente concedendo acesso privilegiado a configurações do sistema, dados de usuários e operações server-side. Atacantes priorizam a enumeração dessas interfaces por meio de Brute Force de diretórios, análise de `robots.txt` ou observação de padrões de URL previsíveis para encontrar pontos de entrada que possam carecer de autenticação robusta ou depender de credenciais padrão. A descoberta de um painel de administração exposto aumenta significativamente o risco de comprometimento total do sistema, modificação não autorizada de dados ou interrupção do serviço. Essas interfaces são frequentemente encontradas em portas não padrão ou subdomínios, tornando-as alvos principais para scanners automatizados e reconhecimento manual.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a interface permitir alterações de configuração em todo o sistema, gerenciamento de usuários ou exfiltração de dados sem autenticação de múltiplos fatores (MFA).

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### As interfaces administrativas são descobertas via Fuzzing de diretórios ou reconhecimento?
- [ ] Não — nenhuma interface administrativa está exposta ou é detectável  
- [ ] Sim — as interfaces são detectáveis, mas o acesso **é restrito** a faixas de IP internos  
- [ ] Sim — as interfaces **são** detectáveis e acessíveis publicamente  

### A autenticação é obrigatória nos portais administrativos descobertos?
- [ ] Sim — autenticação de múltiplos fatores (MFA) **é obrigatória**  
- [ ] Sim — autenticação de fator único **é obrigatória**  
- [ ] Não — nenhuma autenticação é necessária para acessar a interface *(Crítico)*  

### Os caminhos administrativos estão ocultos ou são não padrão para evitar a descoberta?
- [ ] Sim — os caminhos utilizam nomenclatura não padrão, aleatória ou não previsível  
- [ ] Não — os caminhos utilizam convenções de nomenclatura comuns (ex: `/admin`, `/manager`, `/console`) e **podem** ser facilmente adivinhados  

### A interface fornece acesso a funcionalidades sensíveis do sistema ou da aplicação?
- [ ] Não — a interface é um dashboard de status apenas de leitura ("read-only") com impacto mínimo  
- [ ] Sim — a interface **pode** modificar a configuração da aplicação ou dados de usuários  
- [ ] Sim — a interface **pode** executar comandos em nível de sistema ou realizar gerenciamento de banco de dados  

---