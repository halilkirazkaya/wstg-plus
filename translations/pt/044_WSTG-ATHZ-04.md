## WSTG-ATHZ-04 — Teste de Referências Diretas Inseguras a Objetos (IDOR)

As Referências Diretas Inseguras a Objetos (IDOR) ocorrem quando uma aplicação fornece acesso direto a objetos com base em entradas fornecidas pelo usuário sem realizar uma verificação de autorização para validar as permissões do solicitante. Esta vulnerabilidade impacta significativamente a confidencialidade e a integridade, pois permite que atacantes visualizem, modifiquem ou excluam dados pertencentes a outros usuários ou ao sistema. Geralmente, manifesta-se em parâmetros de URL, dados no corpo de requisições POST ou chaves JSON que se referem a chaves internas de bancos de dados, nomes de arquivos ou identificadores de conta. Do ponto de vista de um atacante, a exploração envolve a manipulação desses identificadores — frequentemente por meio do incremento de números inteiros ou fuzzing de UUIDs — para acessar recursos fora de seu escopo autorizado.


| Campo | Valor |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Status do Teste** | Não Realizado |
| **Severidade** | Alta |

**Referências:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Ferramentas:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### A aplicação utiliza identificadores diretos de objetos nos parâmetros da requisição?
- [ ] Não — a aplicação utiliza referências indiretas ou mapas criptografados *(Mais seguro)*  
- [ ] Sim — a aplicação utiliza identificadores não previsíveis (ex: UUIDs longos/hashes)  
- [ ] Sim — a aplicação utiliza identificadores previsíveis (ex: inteiros incrementais ou nomes de usuário)  

### A autorização no lado do servidor (server-side) é realizada para cada requisição que envolve um identificador de objeto?
- [ ] Sim — as verificações no lado do servidor **não podem** ser burladas para nenhum identificador  
- [ ] Sim — as verificações no lado do servidor estão presentes, mas o bypass **é possível** via parameter pollution ou manipulação de cabeçalhos  
- [ ] Não — nenhuma verificação de autorização **é aplicada** para verificar a propriedade do objeto solicitado  

### Um atacante pode acessar ou modificar objetos pertencentes a outros usuários (IDOR Horizontal)?
- [ ] Não — o acesso a dados de outros usuários **não é possível**  
- [ ] Sim — a visualização de dados de outros usuários **é possível**, mas a modificação **não é possível**  
- [ ] Sim — tanto a visualização quanto a modificação de dados de outros usuários **são possíveis** *(Crítico)*  

### É possível acessar objetos administrativos ou de nível de sistema (IDOR Vertical)?
- [ ] Não — o acesso a objetos de privilégios mais elevados **não é possível**  
- [ ] Sim — o acesso a configurações administrativas ou arquivos do sistema **é possível**  

### A aplicação vaza identificadores de objetos por meio de busca, metadados ou outros endpoints?
- [ ] Não — os identificadores **não são expostos** involuntariamente  
- [ ] Sim — identificadores de outros usuários/objetos **podem** ser enumerados através de endpoints secundários ou perfis públicos  

---