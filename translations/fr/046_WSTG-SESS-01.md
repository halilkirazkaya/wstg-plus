## WSTG-SESS-01 — Test du schéma de gestion de session

Le test du schéma de gestion de session se concentre sur l'analyse de la manière dont l'application gère le cycle de vie et la structure des identifiants de session afin de s'assurer qu'ils sont robustes contre la prédiction et le détournement (Session Hijacking). Les attaquants capturent plusieurs jetons (tokens) de session pour effectuer une analyse statistique, à la recherche de modèles, d'une faible entropie ou d'incréments prévisibles permettant de forger des sessions valides pour d'autres utilisateurs. Les faiblesses du schéma se manifestent souvent par des vulnérabilités de fixation de session (Session Fixation) ou par l'utilisation de valeurs insuffisamment aléatoires, généralement trouvées dans les cookies HTTP, les paramètres d'URL ou les implémentations d'en-têtes personnalisés. Compromettre le schéma de session est une attaque à fort impact qui accorde à un adversaire un accès complet à l'état authentifié d'une victime sans avoir besoin de ses identifiants principaux.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **État du test** | Non effectué |
| **Sévérité** | Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Outils :** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy pour l'analyse d'entropie)`, `Cookie Editor`

### L'identifiant de session est-il suffisamment complexe et aléatoire ?
- [ ] Oui — le jeton passe les tests statistiques de caractère aléatoire (haute entropie) et aucun contournement n'est possible  
- [ ] Oui — le jeton est long mais contient des modèles prévisibles ou des segments statiques  
- [ ] Non — le jeton est séquentiel, basé sur des horodatages (timestamps) prévisibles, ou présente une entropie **critiquement faible** *(Critique)*  

### Où l'identifiant de session est-il transmis et stocké ?
- [ ] L'identifiant est stocké dans un cookie `Secure` et `HttpOnly` *(Le plus sécurisé)*  
- [ ] L'identifiant est stocké dans un cookie mais ne possède pas les drapeaux `Secure` ou `HttpOnly`  
- [ ] L'identifiant est transmis via des paramètres d'URL ou des requêtes `GET` **uniquement** *(Risque élevé)*  

### L'application génère-t-elle un nouvel identifiant de session après une authentification réussie ?
- [ ] Oui — un nouvel ID de session unique **est généré** immédiatement après la connexion  
- [ ] Non — l'ID de session reste le même avant et après la connexion *(Session Fixation possible)*  

### L'identifiant de session est-il lié à une adresse IP spécifique ou à un User-Agent pour empêcher le réemploi ?
- [ ] Oui — la session est invalidée si l'empreinte (fingerprint) du client change de manière significative  
- [ ] Non — la session **peut** être réutilisée sur différentes adresses IP ou appareils sans vérification supplémentaire  

### Les identifiants de session sont-ils correctement invalidés côté serveur après une déconnexion ou un délai d'expiration ?
- [ ] Oui — l'état de la session côté serveur est **complètement détruit** lors de la déconnexion  
- [ ] Non — la session reste valide sur le serveur même si le cookie côté client est supprimé  
- [ ] Non — la session **n'expire jamais** ou possède un délai d'expiration (timeout) excessivement long  

---