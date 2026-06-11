## WSTG-ATHN-06 — Teste de Fraqueza no Cache do Navegador

A fraqueza no cache do navegador ocorre quando informações sensíveis são armazenadas no cache local do navegador e podem ser recuperadas por um usuário não autorizado com acesso à mesma máquina física. Essa falta de cabeçalhos de controle de cache adequados permite que dados potencialmente sensíveis, como informações pessoais, detalhes da conta ou identificadores de sessão, persistam após o usuário ter feito logout ou fechado a sessão. Atacantes ou usuários subsequentes em terminais compartilhados ou públicos podem explorar isso navegando pelo histórico do navegador, usando o botão "Voltar" ou inspecionando arquivos de cache locais para exfiltrar respostas armazenadas em cache. Garantir a implementação de diretivas rigorosas como `Cache-Control: no-store` e `Pragma: no-cache` é crítico para qualquer aplicação que manipule conteúdo autenticado, para assegurar que dados sensíveis nunca sejam gravados no disco.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média* |

> *A severidade torna-se Média se a aplicação for acessada frequentemente a partir de quiosques compartilhados/públicos ou contiver dados PII/PHI altamente sensíveis.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Ferramentas:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Cabeçalhos de controle de cache apropriados estão presentes em páginas autenticadas ou sensíveis?
- [ ] Sim — `Cache-Control: no-store, no-cache` e `Pragma: no-cache` estão **presentes** e **configurados corretamente**  
- [ ] Sim — alguns cabeçalhos estão presentes, mas o `no-store` está **ausente**, permitindo o cache em disco  
- [ ] Não — nenhum cabeçalho relacionado ao cache é **aplicado**, dependendo do comportamento padrão do navegador  

### A aplicação utiliza a diretiva `private` para dados específicos do usuário?
- [ ] Sim — `Cache-Control: private` está **habilitado** para todo o conteúdo personalizado para evitar o cache por proxies  
- [ ] Não — o conteúdo está marcado como `public` ou carece da diretiva `private`, tornando-o **passível de cache** por proxies intermediários  

### Informações sensíveis estão acessíveis através do botão "Voltar" do navegador após um logout bem-sucedido?
- [ ] Não — o navegador solicita nova autenticação ou a página **não é carregada** a partir do cache local  
- [ ] Sim — informações sensíveis **ainda estão visíveis** através do botão "Voltar" após a sessão ter sido encerrada  

### Arquivos não-HTML sensíveis (ex: PDFs, exportações CSV, respostas JSON de API) estão protegidos contra cache?
- [ ] Não — nenhum arquivo não-HTML sensível é gerado ou manipulado pela aplicação  
- [ ] Sim — cabeçalhos de controle de cache rigorosos são **aplicados** a todos os downloads de arquivos sensíveis e endpoints de API  
- [ ] Não — downloads sensíveis ou respostas de API são **armazenados** no cache local do navegador ou em diretórios temporários  

### A aplicação utiliza `Expires: 0` ou uma data no passado para evitar o uso de dados obsoletos?
- [ ] Sim — o cabeçalho `Expires` está definido como `0` ou uma data histórica, **garantindo** que o navegador trate o conteúdo como imediatamente obsoleto  
- [ ] Não — o cabeçalho `Expires` está **ausente** ou definido para uma data futura em páginas sensíveis  

---