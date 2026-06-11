## WSTG-CLNT-14 — Teste de Reverse Tabnabbing

O Reverse Tabnabbing é uma vulnerabilidade client-side onde uma página aberta via `target="_blank"` mantém uma referência funcional para a página de origem (parent page) através do objeto `window.opener`. Isso permite que um site de destino controlado por um atacante redirecione programaticamente a aba original e confiável para uma URL maliciosa, geralmente uma página de phishing projetada para capturar credenciais. O ataque explora a confiança intrínseca do usuário na aba original, pois é improvável que ele perceba que a página que acabou de deixar navegou para um domínio diferente. As práticas de segurança modernas exigem o uso dos atributos `rel="noopener"` ou `rel="noreferrer"` em tags de âncora, ou a configuração da propriedade `opener` como null em JavaScript, para evitar essa comunicação entre janelas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média* |

> *A severidade é Baixa na maioria dos navegadores modernos, pois agora eles definem `target="_blank"` como `rel="noopener"` por padrão. A severidade torna-se Média se a aplicação for direcionada a navegadores legados ou utilizar `window.open()` sem definir a propriedade `opener` como null.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Existem links que abrem em uma nova janela ou aba?
- [ ] Não — nenhum link utiliza `target="_blank"` ou redirecionamento equivalente baseado em JavaScript  
- [ ] Sim — `target="_blank"` é usado exclusivamente em links internos e confiáveis  
- [ ] Sim — `target="_blank"` é usado em links externos ou URLs fornecidas pelo usuário  

### A relação `window.opener` está restrita nas tags de âncora?
- [ ] Sim — `rel="noopener"` ou `rel="noreferrer"` é **aplicado consistentemente** a todos os links com `target="_blank"` *(Mais seguro)*  
- [ ] Sim — os atributos são aplicados, mas estão **ausentes** em links de alto risco ou gerados pelo usuário  
- [ ] Não — os atributos **não são aplicados**, permitindo que a página filha acesse `window.opener`  

### A aplicação está vulnerável ao Reverse Tabnabbing via JavaScript `window.open()`?
- [ ] Não — `window.open()` não é utilizado ou define explicitamente a propriedade `opener` como null  
- [ ] Sim — `window.open()` é utilizado e a referência `opener` **permanece acessível** para a janela filha  

### Um atacante pode controlar o destino de um link que abre em uma nova aba?
- [ ] Não — todos os destinos de links estão codificados (hardcoded) e apontam para domínios confiáveis  
- [ ] Sim — os destinos dos links são fornecidos pelo usuário (ex: perfis de redes sociais, links de bio) e **não** são sanitizados para proteções contra tabnabbing  

---