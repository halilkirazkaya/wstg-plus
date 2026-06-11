## WSTG-ERRH-02 — Teste de Stack Traces

Stack traces são gerados quando uma aplicação falha ao tratar uma exceção de forma adequada, resultando na exposição do estado de execução interno e da hierarquia de chamadas para o usuário final. Para um profissional de testes de invasão, esses rastros são inestimáveis, pois revelam o framework da aplicação, versões de bibliotecas, caminhos internos do sistema de arquivos e o fluxo lógico, o que reduz significativamente o esforço necessário para o reconhecimento (reconnaissance). A exploração (exploitation) envolve o acionamento intencional de erros por meio de entradas malformadas, tipos de dados inesperados ou violações de condições de contorno para forçar a aplicação a um estado instável e exfiltrar informações sobre a pilha tecnológica subjacente. Esse vazamento geralmente fornece o contexto necessário para identificar componentes de terceiros vulneráveis ou para criar exploits específicos para falhas lógicas descobertas nos caminhos de código expostos.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média* |

> *A severidade aumenta para Média se o stack trace revelar detalhes arquiteturais sensíveis, consultas de banco de dados ou parâmetros de configuração internos.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Stack traces completos são retornados ao cliente em caso de erros na aplicação?
- [ ] Não — a aplicação retorna páginas de erro genéricas ou mensagens de erro personalizadas  
- [ ] Sim — stack traces estão **habilitados**, mas apenas para módulos específicos e não sensíveis  
- [ ] Sim — stack traces completos estão **habilitados** e visíveis no corpo da resposta HTTP  

### As mensagens de erro expõem informações sensíveis do ambiente?
- [ ] Não — as mensagens de erro não contêm detalhes técnicos ou ambientais  
- [ ] Sim — as mensagens revelam **caminhos de arquivos internos** ou **estruturas de diretórios** do servidor  
- [ ] Sim — as mensagens revelam **schemas de banco de dados**, **consultas SQL** ou **versões de bibliotecas de terceiros**  

### Os stack traces podem ser acionados manipulando parâmetros de entrada?
- [ ] Não — entradas malformadas são tratadas adequadamente ou rejeitadas pela validação  
- [ ] Sim — os rastros podem ser acionados por **tipos de dados inesperados** (ex: enviar um array onde uma string é esperada)  
- [ ] Sim — os rastros são acionados por **null bytes**, **caracteres especiais** ou **valores de contorno**  

### Um manipulador de erros global é aplicado de forma consistente em toda a aplicação?
- [ ] Sim — um manipulador de exceções global (global exception handler) está **sendo aplicado consistentemente** em todos os endpoints testados  
- [ ] Sim — um manipulador está presente, mas **não é aplicado** a endpoints legados, API ou recém-adicionados  
- [ ] Não — nenhum mecanismo de tratamento de erros global foi **detectado**, resultando em erros padrão do servidor  

---