## WSTG-CONF-09 — Teste de Permissões de Arquivo

O teste de permissões de arquivo envolve a auditoria dos níveis de controle de acesso atribuídos a arquivos e diretórios no servidor web para garantir que recursos sensíveis não estejam excessivamente expostos a usuários ou processos não autorizados. Permissões configuradas incorretamente podem permitir que atacantes leiam arquivos de configuração sensíveis contendo credenciais de banco de dados, visualizem código-fonte proprietário ou modifiquem scripts do lado do servidor para obter execução remota de código. Esta vulnerabilidade ocorre tipicamente durante a fase de implantação (deployment), quando sinalizações de "leitura global" (world-readable) ou "escrita global" (world-writable) são deixadas em diretórios críticos da aplicação, como caminhos de configuração, pastas de log ou repositórios de upload. Do ponto de vista de um atacante, a descoberta de um arquivo incorretamente protegido frequentemente serve como o catalisador primário para o escalonamento de privilégios ou o comprometimento total do sistema.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se arquivos de configuração sensíveis (ex: `.env`, `web.config`) estiverem legíveis ou se o acesso de escrita na raiz da web (web root) for possível.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Ferramentas:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Os arquivos de configuração sensíveis da aplicação estão protegidos contra acesso não autorizado?
- [ ] Sim — arquivos de configuração (ex: `.env`, `config.php`) **não são acessíveis** via requisições web  
- [ ] Sim — os arquivos estão presentes, mas o acesso é **restrito** apenas a usuários locais autorizados  
- [ ] Não — arquivos de configuração sensíveis **estão acessíveis** e expõem credenciais ou segredos  

### Um atacante pode modificar arquivos ou diretórios dentro da raiz da web (web root)?
- [ ] Não — todos os diretórios da raiz da web são de **apenas leitura** para o usuário do servidor web  
- [ ] Sim — existem diretórios com permissão de escrita, mas a execução de scripts está **desativada**  
- [ ] Sim — existem diretórios com permissão de escrita global (**world-writable**) que **permitem** o upload e a execução de scripts *(Crítico)*  

### Metadados de controle de versão ou arquivos de backup estão expostos devido a permissões frouxas?
- [ ] Não — arquivos `.git`, `.svn` e arquivos de backup (ex: `.bak`, `~`) **não estão presentes** ou estão protegidos  
- [ ] Sim — diretórios de metadados existem, mas a listagem de diretórios está **desativada**  
- [ ] Sim — metadados sensíveis ou backups estão **totalmente acessíveis**, permitindo a recuperação do código-fonte  

### O usuário do servidor web opera com o princípio do menor privilégio?
- [ ] Sim — o processo do servidor web é executado como um usuário **dedicado de baixo privilégio**  
- [ ] Não — o processo do servidor web é executado com **privilégios desnecessários** (ex: `root` ou `Administrator`)  

### Os arquivos de log estão protegidos contra leitura ou modificação não autorizada?
- [ ] Sim — os arquivos de log são **restritos** a usuários administrativos  
- [ ] Sim — os arquivos de log são legíveis, mas **não** contêm tokens de sessão ou PII (Informações de Identificação Pessoal)  
- [ ] Não — os arquivos de log são **publicamente legíveis** e expõem dados sensíveis de transações ou informações de usuários  

---