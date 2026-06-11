## WSTG-INFO-08 — Fingerprinting de Framework de Aplicação Web

O fingerprinting de um framework de aplicação web envolve a identificação da stack tecnológica específica e das versões de software utilizadas para construir e servir a aplicação. Atacantes analisam cabeçalhos de resposta HTTP, cookies específicos do framework, estruturas de arquivos previsíveis e artefatos de JavaScript exclusivos para determinar se o alvo está executando plataformas como React, Angular, Django ou Spring. Esta fase de reconhecimento é vital pois permite que um testador restrinja a superfície de ataque a vulnerabilidades conhecidas (CVEs) e fraquezas de configuração comuns específicas para aquele framework. Ao compreender a arquitetura subjacente, um atacante pode passar de testes genéricos para estratégias de Exploit altamente direcionadas.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Informativa |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Os cabeçalhos de resposta HTTP estão expondo o nome ou a versão do framework?
- [ ] Não — cabeçalhos como `X-Powered-By` ou `Server` **não** estão presentes ou estão sanitizados  
- [ ] Sim — o nome do framework está presente, mas a versão **está oculta**  
- [ ] Sim — tanto o nome quanto a versão do framework **estão expostos**  

### Os cookies ou as estruturas de diretórios específicos do framework são identificáveis?
- [ ] Não — artefatos comuns de framework (ex: caminhos `.js`, nomes de cookies) estão ofuscados ou renomeados  
- [ ] Sim — cookies específicos do framework (ex: `JSESSIONID`, `PHPSESSID`, `csrftoken`) **são** identificáveis  
- [ ] Sim — estruturas de diretórios (ex: `/wp-content/`, `_next/static/`) **estão** visíveis e confirmam o framework  

### As mensagens de erro ou comentários no código-fonte divulgam detalhes do framework?
- [ ] Não — o tratamento de erros é genérico e os comentários foram removidos  
- [ ] Sim — stack traces específicos do framework **estão habilitados** e visíveis nas respostas  
- [ ] Sim — o código-fonte HTML contém comentários ou tags de metadados que identificam o framework  

### Existem vulnerabilidades conhecidas associadas à versão do framework identificada?
- [ ] Não — o framework identificado está atualizado e **sem** vulnerabilidades públicas conhecidas  
- [ ] Sim — a versão do framework está desatualizada e CVEs específicos **podem ser** identificados  

---