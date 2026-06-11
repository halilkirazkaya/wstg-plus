## WSTG-APIT-01 — Reconhecimento de API

O reconhecimento de API é o processo sistemático de identificar e mapear a superfície de ataque da API para descobrir endpoints, métodos suportados e estruturas de dados subjacentes. Atacantes realizam esta fase para descobrir APIs não documentadas (Shadow APIs), versões obsoletas com vulnerabilidades legadas e arquivos de documentação expostos, como definições Swagger ou OpenAPI, que revelam a lógica de negócio interna. Ao analisar padrões de URL previsíveis, arquivos JavaScript do lado do cliente e resultados de Brute Force de diretórios, um adversário pode construir um mapa abrangente da API para identificar alvos para ataques de Broken Object Level Authorization (BOLA) ou Mass Assignment. Este reconhecimento normalmente visa caminhos comuns, como `/api/v1/`, `/swagger.json` ou `/graphql`, para obter um roteiro da funcionalidade de backend da aplicação.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Informativa / Baixa |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Ferramentas:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Os arquivos de documentação da API (Swagger, OpenAPI, WSDL) estão acessíveis publicamente?
- [ ] Não — A documentação da API **não** está acessível ou é restrita por autenticação  
- [ ] Sim — A documentação está acessível, mas requer credenciais válidas  
- [ ] Sim — A documentação da API (ex: `/swagger-ui.html`) está **acessível publicamente** sem autenticação  

### Endpoints de API não documentados ou "shadow" podem ser descobertos via fuzzing?
- [ ] Não — O Fuzzing de diretórios e endpoints não revelou caminhos não documentados  
- [ ] Sim — Endpoints não documentados foram encontrados, mas **não** podem ser acessados sem autorização  
- [ ] Sim — Endpoints não documentados foram encontrados e **podem** ser acessados sem autorização válida  

### A aplicação revela múltiplas versões da API (ex: /v1/, /v2/, /beta/)?
- [ ] Não — Apenas a versão atual e protegida (hardened) da API está acessível  
- [ ] Sim — Versões legadas existem, mas implementam os **mesmos** controles de segurança da versão atual  
- [ ] Sim — Versões legadas (ex: `/v1/`) estão acessíveis e **carecem** dos controles de segurança das versões mais recentes  

### Recursos do lado do cliente (JavaScript/Apps Móveis) vazam estruturas de endpoints ou chaves de API?
- [ ] Não — O código do lado do cliente **não** contém caminhos de API codificados (hardcoded) ou chaves sensíveis  
- [ ] Sim — O código do lado do cliente contém mapas de endpoints de API, mas **nenhuma** chave sensível  
- [ ] Sim — Chaves de API, secrets ou URLs de endpoints apenas internos **estão** codificados (hardcoded) em recursos do lado do cliente  

### Parâmetros ou cabeçalhos ocultos são detectáveis em endpoints conhecidos?
- [ ] Não — O Fuzzing de parâmetros (ex: via `Arjun`) **não** revelou entradas ocultas  
- [ ] Sim — Parâmetros ocultos (ex: `debug=true`, `admin=1`) foram descobertos, mas **não** são funcionais  
- [ ] Sim — Parâmetros ocultos foram descobertos e **podem** ser usados para alterar o comportamento da aplicação  

---