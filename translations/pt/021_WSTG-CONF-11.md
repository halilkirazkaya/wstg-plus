## WSTG-CONF-11 — Test Cloud Storage

O teste de configurações incorretas de armazenamento em nuvem envolve a identificação e auditoria de serviços de armazenamento de objetos acessíveis publicamente, como Amazon S3, Azure Blobs ou Google Cloud Storage. Esses recursos frequentemente contêm dados sensíveis, incluindo backups, código-fonte, PII ou arquivos de configuração que estão expostos devido a Access Control Lists (ACLs) ou políticas de Identity and Access Management (IAM) excessivamente permissivas. Atacantes enumeram nomes de buckets por meio do reconhecimento de arquivos JavaScript, código-fonte HTML e técnicas de Brute Force para encontrar pontos de entrada para exfiltração de dados ou modificação não autorizada de arquivos. A exploração pode levar a um impacto significativo, incluindo violações de dados completas ou a capacidade de servir arquivos maliciosos a partir de um domínio confiável.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta / Crítica* |

> *A severidade torna-se Crítica se PII, credenciais ou backups de produção estiverem acessíveis, ou se o acesso de gravação (write access) não autenticado estiver habilitado.

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Ferramentas:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Os endpoints de armazenamento em nuvem (buckets/containers) foram identificados e enumerados?
- [ ] Não — nenhum endpoint de armazenamento em nuvem é utilizado ou detectável  
- [ ] Sim — os endpoints de armazenamento foram identificados via análise de código ou monitoramento de tráfego  
- [ ] Sim — os endpoints de armazenamento foram descobertos via Brute Force de convenção de nomes  

### Usuários não autenticados conseguem listar o conteúdo do armazenamento em nuvem?
- [ ] Não — a listagem do bucket está **desativada** e retorna 403 Forbidden ou 404 Not Found  
- [ ] Sim — a listagem está **ativada**, mas nenhum arquivo sensível está presente  
- [ ] Sim — a listagem está **ativada** e nomes de arquivos sensíveis/metadados estão visíveis  

### O download de arquivos sensíveis dos buckets identificados é possível?
- [ ] Não — os arquivos estão protegidos por políticas de IAM ou autenticação válida é exigida  
- [ ] Sim — os arquivos estão acessíveis, mas exigem uma URL assinada (signed URL) que **não pode** ser adivinhada facilmente  
- [ ] Sim — arquivos sensíveis (backups, `.env`, PII) estão **publicamente acessíveis** para download *(Crítica)*  

### O upload ou modificação de arquivos de forma não autenticada é permitido?
- [ ] Não — uploads ou modificações não autenticados **não são possíveis**  
- [ ] Sim — o upload autenticado é exigido, mas o bypass **é possível** via condições de política fracas  
- [ ] Sim — a escrita, sobrescrita ou exclusão de arquivos de forma não autenticada é **possível** *(Crítica)*  

### As políticas de Cross-Origin Resource Sharing (CORS) no endpoint de armazenamento são restritivas?
- [ ] Sim — o CORS está **desativado** ou restrito a origens confiáveis específicas  
- [ ] Não — o CORS está **ativado** com uma origem wildcard `*`, permitindo acesso não autorizado baseado em web  

---