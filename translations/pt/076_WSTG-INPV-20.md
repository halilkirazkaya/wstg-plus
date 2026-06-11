## WSTG-INPV-20 — Mass Assignment

Mass Assignment, também conhecido como Overposting ou Insecure Deserialization de parâmetros, ocorre quando uma aplicação web vincula automaticamente parâmetros de requisições HTTP recebidas a propriedades de objetos internos sem a filtragem adequada de atributos. Esta vulnerabilidade é prevalente em frameworks MVC modernos e APIs RESTful onde dados JSON, XML ou codificados em formulários são desserializados diretamente em modelos de dados do lado da aplicação. Atacantes exploram esse comportamento injetando parâmetros adicionais não listados em uma requisição — como `isAdmin`, `role` ou `account_balance` — para modificar campos sensíveis que os desenvolvedores não pretendiam expor ao controle do usuário. A exploração bem-sucedida normalmente resulta em escalonamento de privilégios (privilege escalation), modificação não autorizada de dados ou o bypass de fluxos de lógica de negócio críticos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Média* |

> *A severidade torna-se Alta se atributos administrativos, níveis de permissão ou campos de faturamento puderem ser modificados.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### A aplicação utiliza vinculação automática (automatic binding) para os parâmetros de requisição recebidos?
- [ ] Não — os parâmetros são mapeados manualmente para variáveis específicas ou objetos seguros  
- [ ] Sim — a vinculação automática é usada, mas **white-lists** rigorosas ou DTOs (Data Transfer Objects) são aplicados  
- [ ] Sim — a vinculação automática é usada e atributos internos sensíveis **podem** ser modificados  

### Atributos administrativos ou relacionados a permissões podem ser injetados com sucesso?
- [ ] Não — parâmetros injetados são ignorados, removidos ou causam um erro de validação  
- [ ] Sim — parâmetros extras são aceitos, mas **não** afetam o estado do objeto no backend  
- [ ] Sim — injetar parâmetros como `is_admin`, `role` ou `permissions` escala privilégios **com sucesso** *(Crítico)*  

### É possível modificar a lógica de negócio sensível ou campos financeiros via overposting?
- [ ] Não — campos como `price`, `balance` ou `status` estão protegidos contra mass assignment  
- [ ] Sim — atributos de negócio sensíveis **podem** ser modificados ao adicioná-los ao corpo da requisição JSON ou formulário  

### A aplicação revela estruturas de objetos internos que facilitam a adivinhação de parâmetros?
- [ ] Não — as respostas incluem apenas campos públicos pretendidos e a documentação é restrita  
- [ ] Sim — campos de objetos internos são visíveis em respostas GET, tornando a descoberta de parâmetros **possível**  
- [ ] Sim — a documentação da API (Swagger/OpenAPI) lista explicitamente propriedades internas que **podem** ser atribuídas  

---