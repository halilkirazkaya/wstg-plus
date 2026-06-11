## WSTG-INPV-05 — SQL Injection

O SQL Injection ocorre quando entradas fornecidas pelo usuário são incorporadas em consultas SQL sem a devida parametrização ou sanitização, permitindo que atacantes manipulem a lógica da consulta. A exploração bem-sucedida pode levar à exfiltração não autorizada de dados, bypass de autenticação, modificação de dados e, em alguns casos, execução remota de código (Remote Code Execution) por meio de recursos do banco de dados, como `xp_cmdshell` ou `LOAD_FILE()`. Esta vulnerabilidade afeta comumente formulários de login, funcionalidades de busca, parâmetros de API REST, cabeçalhos HTTP e qualquer endpoint que construa consultas SQL dinâmicas a partir de entradas controladas pelo usuário. Do ponto de vista do atacante, identificar um ponto de injeção é o principal portão de entrada para o comprometimento total do banco de dados e potencial movimentação lateral dentro da infraestrutura.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Crítica |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Ferramentas:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Os parâmetros controláveis pelo usuário são processados utilizando métodos seguros de acesso ao banco de dados?
- [ ] Sim — a aplicação utiliza ORM ou consultas parametrizadas exclusivamente e o bypass **não é possível**  
- [ ] Sim — consultas parametrizadas são utilizadas, mas o bypass **é possível** através de casos específicos (ex: cláusulas `ORDER BY`)  
- [ ] Não — a concatenação direta de strings é utilizada para construir consultas SQL **sem** parametrização  

### O mecanismo de autenticação pode ser contornado via SQL Injection?
- [ ] Não — os parâmetros de login **não** são vulneráveis a injeção  
- [ ] Sim — o bypass de autenticação **é possível** utilizando payloads baseados em tautologia (ex: `' OR 1=1 --`)  

### A exfiltração de dados é possível através de técnicas in-band ou out-of-band?
- [ ] Não — nenhuma exfiltração de dados foi identificada  
- [ ] Sim — exfiltração baseada em UNION ou baseada em erro **é possível**  
- [ ] Sim — exfiltração blind (Booleana ou baseada em tempo) **é possível**  
- [ ] Sim — exfiltração out-of-band (DNS/HTTP) **é possível**  

### As funções da aplicação apresentam vulnerabilidades de SQL Injection de segunda ordem?
- [ ] Não — os dados armazenados são manipulados de forma segura ao serem recuperados para consultas posteriores  
- [ ] Sim — a entrada do usuário é armazenada e posteriormente utilizada em uma consulta SQL insegura **sem** validação  

### Os privilégios da conta de serviço do banco de dados estão restritos ao mínimo necessário?
- [ ] Sim — a conta de serviço possui o **menor privilégio** (ex: limitada a tabelas/ações específicas)  
- [ ] Não — a conta de serviço possui privilégios elevados (ex: `DROP TABLE`, privilégios de `FILE`, ou `xp_cmdshell` **habilitado**)  

---