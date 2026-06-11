## WSTG-BUSL-02 — Testar a Capacidade de Forjar Requisições

Forjar requisições dentro de um contexto de lógica de negócio envolve a manipulação de parâmetros definidos pela aplicação para realizar ações fora do fluxo de trabalho pretendido ou do nível de autorização. Atacantes visam processos de várias etapas, como sistemas de checkout ou registro de contas, onde o estado é mantido via parâmetros no lado do cliente (client-side) que o servidor falha em revalidar contra uma fonte de backend confiável. Ao alterar campos ocultos, valores JSON ou parâmetros REST, como preços, quantidades ou flags de status interno, um atacante pode burlar requisitos de pagamento, realizar escalada de privilégios ou acionar mudanças de estado não pretendidas. Esta vulnerabilidade normalmente se manifesta em formulários web ou APIs com estado (stateful), onde o servidor confia implicitamente na representação do estado de negócio feita pelo cliente durante uma transação.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica* |

> *A severidade torna-se Crítica se a requisição forjada permitir perda financeira, ações administrativas não autorizadas ou o comprometimento total da conta (full account takeover).

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### A aplicação valida os parâmetros transacionais contra uma "fonte da verdade" no lado do servidor (server-side)?
- [ ] Sim — todos os parâmetros sensíveis (preço, cargo/role, ID) são validados contra o banco de dados e o bypass **não é possível**  
- [ ] Sim — a validação é aplicada, mas pode ser burlada via parameter pollution ou codificações alternativas  
- [ ] Não — a aplicação confia nos valores do lado do cliente para determinar o custo ou o resultado de uma transação *(Crítica)*  

### Valores críticos para o negócio (ex: quantidades, montantes) podem ser modificados para valores não autorizados?
- [ ] Não — valores negativos, valores zero ou valores excessivamente altos são rejeitados pelo servidor  
- [ ] Sim — o servidor aceita valores modificados, mas apenas dentro de uma faixa limitada  
- [ ] Sim — valores negativos ou zero **podem** ser enviados, levando a prejuízos financeiros ou bypass de lógica  

### Campos ocultos ou parâmetros de API são utilizados para manter o estado em processos de múltiplas etapas?
- [ ] Não — o estado é mantido de forma segura via sessões no servidor ou tokens assinados  
- [ ] Sim — o estado é passado via cliente, mas verificações de integridade (HMAC/Assinaturas) estão **habilitadas** e são robustas  
- [ ] Sim — o estado é passado via cliente e as verificações de integridade estão **desabilitadas** ou podem ser removidas  

### É possível forjar requisições que pulam etapas intermediárias obrigatórias em um fluxo de trabalho?
- [ ] Não — o servidor impõe uma sequência estrita de operações para cada transação  
- [ ] Sim — as etapas **podem** ser puladas, mas a ação final ainda requer dados válidos de etapas anteriores  
- [ ] Sim — a requisição final de "enviar" (submit) ou "concluir" **pode** ser forjada diretamente para burlar etapas de autorização ou pagamento  

---