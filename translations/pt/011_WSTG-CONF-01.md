## WSTG-CONF-01 — Testar a Configuração da Infraestrutura de Rede

O teste de configuração da infraestrutura de rede envolve a identificação de vulnerabilidades nos componentes de servidor e rede subjacentes que suportam a aplicação web. Atacantes visam serviços mal configurados, protocolos obsoletos e portas abertas desnecessárias para obter um ponto de apoio, realizar movimentação lateral ou efetuar reconhecimento. Descobertas comuns incluem versões inseguras de TLS, banners de serviço detalhados e interfaces administrativas expostas que deveriam estar restritas a redes internas. Ao explorar essas falhas de nível de infraestrutura, um adversário pode comprometer a confidencialidade dos dados em trânsito ou obter acesso não autorizado ao sistema operacional hospedeiro.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Baixa / Média |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Ferramentas:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Existem portas abertas ou serviços desnecessários expostos no host da aplicação?
- [ ] Não — apenas as portas necessárias (ex: 443) estão **acessíveis**  
- [ ] Sim — serviços adicionais estão expostos, mas seguem uma configuração **segura**  
- [ ] Sim — serviços não essenciais (ex: FTP, Telnet, SMB) estão **expostos** publicamente  

### Os banners ou cabeçalhos de serviço vazam informações confidenciais de versão?
- [ ] Não — os banners foram removidos ou são genéricos  
- [ ] Sim — os banners revelam o software do servidor (ex: Apache/nginx), mas **não** os números de versão  
- [ ] Sim — banners detalhados revelam versões específicas de software, auxiliando a **exploração**  

### A configuração de TLS segue as melhores práticas de segurança modernas?
- [ ] Sim — apenas TLS 1.2/1.3 estão **habilitados** com cipher suites fortes  
- [ ] Sim — protocolos legados (TLS 1.0/1.1) estão **habilitados**  
- [ ] Não — cifras fracas ou protocolos inseguros (SSLv2/v3) estão **habilitados**  

### As interfaces administrativas ou de gerenciamento estão restritas a redes autorizadas?
- [ ] Sim — as interfaces (ex: SSH, RDP, painéis de controle) **não estão acessíveis** a partir da internet pública  
- [ ] No — as interfaces estão publicamente **acessíveis**, mas exigem autenticação de múltiplos fatores  
- [ ] No — as interfaces estão publicamente **acessíveis** com autenticação de fator único ou credenciais padrão  

---