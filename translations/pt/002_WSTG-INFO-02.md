## WSTG-INFO-02 — Fingerprint Web Server

O fingerprinting de servidor web é o processo de identificar o tipo específico de software, a versão e o sistema operacional subjacente de um servidor web alvo. Os atacantes realizam este reconhecimento para identificar vulnerabilidades conhecidas, como CVEs não corrigidos ou fraquezas de configuração, que são específicos para certas versões de Apache, Nginx, IIS ou outras tecnologias de servidor. Isso é normalmente alcançado através da análise de cabeçalhos de resposta HTTP (ex: `Server`, `X-Powered-By`), do exame de comportamentos únicos de protocolos ou da observação de informações vazadas por meio de arquivos e páginas de erro padrão. Um fingerprinting bem-sucedido permite que um atacante adapte sua estratégia de Exploit, passando de um escaneamento genérico para ataques de alta probabilidade contra falhas arquiteturais conhecidas.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Informativo / Baixa* |

> *A severidade torna-se Alta se o fingerprinting revelar uma versão de servidor obsoleta ou em fim de vida (EOL) com vulnerabilidades críticas conhecidas.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Ferramentas:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### As strings de identificação estão presentes nos cabeçalhos de resposta HTTP?
- [ ] Não — os cabeçalhos `Server` e `X-Powered-By` **não estão presentes** ou contêm valores genéricos  
- [ ] Sim — os cabeçalhos revelam o software do servidor, mas **não** os números de versão específicos  
- [ ] Sim — os cabeçalhos revelam o software do servidor específico e os números de versão **exatos**  

### As páginas de erro padrão ou respostas geradas pelo sistema vazam detalhes do servidor?
- [ ] Não — páginas de erro personalizadas são usadas e **não** divulgam informações do servidor  
- [ ] Sim — as páginas de erro padrão vazam o nome e/ou a versão do software do servidor  
- [ ] Sim — as páginas de erro vazam detalhes do servidor, versão e o **sistema operacional subjacente**  

### A versão do servidor é exposta através de arquivos padrão ou estruturas de diretórios?
- [ ] Não — arquivos de instalação padrão, manuais e scripts de teste foram **removidos**  
- [ ] Sim — arquivos padrão (ex: `info.php`, `manual/`, `test.html`) estão **presentes** e divulgam dados da versão  

### O tipo de servidor pode ser inferido com precisão através da análise de comportamento único?
- [ ] Não — as respostas do servidor são normalizadas e o fingerprinting baseado em comportamento **não é possível**  
- [ ] Sim — comportamentos únicos (ex: ordenação de cabeçalhos, códigos de erro específicos ou peculiaridades da pilha TCP/IP) **permitem** a identificação do servidor  

### A versão do servidor identificada é atualmente suportada e está corrigida (patched)?
- [ ] Sim — a versão identificada é atual e **suportada** pelo fornecedor  
- [ ] Não — a versão identificada está **desatualizada** ou em **fim de vida (EOL)** com vulnerabilidades conhecidas  

---