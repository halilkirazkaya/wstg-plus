## WSTG-BUSL-03 — Teste de Verificações de Integridade

O teste de verificação de integridade foca-se em verificar se a aplicação garante que os dados permanecem inalterados e confiáveis à medida que se movem através de vários processos de negócio ou entre o cliente e o servidor. Os atacantes visam esta lógica através da manipulação de parâmetros críticos — tais como preços de produtos, quantidades, privilégios de utilizador ou estados de transação — dentro de pedidos HTTP, campos ocultos ou cookies. Se a aplicação carecer de verificação no lado do servidor (server-side) ou de controlos de integridade robustos como Hashing ou HMACs, um adversário pode manipular estes valores para cometer fraude, escalar a sua própria autoridade ou contornar as restrições de negócio pretendidas. Este teste é vital para qualquer aplicação que processe transações financeiras, transições de estado sensíveis ou fluxos de trabalho de várias etapas, onde a saída de uma fase serve como a entrada confiável para a próxima.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Estado do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a falha de integridade permitir o roubo financeiro, escalada de privilégios não autorizada ou modificação de registos persistentes.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Os parâmetros de negócio sensíveis estão expostos à manipulação no lado do cliente (client-side tampering)?
- [ ] Não — os valores sensíveis são geridos exclusivamente no lado do servidor  
- [ ] Sim — valores como preço, quantidade ou sinalizadores de estado (status flags) **estão** expostos, mas encriptados  
- [ ] Sim — os valores **estão** expostos em texto simples (plain text) dentro de campos ocultos, parâmetros de URL ou cookies  

### É aplicado um mecanismo de integridade (ex: HMAC, Checksum) aos parâmetros controlados pelo cliente?
- [ ] Sim — uma verificação de integridade robusta **é aplicada** e verificada em cada pedido (request)  
- [ ] Sim — uma verificação de integridade **é aplicada**, mas utiliza um algoritmo fraco/previsível ou um segredo codificado rigidamente (hardcoded)  
- [ ] Não — não **são aplicadas** verificações de integridade a parâmetros sensíveis  

### A verificação de integridade pode ser contornada (bypassed) ou recalculada por um atacante?
- [ ] Não — a verificação de assinatura é estritamente aplicada e o bypass **não é possível**  
- [ ] Sim — a verificação de assinatura pode ser contornada ao omitir o parâmetro de assinatura  
- [ ] Sim — a assinatura **pode** ser recalculada porque a chave secreta ou a lógica está exposta/é fraca  

### A aplicação realiza a revalidação de backend dos dados contra uma fonte confiável?
- [ ] Sim — o servidor revalida todos os valores contra a base de dados e o bypass **não é possível**  
- [ ] Sim — o servidor realiza validação parcial, mas o bypass **é possível** através de casos extremos (edge cases) específicos  
- [ ] Não — o servidor confia nos dados fornecidos pelo cliente sem verificação de backend *(Crítico)*  

### É possível manipular o estado de um processo de múltiplas etapas (multi-step process)?
- [ ] Não — o estado da sessão (session state) é gerido no lado do servidor e não pode ser manipulado  
- [ ] Sim — a informação de estado é passada via cliente e a manipulação (tampering) **é possível** para saltar etapas ou repetir ações  

---