## WSTG-CONF-02 — Testar a Configuração da Plataforma da Aplicação

O teste de configuração da plataforma da aplicação envolve a auditoria do servidor web subjacente, do servidor de aplicação e das definições do framework para garantir que estejam endurecidos (hardened) contra exploits comuns. Atacantes buscam ativamente por credenciais padrão, vulnerabilidades de plataforma não corrigidas (unpatched) e interfaces administrativas expostas para obter acesso não autorizado ou executar código. Configurações incorretas (misconfigurations) frequentemente resultam na exposição de variáveis de ambiente sensíveis, caminhos internos do sistema ou na presença de aplicações de exemplo desnecessárias que expandem a superfície de ataque. Do ponto de vista profissional, uma plataforma mal configurada serve como um ponto de entrada de alta probabilidade para alcançar o acesso inicial ao ambiente alvo.

| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Média / Alta* |

> *A severidade torna-se Alta se interfaces administrativas estiverem acessíveis com credenciais padrão ou se scripts de exemplo permitirem Execução Remota de Código (Remote Code Execution).

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Credenciais ou contas padrão estão ativas na plataforma?
- [ ] Não — todas as contas padrão estão **desativadas** ou as senhas foram alteradas  
- [ ] Sim — existem contas padrão, mas **não estão acessíveis** a partir da rede externa  
- [ ] Sim — as contas padrão estão **ativas** e acessíveis via internet *(Crítico)*  

### A plataforma está expondo informações sensíveis de versão através de cabeçalhos ou páginas de erro?
- [ ] Não — as strings de versão estão **ocultas** ou são genéricas  
- [ ] Sim — informações detalhadas de versão **são expostas** em cabeçalhos HTTP (ex: `Server`, `X-Powered-By`)  
- [ ] Sim — stack traces completos e caminhos internos do sistema **são expostos** em mensagens de erro detalhadas (verbose)  

### As interfaces administrativas ou de gerenciamento estão devidamente restritas?
- [ ] Não — interfaces administrativas **não estão expostas**  
- [ ] Sim — as interfaces existem, mas estão **restritas** por IP allowlisting ou Autenticação de Múltiplos Fatores (Multi-Factor Authentication)  
- [ ] Sim — interfaces (ex: `/admin`, `/manager`, `/console`) estão **publicamente acessíveis** com autenticação de fator único  

### Existem arquivos de exemplo, scripts ou documentação presentes no servidor de produção?
- [ ] Não — todos os arquivos não essenciais foram **removidos**  
- [ ] Sim — arquivos de exemplo ou documentação **estão presentes**, mas não vazam informações sensíveis  
- [ ] Sim — scripts de exemplo (ex: `info.php`, `examples/`) **estão presentes** e fornecem um vetor de ataque direto  

### O servidor está configurado para suportar métodos HTTP inseguros?
- [ ] Não — apenas `GET` e `POST` estão **habilitados**  
- [ ] Sim — métodos potencialmente arriscados como `PUT`, `DELETE` ou `TRACE` estão **habilitados**, mas restritos  
- [ ] Sim — métodos arriscados estão **habilitados** e permitem a modificação não autorizada de arquivos ou roubo de credenciais  

---