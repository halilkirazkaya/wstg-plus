## WSTG-INPV-16 — HTTP Request Smuggling

L'HTTP Request Smuggling (HRS) se produit lorsqu'une chaîne de serveurs HTTP (tels qu'un équilibreur de charge et un serveur web back-end) interprète les limites d'une requête de manière différente, généralement en raison d'en-têtes `Content-Length` et `Transfer-Encoding` conflictuels. Un attaquant exploite cette désynchronisation pour « smuggler » (introduire clandestinement) une entrée dans le tampon de requête du serveur back-end, préfixant ainsi la requête de l'utilisateur légitime suivant avec un segment contrôlé par l'attaquant. Cette technique permet des attaques graves, notamment le session hijacking, le contournement des contrôles de sécurité et le cache poisoning. L'exploitation est généralement réalisée par une analyse basée sur le temps (timing-based analysis) ou en observant des réponses inattendues du back-end lorsque des requêtes ultérieures sont envoyées au serveur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Critique / Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Outils :** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Les serveurs front-end et back-end gèrent-ils l'en-tête `Transfer-Encoding: chunked` de manière cohérente ?
- [ ] Oui — les deux serveurs gèrent le codage par blocs (chunked encoding) de manière identique et la désynchronisation est **impossible**  
- [ ] Non — les serveurs interprètent les en-têtes différemment mais un assainissement ou une normalisation **est appliqué(e)**  
- [ ] Non — la désynchronisation CL.TE (Content-Length/Transfer-Encoding) **est possible** *(Critique)*  
- [ ] Non — la désynchronisation TE.CL (Transfer-Encoding/Content-Length) **est possible** *(Critique)*  

### L'attaquant peut-il empoisonner le cache côté serveur ou côté client à l'aide d'une requête « smuggled » ?
- [ ] Non — la mise en cache est **désactivée** ou n'est pas susceptible d'être empoisonnée  
- [ ] Oui — les requêtes « smuggled » **peuvent** rediriger les utilisateurs vers des domaines malveillants ou injecter des scripts via le cache poisoning  

### Est-il possible de capturer les requêtes ou les jetons de session (session tokens) d'autres utilisateurs via la concaténation de requêtes ?
- [ ] Non — le smuggling ne **permet pas** l'exposition de données entre utilisateurs  
- [ ] Oui — le smuggling permet d'ajouter la requête de l'utilisateur suivant à un corps `POST` contrôlé par l'attaquant, rendant l'exfiltration **possible**  

### L'infrastructure utilise-t-elle HTTP/2, et est-elle sensible à la désynchronisation H2.CL ou H2.TE ?
- [ ] Non — l'application utilise uniquement HTTP/1.1 ou HTTP/2 est implémenté de manière sécurisée sans downgrade  
- [ ] Oui — des downgrades en texte clair de HTTP/2 vers HTTP/1.1 se produisent et la désynchronisation **est possible**  

### Les en-têtes malformés (par exemple, `Transfer-Encoding:  chunked` avec un espace) sont-ils gérés de manière sécurisée ?
- [ ] Oui — les en-têtes malformés sont rejetés ou normalisés sur tous les niveaux  
- [ ] Non — la désynchronisation TE.TE **est possible** en raison d'une gestion incohérente des en-têtes malformés ou masqués  

---