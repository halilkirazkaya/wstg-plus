## WSTG-CRYP-04 — Teste de Primitivas Criptográficas Fracas

Primitivas criptográficas fracas envolvem o uso de algoritmos obsoletos ou inseguros, como MD5, SHA-1, DES ou RC4, que são suscetíveis a ataques de colisão, análise de frequência ou descriptografia por força bruta (Brute Force). Essas fraquezas ocorrem tipicamente em hashing de senhas, criptografia de dados em repouso, geração de tokens de sessão ou verificação de assinatura digital no backend da aplicação. Do ponto de vista de um atacante, a identificação dessas primitivas permite o comprometimento da integridade e confidencialidade dos dados, como a quebra (cracking) de hashes interceptados para recuperar senhas em texto claro ou a forja de tokens de autenticação. Aplicações modernas devem, em vez disso, utilizar primitivas padrão da indústria, resistentes a colisões e de alta entropia, como Argon2, Bcrypt ou AES-GCM, para garantir uma proteção robusta contra criptoanálise.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Algoritmos de hashing depreciados, como MD5 ou SHA-1, são utilizados para o armazenamento de dados sensíveis?
- [ ] Não — a aplicação utiliza algoritmos modernos resistentes a colisões, como Argon2 ou Bcrypt  
- [ ] Sim — algoritmos depreciados são utilizados, mas **apenas** para verificações de integridade não sensíveis à segurança  
- [ ] Sim — algoritmos depreciados **são utilizados** para senhas ou tokens sensíveis *(Alta)*  

### Cifras de criptografia simétrica fracas, como DES, 3DES ou RC4, estão sendo empregadas no momento?
- [ ] Não — é utilizado AES (128 bits ou superior) em um modo seguro, como GCM ou CBC  
- [ ] Sim — cifras legadas estão **habilitadas** para compatibilidade reversa, mas **não** são o padrão  
- [ ] Sim — cifras legadas são o método principal para proteger dados em repouso ou em trânsito  

### A aplicação implementa lógica criptográfica "própria" (roll-your-own) ou não padronizada?
- [ ] Não — a aplicação adere estritamente a bibliotecas criptográficas padrão e revisadas por pares  
- [ ] Sim — existem wrappers personalizados, mas as primitivas subjacentes **são** padrão da indústria  
- [ ] Sim — algoritmos criptográficos proprietários ou lógica personalizada **são aplicados** a dados sensíveis *(Crítica)*  

### Salts criptográficos e vetores de inicialização (IVs) são gerenciados de acordo com as melhores práticas?
- [ ] Sim — os salts são exclusivos por usuário e os IVs são imprevisíveis para cada operação  
- [ ] Sim — salts são utilizados, mas **não** são exclusivos em todos os registros  
- [ ] Não — os salts são estáticos, hardcoded ou ausentes, e os IVs **são reutilizados** em múltiplas sessões  

### O comprimento de bits das chaves assimétricas (RSA, Diffie-Hellman) é suficiente para os padrões modernos?
- [ ] Sim — as chaves RSA/DH possuem 2048 bits ou mais, ou Curva Elíptica (ECC) é utilizada  
- [ ] Não — as chaves possuem menos de 2048 bits, mas o bypass **não** é trivial  
- [ ] Não — as chaves possuem 1024 bits ou menos, tornando-as **vulneráveis** a fatoração ou ataques computacionais  

---