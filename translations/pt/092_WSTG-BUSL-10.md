## WSTG-BUSL-10 — Testar a Funcionalidade de Pagamento

O teste da funcionalidade de pagamento envolve a identificação de falhas de lógica de negócio que permitem a um atacante contornar gateways de pagamento, manipular totais de pedidos ou falsificar sinais de transação bem-sucedida. Essas vulnerabilidades geralmente residem na comunicação entre a aplicação, o navegador do cliente e processadores de pagamento de terceiros, como Stripe, PayPal ou sistemas de contabilidade internos. Um atacante pode tentar modificar parâmetros de preço em trânsito, enviar quantidades negativas para reduzir o custo total ou realizar o replay de callbacks de transações bem-sucedidas para processar pedidos sem o pagamento real. A exploração bem-sucedida leva a perdas financeiras diretas, discrepâncias de inventário e potencial comprometimento da integridade do e-commerce da aplicação.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### O valor da transação é validado no lado do servidor antes do processamento?
- [ ] Sim — o valor é estritamente validado contra o banco de dados de produtos no servidor *(Mais seguro)*  
- [ ] Sim — o valor é validado, mas o bypass da lógica **é possível** via parameter pollution ou troca de moeda  
- [ ] Não — o valor da transação **pode** ser modificado na requisição do lado do cliente e é aceito pelo backend  

### A quantidade de itens pode ser manipulada para resultar em um total zero ou negativo?
- [ ] Não — quantidades negativas ou zero são rejeitadas pela lógica de validação da aplicação  
- [ ] Sim — quantidades negativas são aceitas no carrinho, mas o total é corrigido no checkout  
- [ ] Sim — quantidades negativas **podem** ser usadas para reduzir o total geral do pedido ou obter um crédito *(Crítico)*  

### A aplicação gerencia de forma segura callbacks ou webhooks de gateways de pagamento?
- [ ] Sim — os callbacks exigem uma assinatura criptográfica válida que **não pode** ser falsificada  
- [ ] Sim — as assinaturas são verificadas, mas a chave secreta é fraca ou a verificação **pode** ser contornada  
- [ ] Não — a aplicação depende de callbacks não autenticados ou não verificados para confirmar o status do pagamento  

### A aplicação é vulnerável a race conditions durante a fase de checkout ou pagamento?
- [ ] Não — integridade transacional e mecanismos de bloqueio de banco de dados estão **habilitados**  
- [ ] Sim — race conditions **são possíveis**, mas não impactam o preço final ou o processamento do pedido  
- [ ] Sim — requisições simultâneas **podem** levar a múltiplos processamentos para um único pagamento ou "double-spending" de vales-presente  

### Códigos de desconto ou pontos de fidelidade estão sujeitos a manipulação ou reutilização?
- [ ] Não — os códigos são validados para uso único e restrições de lógica são **aplicadas**  
- [ ] Sim — os códigos podem ser reutilizados ou aplicados a itens não elegíveis, mas o impacto é limitado  
- [ ] Sim — atacantes **podem** aplicar múltiplos códigos conflitantes ou burlar prazos de validade e limites de uso  

---