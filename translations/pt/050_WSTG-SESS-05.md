## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) é uma vulnerabilidade na qual um invasor engana o navegador de uma vítima para realizar uma ação indesejada em um site diferente onde a vítima está autenticada no momento. Este exploit aproveita o comportamento do navegador de anexar automaticamente credenciais "ambientais", como cookies de sessão ou cabeçalhos de Authorization, a solicitações de saída. Os invasores normalmente visam operações que alteram o estado, como alteração de senhas, atualização de endereços de e-mail ou execução de transferências financeiras, hospedando scripts maliciosos ou formulários ocultos em um site de terceiros. A exploração bem-sucedida pode levar ao total account takeover ou modificação não autorizada de dados sem o conhecimento ou consentimento do usuário.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Status do Teste** | Não Performed |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se a ação vulnerável permitir account takeover, escalonamento de privilégios ou transações financeiras não autorizadas.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Ferramentas:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer para hospedagem de PoC)`

### Os tokens anti-CSRF estão implementados para solicitações que alteram o estado?
- [ ] Sim — tokens únicos e criptograficamente fortes são exigidos para todas as ações que alteram o estado  
- [ ] Sim — os tokens estão presentes, mas **não** são únicos por sessão ou são previsíveis  
- [ ] Não — os tokens anti-CSRF **não** estão implementados  

### A validação do token anti-CSRF no lado do servidor (server-side) é robusta?
- [ ] Sim — o servidor valida estritamente o token e o bypass **não é possível**  
- [ ] Sim — a validação é realizada, mas **pode** ser burlada removendo o parâmetro do token  
- [ ] Sim — a validação é realizada, mas **pode** ser burlada fornecendo um token vazio ou fictício (dummy)  
- [ ] Sim — a validação é realizada, mas o token **não** está vinculado à sessão do usuário  

### A aplicação depende de métodos facilmente burláveis para proteção contra CSRF?
- [ ] Não — a aplicação não depende apenas de cabeçalhos fracos ou verificações de origem  
- [ ] Sim — a proteção depende exclusivamente do cabeçalho `Referer` ou `Origin`, que **pode** ser falsificado ou removido  
- [ ] Sim — a proteção depende da verificação do cabeçalho `X-Requested-With`, que **pode** ser burlada através de configurações incorretas de CORS  

### Os cookies de sessão estão configurados com o atributo `SameSite`?
- [ ] Sim — `SameSite` está definido como `Strict` ou `Lax` em todos os cookies relacionados à sessão  
- [ ] Não — o atributo `SameSite` está **ausente**, assumindo o comportamento padrão do navegador  
- [ ] Não — `SameSite` está explicitamente definido como `None` sem a flag `Secure` ou mitigações adicionais  

### A proteção CSRF pode ser burlada alterando o método de solicitação HTTP?
- [ ] Não — a proteção é aplicada independentemente do método HTTP utilizado  
- [ ] Sim — os tokens são validados apenas em solicitações `POST`, mas a ação **pode** ser realizada via `GET`  
- [ ] Sim — a alteração do método (ex: para `PUT` ou `DELETE`) burla a verificação do token  

---