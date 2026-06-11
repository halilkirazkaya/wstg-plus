## WSTG-CRYP-02 — Testando Padding Oracle

Vulnerabilidades de Padding Oracle ocorrem quando o processo de decodificação de uma aplicação vaza informações sobre se o preenchimento (padding) de um texto cifrado (ciphertext) decifrado é válido ou inválido. Ao observar essas respostas de canal lateral (side-channel)—como códigos de status HTTP distintos, mensagens de erro específicas ou variações no tempo de resposta—um invasor pode decifrar dados sem conhecer a chave de criptografia e, potencialmente, re-criptografar texto simples (plaintext) arbitrário. Esta vulnerabilidade geralmente se manifesta em cookies de sessão criptografados, tokens de autenticação ou parâmetros de URL que utilizam cifras de bloco no modo Cipher Block Chaining (CBC) com preenchimento PKCS#7. A exploração permite a perda total da confidencialidade e pode levar ao comprometimento da integridade através da construção de blocos criptografados forjados.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica* |

> *A severidade é Crítica se a vulnerabilidade residir no gerenciamento de sessão, tokens de identidade ou criptografia de PII (Informações de Identificação Pessoal).

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Uma cifra de bloco é usada no modo CBC para dados sensíveis?
- [ ] Não — a aplicação utiliza criptografia autenticada (AES-GCM/ChaCha20-Poly1305)  
- [ ] Sim — a aplicação utiliza cifras de bloco, mas o preenchimento (padding) **não** é necessário (ex: cifras de fluxo ou modo CTR)  
- [ ] Sim — a aplicação utiliza o modo CBC e o preenchimento (padding) **é aplicado**  

### O servidor revela a validade do preenchimento através de canais laterais?
- [ ] Não — o servidor retorna mensagens de erro uniformes e tempos de resposta consistentes  
- [ ] Sim — o servidor retorna diferentes códigos de status HTTP (ex: 200 vs 500) para erros de preenchimento  
- [ ] Sim — o servidor retorna mensagens de erro específicas (ex: "Invalid Padding", "Decryption failed")  
- [ ] Sim — o servidor apresenta diferenças de tempo mensuráveis durante falhas de decodificação  

### A decodificação automatizada do texto cifrado é possível?
- [ ] Não — os canais laterais **não** são estáveis o suficiente para exploração  
- [ ] Sim — o texto cifrado (ciphertext) **pode** ser parcialmente decifrado (últimos blocos)  
- [ ] Sim — a recuperação total do texto simples (plaintext) **é possível** utilizando ferramentas como `PadBuster`  

### O invasor pode realizar bit-flipping ou forjar textos cifrados válidos?
- [ ] Não — as verificações de integridade (HMAC) são validadas **antes** da decodificação  
- [ ] Sim — não há verificação de integridade presente, mas a construção do payload **não** é viável  
- [ ] Sim — a construção de texto cifrado arbitrário **é possível**, permitindo escalonamento de privilégios ou injeção de dados  

---