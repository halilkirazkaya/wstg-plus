## WSTG-ATHN-01 — Testing for Credentials Transported over an Encrypted Channel

O teste de credenciais transportadas por um canal criptografado garante que dados de autenticação sensíveis, como nomes de usuário, senhas e tokens de sessão, estejam protegidos contra interceptação em trânsito. Atacantes posicionados na rede — como por meio de ataques Man-in-the-Middle (MitM) em Wi-Fi público ou redes internas comprometidas — podem usar ferramentas de packet sniffing para capturar credenciais em texto claro se o TLS/SSL não for devidamente aplicado. Esta vulnerabilidade ocorre tipicamente em endpoints de login, formulários de redefinição de senha e headers de autenticação de API onde a aplicação falha em exigir HTTPS ou realiza o redirecionamento de HTTP de forma incorreta. A exploração bem-sucedida concede ao atacante acesso total às contas de usuário, levando ao comprometimento total dos dados do usuário e potencial movimentação lateral (lateral movement) dentro da aplicação.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Ferramentas:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### O HTTPS é imposto em todo o processo de autenticação?
- [ ] Sim — O HSTS está **habilitado** e todo o tráfego é estritamente forçado via HTTPS  
- [ ] Sim — O redirecionamento para HTTPS ocorre, mas o HSTS **não está habilitado**  
- [ ] Não — As credenciais **podem** ser enviadas através de HTTP não criptografado  

### As páginas e formulários de login são servidos por um canal criptografado?
- [ ] Sim — A página que contém o formulário de login é entregue via HTTPS  
- [ ] Não — A página de login é servida via HTTP, permitindo form-action hijacking ou sniffing de credenciais  

### A flag `Secure` é aplicada a todos os cookies de sessão sensíveis?
- [ ] Sim — A flag `Secure` está **aplicada** a todos os cookies de autenticação e sessão  
- [ ] Sim — A flag `Secure` está **aplicada** apenas a alguns cookies  
- [ ] Não — A flag `Secure` **não está aplicada**, permitindo que os cookies sejam exfiltrados via requisições não criptografadas  

### O servidor suporta versões fracas de TLS ou conjuntos de cifras (cipher suites) inseguros?
- [ ] Não — Apenas TLS moderno (1.2+) e cifras fortes estão **habilitados**  
- [ ] Sim — Versões legadas (TLS 1.0/1.1) ou cifras fracas (RC4, DES, CBC) são **suportadas**  

### A aplicação impede a transmissão de credenciais para domínios de terceiros através de canais não criptografados?
- [ ] Sim — Nenhum dado sensível é enviado para domínios externos ou todas as chamadas externas são forçadas via HTTPS  
- [ ] Não — Headers de autenticação ou credenciais são enviados para endpoints de terceiros (ex: analytics ou SSO) via HTTP  

---