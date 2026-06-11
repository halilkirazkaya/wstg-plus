## WSTG-INFO-04 — Enumerar Aplicações no Servidor Web

A enumeração de aplicações é o processo de identificar todas as aplicações e serviços web hospedados em um único servidor web ou endereço IP. Como os servidores web modernos frequentemente hospedam múltiplos hosts virtuais (Virtual Hosts), ambientes de teste (staging) legados ou interfaces administrativas em portas e caminhos não padrão, a falha em identificar esses ativos ocultos deixa uma parte significativa da superfície de ataque (attack surface) sem testes. Atacantes utilizam técnicas como transferências de zona DNS (DNS zone transfers), virtual host brute-forcing e pesquisas de IP reverso (reverse IP lookups) para descobrir esses pontos de entrada esquecidos ou obscurecidos, que são frequentemente menos seguros do que a aplicação de produção principal.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Informativa / Baixa |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Ferramentas:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Múltiplos endereços IP ou aliases de DNS estão associados à infraestrutura alvo?
- [ ] Não — apenas um único IP e registro A (A-record) existem  
- [ ] Sim — múltiplos endereços IP ou aliases **foram** identificados, expandindo o escopo  

### O servidor web hospeda múltiplos Hosts Virtuais (vhosts) no mesmo IP?
- [ ] Não — apenas o domínio primário é servido pelo servidor  
- [ ] Sim — hosts virtuais adicionais foram identificados, mas o acesso **não é possível** (ex: 403 Forbidden)  
- [ ] Sim — hosts virtuais ocultos, de desenvolvimento ou de staging (homologação) **foram** identificados e estão acessíveis  

### Existem aplicações hospedadas em portas não padrão?
- [ ] Não — apenas portas padrão (80/443) estão abertas  
- [ ] Sim — portas não padrão estão abertas, mas **não** hospedam aplicações web  
- [ ] Sim — aplicações web adicionais **foram** identificadas em portas não padrão (ex: 8080, 8443, 9000)  

### Aplicações distintas estão mapeadas para caminhos de URI específicos?
- [ ] Não — uma única aplicação serve todo o diretório raiz  
- [ ] Sim — múltiplas aplicações **foram** descobertas através de enumeração de diretórios (ex: `/api`, `/portal`, `/backup`)  

### Serviços de reconhecimento de terceiros revelam aplicações históricas ou secundárias?
- [ ] Não — sem descobertas significativas no Shodan, Censys ou Wayback Machine  
- [ ] Sim — snapshots históricos ou escaneamentos de serviço revelam aplicações que **estão** atualmente ativas, mas não vinculadas  

---