## WSTG-ATHN-07 — Teste de Políticas de Senhas Fracas

O teste de políticas de senhas fracas envolve a avaliação dos requisitos de complexidade, comprimento e entropia impostos por uma aplicação durante a criação de contas e atualizações de credenciais. Políticas insuficientes comprometem diretamente a confidencialidade das contas de utilizador, tornando-as suscetíveis a ataques de dicionário automatizados, tentativas de Brute Force e Credential Stuffing. Estas vulnerabilidades residem tipicamente em endpoints de registo, fluxos de redefinição de senha e painéis administrativos de gestão de utilizadores. Do ponto de vista de um atacante, uma política permissiva reduz significativamente o custo computacional e o tempo necessário para comprometer credenciais com sucesso utilizando wordlists comuns ou quebra de senhas baseada em padrões.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Baixa* |

> *A severidade torna-se Alta se a aplicação carecer de mecanismos de bloqueio de conta ou de Rate Limiting para prevenir adivinhação automatizada.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Ferramentas:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### O comprimento mínimo de senha é imposto pela aplicação?
- [ ] Sim — o comprimento mínimo é de 12+ caracteres *(Mais seguro)*  
- [ ] Sim — o comprimento mínimo está entre 8 e 11 caracteres  
- [ ] Sim — o comprimento mínimo é **inferior a** 8 caracteres  
- [ ] Não — nenhum comprimento mínimo é imposto  

### Os requisitos de complexidade (maiúsculas, minúsculas, dígitos, símbolos) são impostos?
- [ ] Sim — múltiplas classes de caracteres são obrigatórias e o bypass **não é possível**  
- [ ] Sim — as classes de caracteres são sugeridas, mas **não** impostas  
- [ ] Não — nenhum requisito de complexidade é aplicado  

### A aplicação bloqueia senhas comuns/fracas ou senhas que contenham dados específicos do utilizador?
- [ ] Sim — senhas fracas conhecidas (ex: "password123") e senhas baseadas no nome de utilizador **não podem** ser utilizadas  
- [ ] Sim — senhas comuns são bloqueadas, mas dados específicos do utilizador (ex: nome de utilizador, e-mail) **podem** ser utilizados  
- [ ] Não — qualquer string que cumpra os requisitos de comprimento/complexidade é aceite  

### Os requisitos de complexidade ou comprimento podem ser contornados via modificação no client-side?
- [ ] Não — as políticas são estritamente impostas no **server-side**  
- [ ] Sim — as políticas são impostas via JavaScript e **podem** ser contornadas intercetando o pedido (request)  

### A política de senhas é comunicada claramente ao utilizador sem vazar detalhes de implementação?
- [ ] Sim — a política é visível durante a introdução de dados e fornece mensagens de falha genéricas  
- [ ] Sim — a política é visível, mas as mensagens de falha revelam lacunas de lógica ou regex específicas  
- [ ] Não — a política **não** é visível, forçando a descoberta por tentativa e erro  

---