## WSTG-CONF-02 — Test de la configuration de la plateforme d'application

Le test de configuration de la plateforme d'application consiste à auditer le serveur web sous-jacent, le serveur d'application et les paramètres du framework afin de s'assurer qu'ils sont durcis (hardened) contre les exploits courants. Les attaquants recherchent activement des identifiants par défaut, des vulnérabilités de plateforme non corrigées et des interfaces d'administration exposées pour obtenir un accès non autorisé ou exécuter du code. Les mauvaises configurations entraînent souvent la divulgation de variables d'environnement sensibles, de chemins système internes ou la présence d'applications d'exemple inutiles qui étendent la surface d'attaque. D'un point de vue professionnel, une plateforme mal configurée constitue un point d'entrée à haute probabilité pour obtenir un accès initial à l'environnement cible.

| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si les interfaces d'administration sont accessibles avec des identifiants par défaut ou si des scripts d'exemple permettent une exécution de code à distance (Remote Code Execution - RCE).

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Des identifiants ou comptes par défaut sont-ils actifs sur la plateforme ?
- [ ] Non — tous les comptes par défaut sont **désactivés** ou les mots de passe ont été modifiés  
- [ ] Oui — des comptes par défaut existent mais ne sont **pas accessibles** depuis le réseau externe  
- [ ] Oui — des comptes par défaut sont **actifs** et accessibles via Internet *(Critique)*  

### La plateforme divulgue-t-elle des informations de version sensibles via des en-têtes ou des pages d'erreur ?
- [ ] Non — les chaînes de version sont **masquées** ou génériques  
- [ ] Oui — des informations de version détaillées **sont divulguées** dans les en-têtes HTTP (ex : `Server`, `X-Powered-By`)  
- [ ] Oui — des traces de pile (stack traces) complètes et des chemins système internes **sont exposés** dans des messages d'erreur détaillés (verbose)  

### Les interfaces d'administration ou de gestion sont-elles correctement restreintes ?
- [ ] Non — les interfaces d'administration ne sont **pas exposées**  
- [ ] Oui — des interfaces existent mais sont **restreintes** par liste blanche d'IP (IP allowlisting) ou par authentification multi-facteurs (Multi-Factor Authentication - MFA)  
- [ ] Oui — des interfaces (ex : `/admin`, `/manager`, `/console`) sont **accessibles publiquement** avec une authentification à facteur unique  

### Des fichiers d'exemple, des scripts ou de la documentation sont-ils présents sur le serveur de production ?
- [ ] Non — tous les fichiers non essentiels ont été **supprimés**  
- [ ] Oui — des fichiers d'exemple ou de la documentation **sont présents** mais ne divulguent pas d'informations sensibles  
- [ ] Oui — des scripts d'exemple (ex : `info.php`, `examples/`) **sont présents** et constituent un vecteur d'attaque direct  

### Le serveur est-il configuré pour supporter des méthodes HTTP non sécurisées ?
- [ ] Non — seuls `GET` et `POST` sont **activés**  
- [ ] Oui — des méthodes potentiellement risquées comme `PUT`, `DELETE` ou `TRACE` sont **activées** mais restreintes  
- [ ] Oui — des méthodes risquées sont **activées** et permettent la modification non autorisée de fichiers ou le vol d'identifiants  

---