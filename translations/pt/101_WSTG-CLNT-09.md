## WSTG-CLNT-09 — Testing for Clickjacking

O Clickjacking, também conhecido como UI redressing, ocorre quando um atacante utiliza camadas transparentes ou opacas para enganar um usuário, levando-o a clicar em um botão ou link em outra página quando sua intenção era clicar na página de nível superior. Esta vulnerabilidade compromete a integridade das ações do usuário, potencialmente levando à modificação não autorizada de dados, alterações de conta ou transferências financeiras realizadas em nome da vítima. Os atacantes normalmente exploram isso incorporando o site alvo dentro de um `<iframe>` em um site malicioso controlado e sobrepondo-o com conteúdo enganoso ou iscas atrativas. Ocorre com maior frequência em páginas que executam ações sensíveis de mudança de estado, como botões de "Excluir Conta", formulários de redefinição de senha ou painéis de configuração administrativa.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se ações sensíveis (ex: transferências de fundos, exclusão de conta ou escalação de privilégios) puderem ser acionadas via clickjacking.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Ferramentas:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### A aplicação implementa cabeçalhos no lado do servidor para prevenir o enquadramento (framing) não autorizado?
- [ ] Sim — `X-Frame-Options` ou `Content-Security-Policy` **estão aplicados** e previnem o framing *(Mais seguro)*  
- [ ] Sim — os cabeçalhos estão presentes, mas **mal configurados** (ex: `X-Frame-Options: ALLOW-FROM`, que carece de amplo suporte dos navegadores)  
- [ ] Não — nenhum cabeçalho de proteção contra framing **está aplicado**  

### A diretiva `frame-ancestors` da `Content-Security-Policy` (CSP) está implementada?
- [ ] Sim — `frame-ancestors 'none'` ou `'self'` **está aplicado**  
- [ ] Sim — `frame-ancestors` está presente, mas **permite** origens não confiáveis ou excessivamente amplas  
- [ ] Não — a diretiva está **ausente** ou a CSP não está implementada  

### O cabeçalho `X-Frame-Options` (XFO) está configurado corretamente para suporte a navegadores legados?
- [ ] Sim — `DENY` ou `SAMEORIGIN` **está aplicado**  
- [ ] Sim — o XFO está presente, mas é **ignorado** por navegadores modernos porque existe uma diretiva CSP `frame-ancestors`  
- [ ] Não — o cabeçalho XFO está **ausente** ou definido com um valor inseguro  

### As proteções contra framing podem ser burladas utilizando técnicas conhecidas?
- [ ] Não — o bypass **não é possível** via técnicas padrão (double-framing, etc.)  
- [ ] Sim — a proteção **pode** ser burlada devido a regex ou lógica fraca na white-list do `frame-ancestors`  
- [ ] Sim — a proteção **pode** ser burlada porque a aplicação depende exclusivamente de frame-busting baseado em JavaScript  

### Uma ação sensível é explorável através de uma prova de conceito (PoC) de clickjacking?
- [ ] Não — o framing está bloqueado ou nenhuma ação sensível é alcançável  
- [ ] Sim — o UI redressing **é possível**, mas requer interação complexa do usuário  
- [ ] Sim — o UI redressing **é possível** e permite ações imediatas de mudança de estado *(Crítico)*  

---