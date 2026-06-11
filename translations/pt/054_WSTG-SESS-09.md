## WSTG-SESS-09 — Teste de Session Hijacking

O Session Hijacking ocorre quando um atacante captura, prevê ou fixa um identificador de sessão (SID) válido para obter acesso não autorizado à sessão ativa de um usuário. Esta vulnerabilidade geralmente se manifesta através de segurança insuficiente na camada de transporte, ausência de flags de segurança em cookies ou algoritmos de geração de SID previsíveis que permitem a um atacante contornar completamente a autenticação. Do ponto de vista de um atacante, a exploração bem-sucedida permite a personificação total da vítima sem a necessidade de suas credenciais, sendo frequentemente alcançada através de network sniffing, Cross-Site Scripting (XSS) ou ataques de Session Fixation.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Ferramentas:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Os identificadores de sessão estão protegidos durante o trânsito?
- [ ] Sim — A flag `Secure` está **habilitada** e o HSTS é estritamente aplicado  
- [ ] Sim — A flag `Secure` está **habilitada**, mas o HSTS está **desabilitado**  
- [ ] Não — A flag `Secure` **não é aplicada**, permitindo a interceptação do SID através de canais não criptografados *(Alta)*  

### O identificador de sessão está protegido contra acesso por scripts client-side?
- [ ] Sim — A flag `HttpOnly` é **aplicada** a todos os cookies relacionados à sessão  
- [ ] Não — A flag `HttpOnly` **não é aplicada**, permitindo a exfiltração do SID via XSS *(Crítica)*  

### A aplicação implementa Session Binding a atributos do cliente?
- [ ] Sim — A sessão está vinculada a atributos do cliente (IP/User-Agent) e a reutilização **não é possível**  
- [ ] Sim — O Session Binding está presente, mas o bypass **é possível** via header spoofing  
- [ ] Não — Não existe Session Binding; o SID **é válido** quando utilizado a partir de qualquer origem  

### O identificador de sessão é suficientemente aleatório para evitar a previsibilidade?
- [ ] Sim — O SID utiliza um gerador de números pseudo-aleatórios criptograficamente seguro (CSPRNG)  
- [ ] No — O comprimento do SID é insuficiente ou segue uma sequência/padrão **previsível**  

### As sessões simultâneas são gerenciadas para limitar a janela de ataque?
- [ ] Não — A aplicação impõe estritamente uma única sessão ativa por usuário  
- [ ] Sim — Múltiplas sessões simultâneas estão **habilitadas**, mas são monitoradas  
- [ ] Sim — Sessões simultâneas ilimitadas **são possíveis** em diferentes dispositivos/localizações  

---