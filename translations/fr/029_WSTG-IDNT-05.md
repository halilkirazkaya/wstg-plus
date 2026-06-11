## WSTG-IDNT-05 — Test de politique de noms d'utilisateur faible ou non appliquée

Le test des politiques de noms d'utilisateur faibles ou non appliquées se concentre sur les règles régissant la création d'identifiants de compte et leur vulnérabilité à l'énumération ou au spoofing. Des politiques faibles permettent souvent des séquences prévisibles, des mots du dictionnaire courants ou des identifiants qui reflètent les identifiants internes des employés, ce qui réduit considérablement l'espace de recherche pour les attaques de type Brute Force et Credential Stuffing. Du point de vue d'un attaquant, l'identification de ces modèles est la première étape de la découverte de comptes à grande échelle, souvent réalisée en analysant les messages d'erreur lors de l'enregistrement, le comportement de réinitialisation de mot de passe ou les métadonnées dans les profils publics. Garantir une politique robuste empêche les attaquants de cartographier systématiquement la base d'utilisateurs et facilite la défense contre le Phishing ciblé et les tentatives automatisées d'Authentication Bypass.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **État du test** | Non effectué |
| **Sévérité** | Basse / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Les noms d'utilisateur sont-ils basés sur des modèles hautement prévisibles ou séquentiels ?
- [ ] Non — les noms d'utilisateur sont aléatoires, définis par l'utilisateur ou sont des chaînes à haute entropie  
- [ ] Oui — les noms d'utilisateur suivent un format prévisible (ex : `user1001`, `user1002`) mais l'authentification est robuste  
- [ ] Oui — les noms d'utilisateur sont **strictement séquentiels** ou suivent un format d'entreprise connu, facilitant une énumération aisée  

### L'application impose-t-elle des exigences de longueur minimale et de complexité pour les noms d'utilisateur ?
- [ ] Oui — des politiques strictes de longueur et de jeu de caractères **sont appliquées** lors de l'enregistrement  
- [ ] Oui — des politiques existent mais autorisent des noms d'utilisateur extrêmement courts (ex : 1-2 caractères) ou trop simples  
- [ ] Non — **aucune politique** n'est appliquée concernant la longueur ou les types de caractères des noms d'utilisateur  

### Des noms d'utilisateur valides peuvent-ils être énumérés via les réponses de l'application ?
- [ ] Non — l'application renvoie des messages d'erreur génériques et présente un délai de réponse (timing) cohérent pour toutes les tentatives  
- [ ] Oui — l'application renvoie des messages d'erreur distincts (ex : "Nom d'utilisateur déjà utilisé") mais un Rate Limiting **est appliqué**  
- [ ] Oui — l'application **divulgue** des noms d'utilisateur valides via des erreurs d'enregistrement, des réinitialisations de mot de passe ou des différences de timing  

### La politique empêche-t-elle l'enregistrement de noms d'utilisateur administratifs communs ou réservés ?
- [ ] Oui — l'application bloque les noms communs (ex : `admin`, `root`, `support`) et les termes réservés au système  
- [ ] Non — les utilisateurs **peuvent** enregistrer des comptes avec des noms sensibles ou administratifs  

### La politique de noms d'utilisateur est-elle cohérente sur tous les points d'entrée (API, Mobile, Web) ?
- [ ] Oui — les politiques **sont appliquées** de manière cohérente sur toutes les interfaces et versions  
- [ ] Non — les points de terminaison hérités (legacy) ou des versions spécifiques d'API **n'appliquent pas** les mêmes contraintes de nom d'utilisateur  

---