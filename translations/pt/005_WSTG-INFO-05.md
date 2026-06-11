## WSTG-INFO-05 — Revisão do Conteúdo da Página Web para Vazamento de Informações

A revisão do conteúdo da página web envolve a inspeção manual e automatizada das respostas da aplicação, incluindo arquivos HTML, JavaScript e CSS, para identificar informações sensíveis expostas involuntariamente aos usuários. Os atacantes analisam o código-fonte do lado do cliente (client-side) em busca de comentários de desenvolvedores, campos de formulário ocultos, caminhos internos do servidor, chaves de API (API keys) codificadas (hardcoded) e informações de depuração (debugging) que possam auxiliar em fases posteriores de exploração. Esse vazamento ocorre frequentemente quando os desenvolvedores esquecem de remover metadados ou código de depuração antes de mover para a produção, potencialmente revelando falhas de lógica ou detalhes da infraestrutura de backend. Do ponto de vista de um atacante, isso fornece um método de baixo esforço para mapear a estrutura interna da aplicação e descobrir pontos de entrada negligenciados sem acionar alertas de segurança ou interagir com a lógica de backend do servidor.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média* |

> *A severidade torna-se Alta se credenciais codificadas (hardcoded), chaves privadas ou variáveis de ambiente internas sensíveis forem descobertas.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Ferramentas:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Existem comentários de desenvolvedores contendo informações sensíveis no código-fonte?
- [ ] Não — nenhum comentário ou apenas comentários genéricos e não sensíveis foram encontrados  
- [ ] Sim — existem comentários, mas **não** contêm lógica ou dados sensíveis  
- [ ] Sim — os comentários **revelam** caminhos internos, consultas SQL ou instruções administrativas  
- [ ] Sim — os comentários **revelam** credenciais ou notas sensíveis de desenvolvedores *(Alta)*  

### Os campos de entrada HTML ocultos contêm dados sensíveis ou informações de estado?
- [ ] Não — nenhum campo oculto encontrado ou eles contêm apenas tokens não sensíveis  
- [ ] Sim — campos ocultos são usados para estado, mas estão **criptografados** ou **assinados**  
- [ ] Sim — os campos ocultos contêm senhas em **texto simples**, funções de usuário (user roles) ou IDs internos  

### Existem segredos codificados (hardcoded), chaves de API ou endereços IP internos presentes em arquivos JavaScript?
- [ ] Não — nenhuma string sensível ou segredos encontrados em arquivos JS  
- [ ] Sim — os arquivos JS contêm chaves de API públicas com restrições **adequadas**  
- [ ] Sim — os arquivos JS contêm chaves de API **não protegidas**, IPs internos ou endpoints privados  
- [ ] Sim — os arquivos JS contêm credenciais administrativas ou chaves privadas **codificadas (hardcoded)** *(Crítica)*  

### A aplicação revela versões de frameworks ou caminhos de arquivos internos no código-fonte?
- [ ] Não — as informações do framework e caminhos de arquivos estão **minificados** ou **ofuscados**  
- [ ] Sim — os nomes e versões dos frameworks estão **visíveis**, mas corrigidos (patched)  
- [ ] Sim — caminhos de arquivos internos ou caminhos absolutos do servidor estão **expostos** no código-fonte  

### O código-fonte está propenso a vazamentos devido à falta de minificação ou ofuscação?
- [ ] Não — o código de produção está **totalmente minificado** e sem comentários  
- [ ] Sim — o código está minificado, mas os comentários **permanecem** no código-fonte  
- [ ] Sim — os source maps (arquivos `.map`) estão **publicamente acessíveis**, permitindo a reconstrução total do código-fonte  

---