## WSTG-CRYP-03 — Teste para Informações Sensíveis Enviadas via Canais Não Criptografados

A transmissão de dados sensíveis por canais não criptografados, como HTTP simples, expõe as informações à interceptação e modificação por meio de ataques Man-in-the-Middle (MITM). Esta vulnerabilidade ocorre tipicamente quando as aplicações web falham em aplicar o Transport Layer Security (TLS) em todos os endpoints, particularmente em componentes legados, formulários de login ou durante a transição inicial de HTTP para HTTPS. Do ponto de vista de um atacante, isso permite o sniffing passivo de credenciais de autenticação, tokens de sessão e informações de identificação pessoal (PII) em segmentos de rede comprometidos ou não confiáveis, como Wi-Fi público. Garantir que todos os dados sensíveis sejam encapsulados em um túnel TLS robusto é fundamental para manter a confidencialidade e integridade das interações do usuário e prevenir o session hijacking.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Média |

> *A severidade torna-se Alta se credenciais de autenticação, identificadores de sessão ou PII forem transmitidos em texto claro (cleartext).

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### A aplicação permite comunicação via HTTP não criptografado para endpoints sensíveis?
- [ ] Não — todo o tráfego da aplicação é estritamente forçado via HTTPS *(Mais seguro)*  
- [ ] Sim — páginas não sensíveis são acessíveis via HTTP, mas endpoints sensíveis **não são**  
- [ ] Sim — endpoints sensíveis (login, checkout, perfil) **estão** acessíveis via HTTP não criptografado *(Alto Risco)*  

### Existe um redirecionamento global de HTTP para HTTPS?
- [ ] Sim — a aplicação realiza um redirecionamento 301/302 para HTTPS para todas as requisições HTTP  
- [ ] Sim — o redirecionamento é realizado apenas para endpoints sensíveis específicos  
- [ ] No — nenhum redirecionamento de HTTP para HTTPS **é aplicado**  

### O HTTP Strict Transport Security (HSTS) está implementado?
- [ ] Sim — o HSTS está **habilitado** com um `max-age` longo e inclui as diretivas `includeSubDomains` e `preload`  
- [ ] Sim — o HSTS está **habilitado**, mas carece das diretivas `includeSubDomains` ou `preload`  
- [ ] Não — o HSTS **não está habilitado** no servidor da aplicação  

### Os cookies sensíveis estão protegidos contra transmissão em texto claro?

| Nome do Cookie | Flag Secure Presente | Flag Secure Ausente |
|----------------|:--------------------:|:-------------------:|
| `SessionID`    |                      |                     |
| `AuthToken`    |                      |                     |
| `CSRF-Token`   |                      |                     |

### A aplicação transmite dados sensíveis para APIs de terceiros via canais não criptografados?
- [ ] Não — todas as chamadas de API externas são realizadas através de túneis criptografados (HTTPS)  
- [ ] Sim — algumas telemetrias não sensíveis são enviadas via HTTP  
- [ ] Sim — API keys, PII ou credenciais **são** enviadas para terceiros via HTTP não criptografado  

---