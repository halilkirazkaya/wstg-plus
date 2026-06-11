## WSTG-CRYP-01 — Teste de Segurança Fraca na Camada de Transporte

O teste de segurança fraca na camada de transporte (Transport Layer Security) envolve a análise da implementação e configuração do SSL/TLS para garantir a confidencialidade e integridade dos dados durante o trânsito. Atacantes visam falhas de configuração, como protocolos obsoletos (SSLv2, TLS 1.0), Cipher Suites fracos (NULL, RC4 ou anônimos) e certificados inválidos ou com assinaturas fracas para realizar ataques de Man-in-the-Middle (MitM). Este teste geralmente foca nos endpoints do servidor web ou balanceadores de carga da aplicação onde a comunicação criptografada é estabelecida. A exploração bem-sucedida permite que um adversário intercepte dados sensíveis, realize o sequestro de sessões (Session Hijacking) ou force o Downgrade de conexões para estados inseguros através de Protocol Downgrade ou descriptografia de tráfego.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Média* |

> *A Severidade é Alta se dados sensíveis (PII, credenciais, session tokens) forem transmitidos; Média para tráfego informacional geral.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Protocolos TLS/SSL obsoletos ou inseguros são suportados?
- [ ] Não — apenas TLS 1.2 e TLS 1.3 estão **habilitados**  
- [ ] Sim — TLS 1.0 ou TLS 1.1 estão **habilitados**  
- [ ] Sim — SSLv2 ou SSLv3 estão **habilitados** *(Crítico)*  

### Cipher Suites fracos ou inseguros são aceitos pelo servidor?
- [ ] Não — apenas ciphers fortes e modernos (ex: AES-GCM, ChaCha20) são **suportados**  
- [ ] Sim — ciphers com criptografia fraca (ex: 3DES, RC4) ou chaves curtas são **suportados**  
- [ ] Sim — ciphers NULL, anônimos (ADH) ou de exportação são **suportados** *(Crítico)*  

### O certificado do servidor é válido e confiável?
- [ ] Sim — o certificado é válido, corresponde ao domínio e é assinado por uma **CA confiável**  
- [ ] Não — o certificado está **expirado** ou utiliza um **algoritmo de assinatura fraco** (ex: SHA-1)  
- [ ] Não — o certificado é **autoassinado (self-signed)** ou ocorre divergência no nome do domínio (**mismatch**)  

### O servidor suporta Secure Renegotiation e previne ataques de Downgrade?
- [ ] Sim — Secure Renegotiation é **suportado** e o TLS Fallback SCSV está **habilitado**  
- [ ] Não — Secure Renegotiation **não é suportado**, permitindo potencialmente injeção via MitM  
- [ ] Não — O servidor está vulnerável a ataques **POODLE** ou **ROBOT**  

### O HTTP Strict Transport Security (HSTS) está implementado corretamente?

| Cabeçalho | Presente | Ausente |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Sim — o cabeçalho HSTS está presente com um `max-age` longo e `includeSubDomains` **habilitado**  
- [ ] Sim — o cabeçalho HSTS está presente, mas falta o `includeSubDomains` ou possui um **max-age curto**  
- [ ] Não — o cabeçalho HSTS **não está presente**  

---