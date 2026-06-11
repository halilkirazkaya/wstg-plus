## WSTG-INPV-19 — Teste para Server-Side Request Forgery (SSRF)

O Server-Side Request Forgery (SSRF) ocorre quando um atacante consegue influenciar uma aplicação web a realizar requisições não autorizadas a partir do servidor para recursos internos ou externos. Ao manipular parâmetros que esperam URLs, hostnames ou endereços IP, um atacante pode burlar controles de nível de rede, como firewalls e ACLs, para sondar segmentos de rede interna, acessar serviços de metadados de nuvem (IMDS) ou interagir com APIs e bancos de dados internos vulneráveis. Esta vulnerabilidade geralmente se manifesta em funcionalidades que envolvem a busca de imagens, geração de PDF, webhooks ou uploads de arquivos via URL. Do ponto de vista de um atacante, o SSRF é uma primitiva poderosa utilizada para realizar pivô em ambientes isolados, exfiltrar dados de configuração sensíveis ou, potencialmente, obter execução remota de código (Remote Code Execution - RCE) através da interação com serviços internos como Redis ou Memcached.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Ferramentas:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### A aplicação processa URLs ou hostnames fornecidos pelo usuário?
- [ ] Não — nenhuma funcionalidade aceita URLs ou hostnames externos  
- [ ] Sim — a funcionalidade existe, mas a entrada **é estritamente validada** contra uma allow-list  
- [ ] Sim — a funcionalidade existe e aceita URLs fornecidas pelo usuário  

### Controles de validação ou blacklists são aplicados ao destino solicitado?
- [ ] Sim — uma **allow-list** estrita de domínios/IPs confiáveis é aplicada  
- [ ] Sim — uma **deny-list** é utilizada (ex: bloqueando `127.0.0.1`, `169.254.169.254`)  
- [ ] Não — **nenhuma validação** ou filtragem é aplicada à entrada  

### É possível burlar os filtros através de encoding, redirecionamentos ou DNS rebinding?
- [ ] Não — os filtros são robustos e tratam redirecionamentos/resolução DNS de forma segura  
- [ ] Sim — o bypass **é possível** usando URL encoding ou formatos de IP alternativos (hex, octal)  
- [ ] Sim — o bypass **é possível** através de redirecionamentos HTTP 3xx para alvos internos  
- [ ] Sim — o bypass **é possível** através de ataques de DNS rebinding para resolver IPs internos  

### A aplicação consegue alcançar serviços de metadados internos ou interfaces locais?
- [ ] Não — rede local e endpoints de metadados de nuvem **não estão acessíveis**  
- [ ] Sim — o acesso ao localhost (`127.0.0.1`) ou serviços de loopback **é possível**  
- [ ] Sim — o acesso a serviços de metadados de nuvem (ex: AWS/GCP/Azure IMDS) **é possível** *(Crítica)*  

### Qual é a natureza da resposta do SSRF?
- [ ] Blind — nenhuma resposta ou metadado é retornado ao usuário  
- [ ] Parcial — metadados da resposta (headers, tamanho, tempo de resposta) são retornados ao usuário  
- [ ] Total — o corpo completo da resposta do recurso interno **é refletido** para o usuário  

---