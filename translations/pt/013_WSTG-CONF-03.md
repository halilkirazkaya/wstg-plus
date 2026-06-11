## WSTG-CONF-03 — Teste de Manipulação de Extensões de Arquivo para Informações Sensíveis

O teste de manipulação de extensões de arquivo envolve identificar se o servidor web ou o servidor de aplicação revela informações sensíveis ao fornecer arquivos que deveriam ser restritos ou executados. Atacantes frequentemente procuram por arquivos de backup, arquivos de configuração e fragmentos de código-fonte (ex: `.bak`, `.old`, `.env`, `.inc`) que podem ter sido deixados na raiz web (web root) e fornecidos como texto simples devido à falta de mapeamentos de manipuladores (handlers) específicos. Esta vulnerabilidade é relevante porque pode levar à exposição de credenciais de banco de dados, chaves de API e lógica de negócio interna, facilitando a exploração mais profunda da infraestrutura. Essas exposições ocorrem tipicamente em diretórios públicos, pastas de upload ou via configurações incorretas de MIME-type no servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Médio / Alto* |

> *A gravidade torna-se Alta se arquivos de configuração contendo credenciais ou o código-fonte completo estiverem acessíveis.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Extensões de arquivos de backup ou temporários sensíveis (ex: `.bak`, `.old`, `.swp`, `~`) estão acessíveis?
- [ ] Não — o servidor retorna 403 Forbidden ou 404 Not Found para extensões de backup comuns  
- [ ] Sim — o servidor permite o download de arquivos de backup, mas eles **não** contêm dados sensíveis  
- [ ] Sim — o servidor permite o download de arquivos de backup contendo dados sensíveis ou código-fonte *(Alta)*  

### O servidor expõe arquivos de configuração de ambiente ou do sistema (ex: `.env`, `.config`, `.yml`, `.ini`)?
- [ ] Não — arquivos de configuração restritos **não** podem ser acessados e retornam 403/404  
- [ ] Sim — arquivos de configuração sensíveis **estão** acessíveis e divulgam segredos internos ou credenciais  

### O código-fonte pode ser recuperado anexando extensões alternativas (ex: `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] Não — o código da aplicação **não** é renderizado como texto simples, independentemente da manipulação da extensão  
- [ ] Sim — o código-fonte é revelado porque o servidor **não** possui um manipulador (handler) para a extensão anexada  

### Diretórios administrativos ou de metadados (ex: `.git/`, `.svn/`, `.DS_Store`) estão bloqueados para acesso público?
- [ ] Não — esses diretórios/arquivos **não** existem na raiz web  
- [ ] Sim — o acesso **não é possível** devido a regras de reescrita no lado do servidor ou permissões  
- [ ] Sim — diretórios de metadados **estão** acessíveis e permitem a reconstrução total do código-fonte  

### Como o servidor trata requisições de arquivos com múltiplas extensões (ex: `file.php.jpg`)?
- [ ] Não — o servidor prioriza corretamente a segurança da extensão interna ou bloqueia a requisição  
- [ ] Sim — o servidor processa o arquivo como a primeira extensão, mas o bypass **não é possível** para execução  
- [ ] Sim — o servidor ignora a extensão final, permitindo que a execução de código ou a divulgação da fonte **seja possível**  

---