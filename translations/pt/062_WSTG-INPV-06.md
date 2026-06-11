## WSTG-INPV-06 — Teste de LDAP Injection

A LDAP Injection (Injeção de LDAP) ocorre quando uma aplicação incorpora dados fornecidos pelo usuário em filtros LDAP (Lightweight Directory Access Protocol) sem a devida sanitização ou escaping, permitindo que um atacante manipule a lógica da consulta. Ao injetar caracteres especiais, tais como asteriscos, parênteses e operadores lógicos, os atacantes podem modificar o filtro de busca para ignorar mecanismos de autenticação ou exfiltrar informações sensíveis do diretório, incluindo nomes de usuário, associações a grupos e atributos organizacionais. Esta vulnerabilidade manifesta-se tipicamente em funcionalidades como buscas em diretórios corporativos, portais de funcionários ou sistemas de Single Sign-On (SSO) que fazem interface com Active Directory ou OpenLDAP. Do ponto de vista de um atacante, a exploração bem-sucedida envolve frequentemente técnicas de blind (cegas) para enumerar a estrutura do diretório ou valores de atributos bit a bit quando a saída direta da consulta é suprimida pela aplicação.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Ferramentas:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### A aplicação utiliza LDAP para autenticação ou buscas em diretórios?
- [ ] Não — O LDAP **não é utilizado** na arquitetura da aplicação  
- [ ] Sim — O LDAP é utilizado e todas as entradas são estritamente sanitizadas ou parametrizadas  
- [ ] Sim — O LDAP é utilizado e a entrada do usuário é concatenada nos filtros **sem** o devido escaping  

### Os metacaracteres de LDAP são devidamente escapados ou filtrados na entrada do usuário?
- [ ] Sim — caracteres como `(`, `)`, `&`, `|`, `*` e `\` **não são permitidos** ou são corretamente escapados  
- [ ] Não — metacaracteres **podem** ser injetados para alterar a estrutura do filtro LDAP  

### O mecanismo de autenticação pode ser burlado via injeção?
- [ ] Não — a lógica de login **não é suscetível** a manipulação de consulta  
- [ ] Sim — a autenticação **pode** ser burlada utilizando injeção de OR lógico (`|`) ou wildcard (`*`) nos campos de nome de usuário ou senha  

### É possível realizar a exfiltração cega de dados (blind data exfiltration) do serviço de diretório?
- [ ] Não — os resultados de busca são limitados e não são observadas diferenças de tempo ou booleanas  
- [ ] Sim — atributos de diretório **podem** ser enumerados bit a bit via técnicas de Boolean-based Blind Injection  
- [ ] Sim — registros completos do diretório **são** refletidos na resposta da aplicação devido à manipulação da consulta  

### Componentes secundários da aplicação (ex: mailers, SSO) estão vulneráveis a injeção de atributos baseada em LDAP?
- [ ] Não — os atributos LDAP são manipulados de forma segura em todos os componentes  
- [ ] Sim — um atacante **pode** modificar seus próprios atributos (ex: e-mail, associação a grupos) via injeção para escalar privilégios ou redirecionar comunicações  

---