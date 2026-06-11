## WSTG-ATHN-10 — Teste de Autenticação Fraca em Canais Alternativos

O teste de autenticação fraca em canais alternativos envolve a identificação e análise de caminhos secundários para acesso à conta — como APIs móveis, fluxos de recuperação de senha, interfaces de suporte (help desk) ou subdomínios legados — que podem não aplicar o mesmo rigor de segurança que o portal web principal. Esses canais alternativos frequentemente carecem de autenticação multifator (MFA) robusta, Rate Limiting rigoroso ou requisitos complexos de senha, criando um "elo mais fraco" que compromete todo o sistema de autenticação. Atacantes visam especificamente esses pontos de entrada negligenciados para realizar Credential Stuffing ou burlar o MFA, explorando discrepâncias na forma como diferentes interfaces lidam com a verificação de identidade. Garantir a paridade em todas as superfícies de autenticação é essencial para evitar o acesso não autorizado e manter a integridade da sessão e dos dados do usuário.


| Campo | Valor |
|---|---|
| **ID WSTG** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Canais de autenticação alternativos estão presentes e foram identificados?
- [ ] Não — existe apenas um único canal de autenticação  
- [ ] Sim — múltiplos canais identificados (ex: API de aplicativo móvel, cliente desktop, portal legado, serviços SOAP)  

### Os canais alternativos aplicam paridade de segurança com o canal principal?
- [ ] Sim — todos os canais aplicam requisitos de complexidade de senha e MFA **idênticos**  
- [ ] Sim — os canais aplicam complexidade de senha, mas o MFA **não é obrigatório** ou **pode** ser burlado  
- [ ] Não — os canais alternativos possuem requisitos de autenticação **significativamente mais fracos**  

### O Rate Limiting e o bloqueio de conta são consistentes em todos os canais?
- [ ] Sim — o Rate Limiting e o bloqueio são **aplicados de forma consistente** em todos os endpoints  
- [ ] Sim — os controles estão presentes, mas o bypass **é possível** em canais específicos (ex: API móvel)  
- [ ] Não — o Rate Limiting ou o bloqueio **não são aplicados** a caminhos de autenticação secundários  

### O MFA pode ser burlado ao mudar para um canal alternativo?
- [ ] Não — o MFA é obrigatório e **não pode** ser burlado, independentemente do ponto de entrada  
- [ ] Sim — o MFA é exigido no portal web, mas **não é aplicado** na API móvel ou legada  

### Os fluxos de recuperação de senha ou "esqueci minha senha" são mais fracos que o login padrão?
- [ ] Não — os fluxos de recuperação utilizam verificação out-of-band forte e não vazam informações  
- [ ] Sim — os fluxos de recuperação usam perguntas de segurança fracas que **podem** ser facilmente adivinhadas ou pesquisadas  
- [ ] Sim — os fluxos de recuperação são vulneráveis a enumeração ou **não exigem** o mesmo nível de comprovação que um login padrão  

---