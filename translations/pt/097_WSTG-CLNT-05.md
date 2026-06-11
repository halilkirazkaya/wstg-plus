## WSTG-CLNT-05 — Teste de Injeção de CSS (CSS Injection)

A Injeção de CSS (CSS Injection) ocorre quando uma aplicação permite que a entrada fornecida pelo usuário influencie as Folhas de Estilo em Cascata (CSS) de uma página web sem a devida sanitização ou escape. Embora muitas vezes seja percebida como um problema estético, atacantes podem aproveitar a injeção de CSS para exfiltrar dados sensíveis, como tokens CSRF ou IDs de sessão, utilizando seletores de atributos e propriedades de imagem de fundo (`background-image`) para disparar requisições externas. Esta vulnerabilidade ocorre tipicamente em endpoints onde as preferências do usuário (ex: temas personalizados, cores de fonte) são refletidas dentro de blocos `<style>` ou atributos `style` inline. Do ponto de vista de um atacante, a exploração bem-sucedida pode levar ao UI redressing, phishing por meio de modificação não autorizada do layout, ou exfiltração furtiva de dados em ambientes onde Políticas de Segurança de Conteúdo (CSP) rígidas poderiam bloquear ataques baseados em JavaScript.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Média / Alta* |

> *A gravidade torna-se Alta se o ponto de injeção permitir a exfiltração de atributos sensíveis, como tokens CSRF, ou se puder ser utilizado para UI redressing em fluxos de trabalho sensíveis.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### A aplicação reflete entrada controlada pelo usuário dentro de contextos CSS?
- [ ] Não — a entrada **não** é refletida em tags de estilo, atributos ou arquivos CSS  
- [ ] Sim — a entrada é refletida, mas estritamente limitada a valores alfanuméricos seguros  
- [ ] Sim — a entrada é refletida dentro de um atributo `style` ou bloco `<style>`  

### Os metacaracteres e funções específicas de CSS são devidamente sanitizados?
- [ ] Sim — caracteres como `{`, `}`, `:` e funções como `url()` estão **devidamente escapados**  
- [ ] Sim — a sanitização existe, mas o bypass **é possível** via codificação ou comentários CSS  
- [ ] Não — nenhuma sanitização é aplicada aos metacaracteres CSS  

### A exfiltração de dados é possível utilizando seletores CSS?
- [ ] Não — os seletores **não podem** disparar requisições externas ou vazar dados  
- [ ] Sim — a exfiltração parcial de dados **é possível** via seletores de atributo e `background-image`  
- [ ] Sim — a exfiltração total de tokens sensíveis **é possível** utilizando técnicas automatizadas de força bruta (Brute Force)  

### Uma Política de Segurança de Conteúdo (CSP) mitiga o risco de injeção de CSS?
- [ ] Sim — o `style-src` está restrito a 'self' e o `img-src` ou `connect-src` **estão restritos**  
- [ ] Sim — a CSP está presente, mas utiliza 'unsafe-inline' ou permite domínios externos curinga (wildcards)  
- [ ] Não — nenhuma CSP foi implementada para prevenir o carregamento de recursos externos via CSS  

### A vulnerabilidade pode ser usada para realizar UI redressing ou phishing?
- [ ] Não — o escopo da injeção é muito limitado para modificar o layout significativamente  
- [ ] Sim — a modificação da UI **é possível**, permitindo a sobreposição de elementos maliciosos  
- [ ] Sim — o controle total do layout da página **é possível**, facilitando ataques de phishing altamente convincentes  

---