## WSTG-INFO-01 — Realizar Descoberta e Reconhecimento em Motores de Busca para Vazamento de Informações

A descoberta em motores de busca envolve o aproveitamento de mecanismos de busca públicos, páginas em cache e serviços de indexação para identificar informações que a organização-alvo expôs involuntariamente. Atacantes utilizam operadores de busca avançados (Google Dorks) e serviços de indexação de terceiros para descobrir arquivos sensíveis, diretórios, portais de login, mensagens de erro e metadados que não deveriam estar acessíveis publicamente. Esta fase de reconhecimento é crítica porque requer interação direta zero com o alvo, tornando-a virtualmente indetectável, ao mesmo tempo que pode revelar pontos de entrada de alto valor, tais como painéis de administração expostos, arquivos de configuração, dumps de banco de dados e documentação interna.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Ferramentas:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Arquivos ou diretórios sensíveis estão indexados por motores de busca?
- [ ] Não — nenhum conteúdo sensível foi encontrado nos resultados dos motores de busca  
- [ ] Sim — arquivos de configuração, backups ou documentos internos **estão** indexados *(Crítico)*  

### O Google Dorking revela interfaces administrativas ou de login expostas?
- [ ] Não — painéis de administração e páginas de login **não** estão indexados  
- [ ] Sim — painéis de administração **estão** indexados, mas exigem autenticação multifator forte  
- [ ] Sim — painéis de administração **estão** indexados e acessíveis publicamente **sem** controles suficientes  

### Versões em cache de páginas estão expondo dados sensíveis que foram removidos desde então?
- [ ] Não — nenhum dado sensível foi encontrado em snapshots em cache  
- [ ] Sim — páginas em cache contêm credenciais, endereços IP internos ou informações sensíveis  

### O arquivo `robots.txt` está divulgando caminhos sensíveis involuntariamente?
- [ ] Não — o `robots.txt` **não** revela estruturas de diretórios sensíveis  
- [ ] Sim — o `robots.txt` lista caminhos sensíveis que auxiliam no reconhecimento do atacante  
- [ ] Não existe o arquivo `robots.txt`  

### Serviços de terceiros (`Shodan`, `Censys`, `Wayback Machine`) estão revelando exposição histórica ou atual?
- [ ] Não — sem descobertas significativas em serviços de indexação de terceiros  
- [ ] Sim — snapshots históricos ou varreduras de serviços revelam metadados sensíveis ou banners de serviços  

---