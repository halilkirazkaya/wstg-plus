## WSTG-CONF-04 — Revisão de Backups Antigos e Arquivos Não Referenciados em busca de Informações Sensíveis

A revisão de backups antigos e arquivos não referenciados envolve a identificação de arquivos esquecidos, temporários ou ocultos em um servidor web que não se destinam ao acesso público. Esses arquivos geralmente incluem backups de código-fonte (`.zip`, `.bak`), arquivos de swap de editores de texto (`.swp`, `~`) ou metadados de controle de versão (`.git`, `.svn`) que podem vazar informações sensíveis. Atacantes utilizam wordlists automatizadas e ferramentas de descoberta para realizar Brute Force em convenções de nomenclatura comuns, visando a exfiltração de credenciais de banco de dados, chaves de API hardcoded ou lógica que facilite explorações posteriores. Essa vulnerabilidade geralmente decorre de manutenção manual do servidor ou pipelines de CI/CD falhas que não realizam a sanitização da raiz web (web root) de produção.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se arquivos de configuração, arquivos de código-fonte ou credenciais forem descobertos.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### A listagem de diretórios (directory listing) está habilitada no servidor web?
- [ ] Não — a listagem de diretórios está **desabilitada** e retorna um erro 403 Forbidden ou um erro personalizado  
- [ ] Sim — a listagem de diretórios está **habilitada** em diretórios não sensíveis  
- [ ] Sim — a listagem de diretórios está **habilitada** em diretórios que contêm código-fonte ou arquivos sensíveis *(Crítico)*  

### Arquivos de backup com extensões comuns (ex: .bak, .old, .save) são detectáveis?
- [ ] Não — extensões de backup comuns **não foram encontradas** ou estão bloqueadas pela configuração do servidor  
- [ ] Sim — arquivos de backup existem, mas **não** contêm informações sensíveis  
- [ ] Sim — arquivos de backup de configuração ou código-fonte **estão** acessíveis *(Alta)*  

### Diretórios de controle de versão (ex: .git, .svn) ou metadados estão expostos?
- [ ] Não — metadados de controle de versão **não estão presentes** ou estão bloqueados corretamente  
- [ ] Sim — metadados existem, mas o repositório completo **não pode** ser reconstruído  
- [ ] Sim — todo o repositório de código-fonte **pode** ser exfiltrado através de metadados expostos  

### Arquivos compactados (ex: .zip, .tar.gz, .rar) da aplicação estão presentes na raiz web (web root)?
- [ ] Não — nenhum arquivo compactado sensível foi descoberto via Brute Force ou enumeração  
- [ ] Sim — arquivos compactados foram encontrados, mas estão protegidos por senha ou contêm ativos públicos  
- [ ] Sim — arquivos compactados não criptografados contendo o código ou dados da aplicação **estão** publicamente acessíveis  

### A aplicação vaza arquivos temporários criados por editores de texto ou IDEs?
- [ ] Não — arquivos temporários como `.swp`, `~` ou `.DS_Store` **não estão presentes**  
- [ ] Sim — arquivos temporários não sensíveis estão **presentes**  
- [ ] Sim — arquivos temporários revelam trechos de código-fonte ou caminhos internos  

---