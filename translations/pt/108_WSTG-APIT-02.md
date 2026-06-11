## WSTG-APIT-02 — API Broken Object Level Authorization

A Quebra de Autorização em Nível de Objeto (Broken Object Level Authorization - BOLA), também conhecida como Referência Direta Insegura a Objetos (Insecure Direct Object Reference - IDOR), ocorre quando uma API falha ao validar se um utilizador possui as permissões adequadas para aceder ou manipular um recurso específico identificado por um ID. Os atacantes exploram esta vulnerabilidade através da enumeração sistemática ou adivinhação de identificadores — tais como IDs numéricos ou UUIDs — em caminhos de pedidos (request paths), parâmetros de consulta (query parameters) ou corpos JSON para aceder a dados pertencentes a outros utilizadores. Esta vulnerabilidade é o problema mais comum e impactante na segurança de APIs modernas, podendo levar à exfiltração massiva de dados, modificação não autorizada ou ao comprometimento total de contas (account takeover). Do ponto de vista de um atacante, o objetivo é identificar endpoints que aceitem identificadores de objetos e testar se a lógica de autorização no lado do servidor (server-side) está ausente ou pode ser contornada através da manipulação desses IDs.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Estado do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Ferramentas:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Os identificadores de objetos são previsíveis ou enumeráveis nos pedidos da API?
- [ ] Não — os identificadores são longos, aleatórios e criptograficamente seguros (ex.: UUIDv4)  
- [ ] Sim — os identificadores são números inteiros sequenciais (ex.: `101`, `102`)  
- [ ] Sim — os identificadores seguem um padrão previsível ou são derivados de informações públicas  

### A API realiza a validação do proprietário do objeto no lado do servidor para todos os pedidos?
- [ ] Sim — as verificações de autorização são aplicadas em todos os pedidos e o bypass **não é possível**  
- [ ] Sim — as verificações são aplicadas, mas o bypass **é possível** através de parameter pollution ou method tunneling  
- [ ] Não — a aplicação baseia-se exclusivamente na autenticação do utilizador sem verificar a propriedade do recurso específico  

### É possível aceder ou modificar o recurso de outro utilizador alterando o identificador?
- [ ] Não — o acesso não autorizado a recursos de outros utilizadores **não é possível**  
- [ ] Sim — o acesso de **leitura** não autorizado (Horizontal BOLA) **é possível**  
- [ ] Sim — a **modificação** ou **eliminação** não autorizada (Horizontal BOLA) **é possível**  

### A API permite aceder a objetos de nível administrativo ou de sistema através da substituição de IDs?
- [ ] Não — os recursos administrativos estão protegidos por camadas de autorização secundárias  
- [ ] Sim — o acesso ou modificação de recursos de nível de sistema (Vertical BOLA) **é possível**  

### A verificação de autorização pode ser contornada movendo o identificador para uma parte diferente do pedido?
- [ ] Não — a lógica de autorização é consistente, independentemente da localização do parâmetro  
- [ ] Sim — o bypass **é possível** quando o ID é movido do caminho do URL para o corpo JSON ou cabeçalhos (headers)  
- [ ] Sim — o bypass **é possível** quando são fornecidos múltiplos IDs e o servidor processa o ID não autorizado  

---