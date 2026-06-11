## WSTG-SESS-02 — Teste de Atributos de Cookies

O gerenciamento de sessão frequentemente depende de cookies HTTP para manter o estado, tornando os atributos de segurança desses cookies um alvo primário para atacantes. Ao omitir ou configurar incorretamente atributos como `HttpOnly`, `Secure` e `SameSite`, as aplicações expõem tokens de sessão ao roubo via Cross-Site Scripting (XSS), interceptação em canais não criptografados ou ataques de Cross-Site Request Forgery (CSRF). Pentesters examinam essas flags para determinar se um atacante pode exfiltrar identificadores de sessão do Document Object Model (DOM) do navegador ou forçar o navegador a enviá-los em requisições cross-origin. A configuração adequada dos atributos é uma medida fundamental de defesa em profundidade para garantir que tokens sensíveis permaneçam confinados a contextos autorizados e seguros.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Ferramentas:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Análise da Implementação de Flags de Cookie
| Atributo | Presente | Ausente |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### A flag `HttpOnly` é aplicada a cookies de sessão sensíveis?
- [ ] Sim — aplicada a **todos** os cookies de sessão sensíveis *(Mais seguro)*  
- [ ] Sim — aplicada apenas a alguns cookies de sessão  
- [ ] Não — a flag **não é aplicada**, permitindo o acesso via scripts client-side *(Crítico)*  

### A flag `Secure` é imposta para cookies transmitidos via HTTPS?
- [ ] Sim — aplicada a **todos** os cookies transmitidos via TLS  
- [ ] Não — a flag **não é aplicada**, permitindo que os cookies sejam enviados via HTTP em texto claro  

### O atributo `SameSite` é utilizado para mitigar riscos de CSRF?
- [ ] Sim — configurado como `Strict` ou `Lax` para cookies de sessão  
- [ ] Sim — configurado como `None`, mas com a presença da flag `Secure`  
- [ ] Não — o atributo está **ausente** ou configurado como `None` **sem** `Secure`  

### Os atributos `Domain` e `Path` estão restritos ao escopo mínimo necessário?
- [ ] Sim — limitados ao subdomínio e caminho da aplicação **específicos**  
- [ ] Não — o escopo é muito **amplo** (ex: configurado para um domínio pai ou raiz `/`), aumentando a superfície de ataque  

### Os cookies de sessão sensíveis expiram adequadamente através dos atributos `Expires` ou `Max-Age`?
- [ ] Sim — datas de expiração apropriadas estão configuradas  
- [ ] Não — os cookies são persistentes com tempos de vida excessivamente **longos**  
- [ ] Não — os cookies são apenas de sessão, mas **carecem** de imposição de timeout no lado do servidor  

---