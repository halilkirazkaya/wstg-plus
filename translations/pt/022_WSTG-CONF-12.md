## WSTG-CONF-12 — Content Security Policy (CSP)

A Content Security Policy (CSP) é um mecanismo crítico de defesa em profundidade implementado através de cabeçalhos de resposta HTTP para mitigar riscos como Cross-Site Scripting (XSS), clickjacking e ataques de injeção de dados. Ao definir quais recursos dinâmicos têm permissão para carregar, uma CSP bem configurada restringe a capacidade de um atacante de executar scripts não autorizados ou exfiltrar dados sensíveis para domínios externos. Pentesters avaliam a política em busca de diretivas excessivamente permissivas, o uso de palavras-chave inseguras como `'unsafe-inline'` e a dependência de CDNs inseguras que possam facilitar bypasses. Uma CSP fraca ou ausente não cria uma vulnerabilidade diretamente, mas aumenta significativamente o impacto de ataques de injeção bem-sucedidos ao falhar em fornecer uma camada secundária de proteção.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Ferramentas:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### O cabeçalho Content Security Policy (CSP) está presente nas respostas da aplicação?
- [ ] Sim — o cabeçalho `Content-Security-Policy` está **presente** e é **aplicado**  
- [ ] Sim — `Content-Security-Policy-Report-Only` está **presente** para testes  
- [ ] Não — nenhum cabeçalho CSP está **presente** nas respostas  

### As diretivas `script-src` ou `default-src` estão devidamente restritas?
- [ ] Sim — as diretivas utilizam whitelisting rigoroso, nonces ou hashes e o bypass **não é possível**  
- [ ] Sim — as diretivas estão **configuradas corretamente**, mas a whitelist inclui CDNs conhecidas passíveis de bypass  
- [ ] Não — as diretivas utilizam wildcards (`*`) ou esquemas `data:`, tornando o bypass **possível**  

### A política permite a execução de scripts inline inseguros ou eval?
- [ ] Não — `'unsafe-inline'` e `'unsafe-eval'` **não estão presentes**  
- [ ] Sim — `'unsafe-inline'` está **presente**, mas protegido por um `nonce` ou `hash`  
- [ ] Sim — `'unsafe-inline'` ou `'unsafe-eval'` estão **habilitados** sem proteções adicionais *(Crítico)*  

### A aplicação está protegida contra clickjacking através da diretiva `frame-ancestors`?
- [ ] Sim — `frame-ancestors` está definido como `'none'` ou `'self'`  
- [ ] Sim — `frame-ancestors` está **habilitado**, mas permite domínios de terceiros confiáveis específicos  
- [ ] Não — `frame-ancestors` está **ausente**, dependendo do legado `X-Frame-Options` ou de nenhuma proteção  

### Existe um mecanismo para reportar violações de CSP?
- [ ] Sim — `report-uri` ou `report-to` está **configurado** e ativo  
- [ ] Não — o relatório de violações está **desabilitado** ou **não configurado**  

---