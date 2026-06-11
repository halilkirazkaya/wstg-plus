## WSTG-BUSL-04 — Test de temporisation des processus

Les attaques par analyse temporelle des processus (Process Timing) consistent à mesurer le temps mis par une application pour traiter des requêtes spécifiques afin d'en déduire des informations sensibles sur les états internes ou l'existence de données. En analysant les écarts de temps de réponse de l'ordre de la milliseconde — souvent causés par une logique conditionnelle à sortie anticipée (early-exit), des recherches en base de données ou des comparaisons cryptographiques qui ne s'exécutent pas en temps constant — les attaquants peuvent effectuer une analyse par canal auxiliaire (side-channel analysis) pour énumérer des noms d'utilisateurs valides, vérifier des fragments de mots de passe ou confirmer l'existence d'enregistrements spécifiques. Ces vulnérabilités surviennent généralement au niveau des points de terminaison d'authentification, des formulaires de réinitialisation de mot de passe et des filtres d'API gourmands en ressources où la logique backend bifurque en fonction de la validité de l'entrée. Du point de vue de l'attaquant, ces différences de timing servent d'« oracle booléen » qui révèle des vérités sur les données backend, même lorsque l'application renvoie des messages d'erreur génériques identiques à l'utilisateur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Outils :** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### L'application présente-t-elle des temps de réponse variables pour des requêtes de ressources valides ou invalides ?
- [ ] Non — les temps de réponse sont identiques quelle que soit la validité de l'entrée  
- [ ] Oui — des différences de timing négligeables existent mais ne sont **pas** statistiquement significatives  
- [ ] Oui — des différences de timing constantes sont **observables** et fournissent un oracle fiable  

### Des fonctions de comparaison en temps constant ou des délais artificiels sont-ils implémentés pour normaliser les temps de réponse ?
- [ ] Oui — des algorithmes en temps constant sont **appliqués** à toutes les opérations de comparaison sensibles *(Le plus sécurisé)*  
- [ ] Oui — des délais artificiels ou du jitter sont **activés**, mais la logique sous-jacente ne s'exécute pas en temps constant  
- [ ] Non — aucune mesure d'atténuation du timing n'est **appliquée**, permettant une mesure directe des branchements logiques  

### Un attaquant peut-il utiliser l'analyse statistique pour isoler le signal temporel du bruit réseau ?
- [ ] Non — le jitter réseau et le bruit applicatif rendent l'analyse temporelle **impossible**  
- [ ] Oui — avec un échantillonnage suffisant, le signal temporel **peut** être isolé du bruit  
- [ ] Oui — le delta de timing est suffisamment important pour qu'une normalisation statistique ne soit **pas** requise  

### L'énumération de comptes est-elle possible via la fonctionnalité de connexion ou de réinitialisation de mot de passe ?
- [ ] Non — l'existence d'un compte ne peut pas être déterminée par le timing  
- [ ] Oui — les écarts de timing permettent à un attaquant distant d'**énumérer** des noms d'utilisateurs ou des adresses e-mail valides  

### Des opérations gourmandes en ressources (ex : traitement de fichiers, recherches complexes) divulguent-elles l'existence de données via le timing ?
- [ ] Non — la consommation de ressources est uniforme, qu'un enregistrement soit trouvé ou non  
- [ ] Oui — l'existence d'enregistrements sensibles **peut** être déduite en mesurant la durée du traitement backend  

---