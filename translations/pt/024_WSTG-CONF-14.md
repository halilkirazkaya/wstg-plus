## WSTG-CONF-14 — Testar Outras Má Configurações de Cabeçalhos de Segurança HTTP

O teste para más configurações de cabeçalhos de segurança HTTP envolve verificar a presença e a implementação correta de cabeçalhos defensivos que fornecem proteção essencial contra vetores de ataque comuns baseados na web. Embora esses cabeçalhos não corrijam vulnerabilidades de código subjacentes, sua ausência aumenta significativamente o risco de ataques man-in-the-middle (MITM), exploração de MIME-sniffing e vazamento não intencional de dados entre origens (cross-origin) via referrers. Do ponto de vista de um atacante, a ausência de cabeçalhos de segurança são indicadores de alta visibilidade de um ambiente mal endurecido, facilitando frequentemente cadeias de exploração mais complexas, como session hijacking ou exfiltração de informações. Essas configurações são normalmente avaliadas no nível do servidor e devem ser aplicadas de forma consistente em todos os tipos de resposta, incluindo API endpoints e recursos estáticos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Auditoria de Presença de Cabeçalhos de Segurança
| Cabeçalho | Presente | Ausente | Configuração Recomendada |
|-----------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` or `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Específico por recurso, ex: `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### Como os cabeçalhos de segurança são aplicados em todo o escopo da aplicação?
- [ ] Sim — os cabeçalhos são aplicados globalmente por meio de um balanceador de carga central ou proxy reverso e **não podem** ser substituídos  
- [ ] Sim — os cabeçalhos são aplicados às respostas do documento principal, mas estão **ausentes** em respostas de API ou ativos estáticos  
- [ ] No — os cabeçalhos são aplicados de forma inconsistente ou estão **ausentes** em todo o domínio da aplicação  

### O cabeçalho Strict-Transport-Security (HSTS) está configurado corretamente?
- [ ] Sim — `max-age` está definido para uma longa duração (ex: 2 anos) e `includeSubDomains` está **habilitado**  
- [ ] Sim — `max-age` está definido, mas `includeSubDomains` está **desabilitado**, deixando os subdomínios vulneráveis  
- [ ] No — o cabeçalho HSTS **não está presente** ou o `max-age` está definido como 0, permitindo ataques de downgrade de protocolo  

### A Referrer-Policy evita o vazamento de parâmetros de URL sensíveis?
- [ ] Sim — a política está definida como `no-referrer` ou `same-origin` *(Mais segura)*  
- [ ] Sim — a política está definida como `strict-origin-when-cross-origin`  
- [ ] No — a política **não está definida** ou está definida como `unsafe-url`, potencialmente vazando tokens ou PII sensíveis em cabeçalhos referer  

### O cabeçalho X-Content-Type-Options está prevenindo o MIME-sniffing?
- [ ] Sim — a diretiva `nosniff` está **presente** e corretamente implementada  
- [ ] No — o cabeçalho está **ausente**, potencialmente permitindo que um navegador execute arquivos não executáveis (ex: imagens) como scripts  

---