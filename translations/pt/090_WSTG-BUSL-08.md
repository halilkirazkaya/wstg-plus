## WSTG-BUSL-08 — Teste de Upload de Tipos de Arquivos Inesperados

O upload de tipos de arquivos inesperados permite que atacantes ignorem restrições de lógica de negócios ao enviar arquivos que a aplicação não pretende processar, potencialmente levando a Remote Code Execution, Cross-Site Scripting (XSS) ou Denial of Service. Os atacantes testam essas vulnerabilidades modificando extensões de arquivos, MIME types e magic bytes para enganar a lógica de validação do lado do servidor (server-side) e fazê-la aceitar conteúdo malicioso. Isso ocorre tipicamente em uploads de fotos de perfil, sistemas de gerenciamento de documentos ou anexos de tickets de suporte onde existe validação insuficiente no back-end. A exploração bem-sucedida pode comprometer o servidor hospedeiro se o arquivo enviado for executado ou levar a ataques do lado do cliente (client-side) se o arquivo for servido a outros usuários com um Content-Type incorreto.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Ferramentas:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### A aplicação restringe o upload de arquivos a um conjunto específico de extensões?
- [ ] Sim — uma whitelist estrita de extensões é aplicada e o bypass **não é possível**  
- [ ] Sim — uma whitelist de extensões está presente, mas o bypass **é possível** via extensões duplas ou null bytes  
- [ ] Não — qualquer extensão de arquivo **pode** ser carregada  

### O conteúdo do arquivo é validado além da extensão do arquivo?
- [ ] Sim — a lógica server-side verifica Magic Bytes e realiza inspeção profunda de conteúdo  
- [ ] Sim — a lógica server-side verifica o MIME type apenas através do cabeçalho `Content-Type`  
- [ ] Não — nenhuma validação de conteúdo **é aplicada** após a verificação da extensão  

### Um atacante pode executar código enviando um web shell ou script?
- [ ] Não — os arquivos enviados são armazenados em um diretório não executável ou em um bucket de armazenamento (S3/Azure Blob)  
- [ ] Sim — os arquivos são armazenados em um diretório acessível pela web, mas a execução **está desativada** via configuração do servidor  
- [ ] Sim — scripts maliciosos enviados **podem** ser executados diretamente através de um caminho de URL previsível *(Crítica)*  

### Ataques client-side como XSS são possíveis através de tipos de arquivos enviados?
- [ ] Não — os arquivos são servidos com `Content-Disposition: attachment` e `X-Content-Type-Options: nosniff`  
- [ ] Sim — arquivos SVG ou HTML **podem** ser enviados e renderizados no navegador, levando a XSS  
- [ ] Sim — metadados de imagem (EXIF) **são processados** e refletidos na UI sem sanitização  

---