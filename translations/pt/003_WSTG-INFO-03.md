## WSTG-INFO-03 — Revisão de Meta-arquivos do Servidor Web para Vazamento de Informações

Meta-arquivos de servidor web, como `robots.txt`, `sitemap.xml` e `security.txt`, destinam-se a orientar crawlers e fornecer metadados administrativos, mas frequentemente revelam inadvertidamente estruturas de diretórios sensíveis ou endpoints ocultos. Atacantes analisam esses arquivos para enumerar a superfície de ataque da aplicação, identificando diretivas "Disallow" que frequentemente apontam para painéis administrativos não vinculados, diretórios de backup ou ambientes de desenvolvimento. Além das instruções para mecanismos de busca, arquivos no diretório `.well-known/` ou arquivos como `humans.txt` podem expor stacks de tecnologia, informações de desenvolvedores ou contatos de segurança organizacional. A revisão sistemática desses arquivos permite que um testador obtenha insights estruturais e técnicos sem realizar uma descoberta agressiva por Brute Force.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Baixa / Informativa* |

> *A gravidade torna-se Média se os meta-arquivos revelarem interfaces administrativas não autenticadas ou backups de configuração sensíveis.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### O arquivo `robots.txt` existe e expõe diretórios sensíveis?
- [ ] Não — o `robots.txt` **não** existe  
- [ ] Sim — o `robots.txt` existe, mas contém apenas caminhos públicos padrão  
- [ ] Sim — o `robots.txt` revela caminhos de diretórios sensíveis ou interfaces administrativas  
- [ ] Sim — o `robots.txt` revela credenciais ou versões específicas de software em comentários  

### Um arquivo `sitemap.xml` está presente e revela uma estrutura oculta da aplicação?
- [ ] Não — o `sitemap.xml` **não** existe  
- [ ] Sim — o `sitemap.xml` está presente, mas lista apenas URLs voltadas para o público  
- [ ] Sim — o `sitemap.xml` lista endpoints não vinculados ou recursos apenas internos que **podem** ser acessados  

### Os meta-arquivos de segurança e organizacionais estão expondo detalhes técnicos?
- [ ] Não — nenhum arquivo de metadados encontrado na raiz ou em `.well-known/`  
- [ ] Sim — `security.txt` ou `humans.txt` estão presentes, mas contêm informações padrão  
- [ ] Sim — os meta-arquivos expõem hostnames internos, identidades de desenvolvedores ou detalhes da stack de tecnologia que **podem** auxiliar em engenharia social ou ataques posteriores  

### Existem meta-arquivos legados ou específicos de framework presentes?
- [ ] Não — nenhum arquivo específico de framework (ex: `info.php`, `composer.json`, `package.json`) encontrado  
- [ ] Sim — arquivos como `README.md`, `CHANGELOG.txt` ou `.DS_Store` estão presentes, mas sanitizados  
- [ ] Sim — arquivos específicos de framework ou versão estão presentes e **permitem** um fingerprinting preciso da tecnologia  

---