## WSTG-ATHZ-01 — Testando Directory Traversal e File Include

Vulnerabilidades de Directory Traversal e inclusão de arquivos (File Include) surgem quando uma aplicação utiliza entrada controlável pelo usuário para construir caminhos para arquivos ou diretórios sem validação ou sanitização suficiente. Atacantes exploram essas falhas injetando sequências como `../` para navegar fora do diretório pretendido, potencialmente acessando arquivos sensíveis do sistema, dados de configuração ou código-fonte da aplicação. Em casos mais graves envolvendo Local File Inclusion (LFI) ou Remote File Inclusion (RFI), um atacante pode obter execução remota de código (Remote Code Execution) incluindo scripts maliciosos ou aproveitando log poisoning e wrappers de PHP. Essas vulnerabilidades normalmente residem em parâmetros usados para carregamento dinâmico de conteúdo, mecanismos de template (template engines) ou endpoints de recuperação de imagens onde a lógica do lado do servidor (server-side) manipula caminhos do sistema de arquivos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Ferramentas:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Os parâmetros aceitam nomes de arquivos ou caminhos para processamento no servidor?
- [ ] Não — nenhum parâmetro parece interagir com o sistema de arquivos  
- [ ] Sim — existem parâmetros, mas utilizam uma allowlist rigorosa de identificadores de arquivos  
- [ ] Sim — os parâmetros aceitam nomes de arquivos ou caminhos diretos  

### A entrada é sanitizada para evitar sequências de directory traversal?
- [ ] Sim — a entrada é validada em relação a uma allowlist rigorosa e as sequências de traversal **não são possíveis**  
- [ ] Sim — a entrada é sanitizada removendo sequências `../`, mas um bypass recursivo **é possível**  
- [ ] Não — nenhuma sanitização ou validação **é aplicada** à entrada relacionada ao caminho  

### É possível acessar arquivos fora do diretório restrito através de sequências de traversal?
- [ ] Não — a aplicação ou o SO impede o acesso a arquivos fora do escopo definido  
- [ ] Sim — o acesso a arquivos dentro da raiz da web (web root) **é possível**  
- [ ] Sim — o acesso a arquivos sensíveis do sistema (ex: `/etc/passwd`, `C:\Windows\win.ini`) **é possível** *(Crítica)*  

### O servidor permite a inclusão de URLs remotas (RFI)?
- [ ] Não — a inclusão de arquivos remotos está **desativada** no nível do servidor/aplicação  
- [ ] Sim — arquivos remotos **podem** ser incluídos, mas a execução **não é possível**  
- [ ] Sim — arquivos remotos **podem** ser incluídos e executados no servidor *(Crítica)*  

### Os filtros podem ser burlados usando codificação ou caracteres especiais?
- [ ] Não — os filtros lidam de forma eficaz com várias codificações e null bytes  
- [ ] Sim — o bypass **é possível** usando URL encoding, double URL encoding ou Unicode de 16 bits  
- [ ] Sim — o bypass **é possível** usando injeção de null byte (`%00`) ou wrappers do sistema de arquivos (ex: `php://filter`)  

---