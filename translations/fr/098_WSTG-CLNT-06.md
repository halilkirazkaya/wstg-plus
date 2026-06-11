## WSTG-CLNT-06 — Test de manipulation de ressources côté client (Client-Side Resource Manipulation)

La manipulation de ressources côté client survient lorsqu'une application permet à une entrée contrôlée par l'utilisateur d'influencer la source ou le chemin des ressources chargées par le navigateur, telles que des scripts, des feuilles de style ou des iframes. En manipulant des fragments d'URI, des paramètres de requête (query parameters) ou d'autres sources DOM, un attaquant peut contraindre l'application à charger du contenu externe malveillant ou à exécuter du code non autorisé. Cette vulnérabilité se trouve généralement dans la logique JavaScript qui alimente dynamiquement les attributs `src` ou `href` d'éléments HTML sans validation suffisante par rapport à une liste d'autorisation (allow-list) de confiance. Du point de vue d'un attaquant, cela est exploité en forgeant une URL qui redirige le chargement de ressources vers un serveur contrôlé par l'attaquant, ce qui peut entraîner un Cross-Site Scripting (XSS) basé sur le DOM, une exfiltration de données sensibles ou un détournement d'interface utilisateur (UI redressing).


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyenne / Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Outils :** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### L'application utilise-t-elle des récepteurs (sinks) côté client pour charger ou intégrer dynamiquement des ressources ?
- [ ] Non — les ressources sont chargées uniquement via du HTML statique ou une logique côté serveur  
- [ ] Oui — le JavaScript côté client alimente dynamiquement des attributs de ressources tels que `src`, `href` ou `data`  

### Des contrôles de validation ou des listes d'autorisation (allow-lists) sont-ils implémentés pour les sources d'URI des ressources ?
- [ ] Oui — des listes d'autorisation strictes et une validation d'origine **sont appliquées** à toutes les ressources dynamiques  
- [ ] Oui — une validation est présente mais repose sur des listes d'exclusion (blacklists) faibles ou des regex facilement contournables  
- [ ] Non — aucun contrôle de validation n'est appliqué aux chemins de ressources contrôlés par l'utilisateur  

### Est-il possible de contourner la validation d'URI en utilisant des protocoles alternatifs ou du codage ?
- [ ] Non — le contournement des filtres de ressources n'est **pas possible**  
- [ ] Oui — le contournement **est possible** en utilisant différents protocoles tels que `data:`, `vbscript:` ou `javascript:`  
- [ ] Oui — le contournement **est possible** en utilisant des caractères encodés ou des octets nuls (null bytes) pour contourner les correspondances de chaînes simples  

### Un attaquant peut-il rediriger le chargement de ressources vers un domaine externe non fiable ?
- [ ] Non — les chemins de ressources sont restreints à la même origine ou à un sous-répertoire fixe  
- [ ] Oui — l'application **peut** être contrainte de charger des scripts ou des styles à partir d'un domaine externe arbitraire  

### Quel est l'impact maximal de la manipulation de ressources ?
- [ ] Faible — la manipulation n'affecte que des éléments non exécutables comme des images ou du texte non sensible  
- [ ] Moyen — la manipulation permet l'injection de CSS ou un détournement d'interface (UI redressing) significatif  
- [ ] Élevé — la manipulation conduit à un XSS basé sur le DOM ou à l'exécution de JavaScript arbitraire *(Critique)*  

---