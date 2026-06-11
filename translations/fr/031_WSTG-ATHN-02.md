## WSTG-ATHN-02 — Test des identifiants par défaut

Le test des identifiants par défaut consiste à identifier les interfaces d'administration, les services ou les composants d'application qui utilisent encore des noms d'utilisateur et des mots de passe définis en usine ou fournis par le fournisseur. Cette vulnérabilité est une cible prioritaire pour les attaquants car elle offre souvent une voie immédiate vers la compromission complète du système, un accès administratif ou une exécution de code à distance (Remote Code Execution) avec un effort minimal. Elle se produit généralement dans les logiciels du commerce, les plateformes CMS, les outils de gestion de bases de données et le matériel intégré comme les appareils IoT ou les équipements réseau. L'exploitation est généralement réalisée en recoupant les versions de logiciels identifiées avec des bases de données publiques de mots de passe par défaut ou en utilisant des outils de Credential Stuffing automatisés lors des phases initiales de reconnaissance et d'exploitation.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Critique / Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Outils :** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Les interfaces d'administration ou de gestion sont-elles exposées sur Internet ou sur des segments de réseau non approuvés ?
- [ ] Non — aucune interface de gestion n'est accessible  
- [ ] Oui — des interfaces sont présentes mais limitées par des contrôles au niveau réseau (ex: VPN, IP Allowlist)  
- [ ] Oui — les interfaces **sont** accessibles publiquement et nécessitent une authentification  

### Les identifiants par défaut fournis par le fournisseur ont-ils été modifiés pour tous les composants identifiés ?
- [ ] Oui — tous les identifiants par défaut **ont été modifiés** sur l'ensemble des composants *(Le plus sécurisé)*  
- [ ] Oui — les identifiants **ont été modifiés** pour l'application principale, mais les composants secondaires (ex: CMS, DB) conservent les valeurs par défaut  
- [ ] Non — les identifiants par défaut **sont actifs** sur une ou plusieurs interfaces identifiables *(Critique)*  

### L'application impose-t-elle un changement de mot de passe lors de la première connexion pour les nouveaux comptes ou les comptes administratifs ?
- [ ] Oui — un changement de mot de passe **est obligatoire** avant toute action administrative  
- [ ] Non — l'application **permet** l'utilisation indéfinie des identifiants par défaut  

### Des mécanismes de verrouillage ou de limitation de débit (Rate Limiting) sont-ils actifs pour empêcher les tests automatisés d'identifiants par défaut ?
- [ ] Oui — un Rate Limiting agressif ou un verrouillage de compte (Lockout) **est appliqué**  
- [ ] Oui — des contrôles sont présents mais un contournement **est possible** via la manipulation d'en-têtes ou la rotation d'adresses IP  
- [ ] Non — aucun mécanisme de Rate Limiting ou de verrouillage n'est activé  

### Les plugins tiers, middlewares ou outils d'administration (ex: phpMyAdmin, Tomcat Manager) utilisent-ils des identifiants uniques ?
- [ ] Non — les composants tiers n'existent pas dans l'environnement  
- [ ] Oui — tous les composants tiers utilisent des identifiants uniques non définis par défaut  
- [ ] Non — au moins un composant tiers **est accessible** via des identifiants par défaut connus  

---