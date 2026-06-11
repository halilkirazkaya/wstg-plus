## WSTG-CONF-08 — Testar Políticas de Domínio Cruzado (Cross-Domain) de RIA

As políticas de domínio cruzado (Cross-Domain) de Rich Internet Applications (RIA) envolvem arquivos como `crossdomain.xml` e `clientaccesspolicy.xml`, que definem como clientes web — especificamente Adobe Flash e Microsoft Silverlight — tratam requisições de origem cruzada (Cross-Origin). Esses arquivos são críticos para a segurança pois substituem explicitamente a Política de Mesma Origem (SOP), determinando quais domínios externos têm permissão para interagir com a aplicação e ler suas respostas. Configurações excessivamente permissivas, como o uso de curingas (Wildcards) no atributo `domain`, permitem que atacantes hospedem objetos RIA maliciosos em sites de terceiros que podem exfiltrar dados sensíveis ou realizar ações em nome de usuários autenticados. Pentesters normalmente localizam esses arquivos na raiz do servidor web (Web Root) para avaliar o nível de confiança concedido a origens de terceiros e identificar potenciais vazamentos de dados entre origens.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Status do Teste** | Não Realizado |
| **Gravidade** | Média / Alta* |

> *A gravidade torna-se Alta se a aplicação processar dados sensíveis de usuários ou identificadores de sessão que possam ser exfiltrados por meio de uma política permissiva.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Os arquivos de política de domínio cruzado (`crossdomain.xml` ou `clientaccesspolicy.xml`) estão presentes na raiz do servidor web?
- [ ] Não — os arquivos **não puderam** ser encontrados no diretório raiz  
- [ ] Sim — o arquivo `crossdomain.xml` (Flash) **está** presente  
- [ ] Sim — o arquivo `clientaccesspolicy.xml` (Silverlight) **está** presente  
- [ ] Sim — ambos os arquivos de política RIA **estão** presentes  

### O atributo de domínio `allow-access-from` está restrito a origens confiáveis?
- [ ] Não — os arquivos existem, mas não contêm entradas `allow-access-from`  
- [ ] Sim — domínios específicos e confiáveis estão explicitamente na lista de permissões (Whitelisted) e o bypass **não é possível**  
- [ ] Sim — os domínios estão na lista de permissões, mas incluem terceiros não confiáveis ou subdomínios  
- [ ] Sim — um curinga `*` **é utilizado**, permitindo o acesso de **qualquer** origem *(Crítico)*  

### A política permite comunicação insegura via HTTP para recursos HTTPS?
- [ ] Não — `secure="true"` está definido (ou é o padrão), impedindo que origens HTTP acessem dados HTTPS  
- [ ] Sim — `secure="false"` está explicitamente definido, permitindo que atacantes MITM exfiltrem dados seguros  

### Os cabeçalhos HTTP estão excessivamente permissivos na configuração de domínio cruzado?
- [ ] Não — `allow-http-request-headers-from` está restrito ou **não habilitado**  
- [ ] Sim — cabeçalhos específicos são permitidos para domínios confiáveis  
- [ ] Sim — o curinga `*` é utilizado para cabeçalhos, permitindo que atacantes enviem cabeçalhos personalizados de qualquer domínio  

### A configuração de `site-control` (master-policy) é restritiva?
- [ ] Não — `permitted-cross-domain-policies` está definido como `none` ou `master-only`  
- [ ] Sim — a política permite que outros arquivos no servidor definam suas próprias regras (`all`)  

---