## WSTG-ATHZ-02 — Test de contournement du schéma d'autorisation

Le contournement des schémas d'autorisation se produit lorsqu'une application ne parvient pas à appliquer les contrôles d'accès, permettant à un attaquant d'accéder à des ressources ou des fonctions en dehors de ses permissions prévues. Cette vulnérabilité se manifeste généralement sous la forme d'une Escalade de Privilèges Horizontale (Horizontal Privilege Escalation), où un attaquant accède aux données appartenant à un autre utilisateur de même rang, ou d'une Escalade de Privilèges Verticale (Vertical Privilege Escalation), où un utilisateur peu privilégié obtient des capacités administratives. Les attaquants exploitent ces failles en manipulant des paramètres de requête tels que les IDs de ressources, en modifiant les jetons de session (session tokens) ou en exploitant des en-têtes HTTP comme `X-Original-URL` pour contourner les restrictions basées sur le chemin. Assurer une autorisation robuste est essentiel pour maintenir la confidentialité des données et empêcher des actions administratives non autorisées qui pourraient compromettre l'ensemble du système.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Outils :** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### L'escalade de privilèges horizontale est-elle empêchée entre les utilisateurs de même rôle ?
- [ ] Oui — les contrôles d'autorisation **sont appliqués** à chaque demande de ressource et le contournement n'est **pas possible**  
- [ ] Oui — les contrôles d'autorisation **sont appliqués** mais peuvent être contournés via IDOR ou manipulation de paramètres  
- [ ] Non — les utilisateurs **peuvent** accéder aux données appartenant à d'autres utilisateurs en changeant simplement un ID de ressource *(Élevée)*  

### L'escalade de privilèges verticale est-elle empêchée pour les utilisateurs peu privilégiés ?
- [ ] Non — l'application n'a qu'un seul niveau de privilège  
- [ ] Oui — les fonctions administratives **ne peuvent pas** être accédées par des utilisateurs peu privilégiés  
- [ ] Oui — les fonctions administratives sont masquées dans l'interface utilisateur mais **peuvent** être accédées via une URL directe  
- [ ] Non — les utilisateurs peu privilégiés **peuvent** effectuer des actions administratives en manipulant les rôles ou les permissions *(Critique)*  

### L'autorisation peut-elle être contournée en utilisant la manipulation de verbes HTTP (HTTP verb tampering) ?
- [ ] Non — les contrôles d'accès sont appliqués indépendamment de la méthode HTTP utilisée  
- [ ] Oui — changer la méthode (par exemple, de `GET` à `POST` ou `PUT`) **contourne** les contrôles d'autorisation  

### Les points de terminaison (endpoints) administratifs ou restreints sont-ils accessibles sans aucune authentification ?
- [ ] Non — tous les endpoints sensibles nécessitent une session valide et les permissions appropriées  
- [ ] Oui — certains endpoints administratifs sont accessibles si l'attaquant connaît l'URL directe  
- [ ] Oui — l'accès non authentifié à des fonctions sensibles **est possible** *(Critique)*  

### Les en-têtes HTTP permettent-ils de contourner l'autorisation basée sur le chemin ?
- [ ] Non — l'application n'est **pas** sensible aux contournements basés sur les en-têtes  
- [ ] Oui — des en-têtes tels que `X-Original-URL`, `X-Rewrite-URL` ou `X-Forwarded-For` **peuvent** être utilisés pour contourner les contrôles d'accès  

---