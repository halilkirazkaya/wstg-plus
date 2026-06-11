## WSTG-BUSL-09 — Testar Upload de Arquivos Maliciosos

A funcionalidade de upload de arquivos permite que os usuários enviem dados para o servidor, fornecendo um vetor direto para a introdução de conteúdo malicioso no ambiente da aplicação. Atacantes exploram a lógica de validação fraca para realizar o upload de web shells, malware ou arquivos especialmente criados — como SVG ou HTML — para obter Remote Code Execution (RCE) ou Cross-Site Scripting (XSS). Esta vulnerabilidade é tipicamente encontrada em recursos como configurações de perfil, sistemas de gerenciamento de documentos e manipuladores de anexos, onde o servidor falha em verificar adequadamente os tipos de arquivos, conteúdos e permissões de execução. A exploração bem-sucedida geralmente leva ao comprometimento total do sistema, exfiltração de dados ou ao uso do servidor como um ponto de pivô para ataques na rede interna.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### A aplicação fornece funcionalidade para upload de arquivos?
- [ ] Não — não existe funcionalidade de upload de arquivos  
- [ ] Sim — a funcionalidade existe para usuários autenticados  
- [ ] Sim — a funcionalidade está disponível para usuários **não autenticados** *(Crítico)*  

### A validação da extensão do arquivo é aplicada no lado do servidor (server-side)?
- [ ] Sim — uma allowlist rigorosa de extensões é **aplicada** e o bypass **não é possível**  
- [ ] Sim — uma allowlist é usada, mas o bypass **é possível** via case sensitivity ou extensões duplas  
- [ ] Sim — uma blocklist é usada, a qual **pode** ser burlada com extensões alternativas (ex: `.phtml`, `.asa`)  
- [ ] Não — nenhuma validação de extensão é **aplicada** no lado do servidor  

### O conteúdo do arquivo é validado além da extensão ou do MIME type?
- [ ] Sim — a aplicação verifica magic bytes e realiza inspeção profunda de conteúdo  
- [ ] Sim — a aplicação apenas verifica o cabeçalho `Content-Type`, o qual **é** facilmente forjado (spoofed)  
- [ ] Não — a aplicação depende inteiramente da extensão do arquivo para validação  

### Os arquivos carregados são armazenados em um diretório com permissões de execução?
- [ ] Não — os arquivos são armazenados em um serviço de armazenamento dedicado (ex: S3) ou em um diretório não executável  
- [ ] Sim — os arquivos são armazenados no servidor web, mas a execução está **desativada** via configuração  
- [ ] Sim — os arquivos são armazenados em um diretório acessível via web e a execução **está ativada** *(Crítico)*  

### Os filtros de segurança podem ser burlados via manipulação do nome do arquivo?
- [ ] Não — os nomes dos arquivos são sanitizados ou regenerados aleatoriamente pelo servidor  
- [ ] Sim — os filtros **podem** ser burlados usando null bytes (ex: `shell.php%00.jpg`)  
- [ ] Sim — caracteres de path traversal (ex: `../../`) **podem** ser usados para fazer upload de arquivos para diretórios arbitrários  

---