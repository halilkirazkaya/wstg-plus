## WSTG-INPV-17 — Testing for Host Header Injection

L'injection d'en-tête Host (Host Header Injection) se produit lorsqu'une application fait confiance à l'en-tête `Host` fourni par l'utilisateur et l'incorpore dans la logique côté serveur, telle que la génération de liens ou les redirections, sans validation suffisante. Les attaquants manipulent cet en-tête pour faciliter l'empoisonnement de cache web (Web Cache Poisoning), l'empoisonnement de réinitialisation de mot de passe (Password Reset Poisoning) en redirigeant des jetons sensibles vers un domaine externe, ou pour contourner les contrôles de sécurité reposant sur le routage basé sur les en-têtes. Une exploitation réussie permet à un attaquant d'exfiltrer des données de session, d'effectuer des prises de contrôle de compte (Account Takeover) ou de rediriger des utilisateurs peu méfiants vers une infrastructure malveillante. Cette vulnérabilité est plus couramment rencontrée dans les environnements avec équilibrage de charge (load-balanced) ou dans les applications qui génèrent dynamiquement des URLs absolues basées sur le contexte de la requête entrante.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si l'en-tête Host est utilisé pour générer des liens sensibles dans des e-mails, tels que les réinitialisations de mot de passe.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Outils :** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### L'application reflète-t-elle la valeur de l'en-tête `Host` dans les liens, redirections ou scripts ?
- [ ] Non — l'application utilise des URLs de base codées en dur (hardcoded) ou une configuration strictement validée  
- [ ] Oui — l'en-tête `Host` est reflété mais strictement validé par rapport à une liste blanche (whitelist)  
- [ ] Oui — l'en-tête `Host` est reflété et un contournement **est possible** via des valeurs malformées (ex: `expected.com:attacker.com`)  
- [ ] Oui — l'en-tête `Host` est reflété **sans** aucune validation  

### Des en-têtes alternatifs liés à l'hôte sont-ils traités par l'application ou l'infrastructure ?
- [ ] Non — les en-têtes tels que `X-Forwarded-Host`, `X-Host` ou `Forwarded` sont ignorés  
- [ ] Oui — les en-têtes alternatifs sont traités mais ne **surchargent pas** la logique interne  
- [ ] Oui — les en-têtes alternatifs **peuvent** être utilisés pour surcharger la valeur de l'en-tête `Host` principal  

### L'en-tête `Host` peut-il être manipulé pour empoisonner les e-mails générés par le système (ex: réinitialisations de mot de passe) ?
- [ ] Non — les liens dans les e-mails utilisent un domaine statique et de confiance provenant de la configuration du serveur  
- [ ] Oui — l'en-tête `Host` influence les liens des e-mails mais la validation **ne peut pas** être contournée  
- [ ] Oui — l'en-tête `Host` **peut** être manipulé pour rediriger les jetons de réinitialisation de mot de passe vers un domaine contrôlé par l'attaquant *(Critique)*  

### L'application est-elle susceptible au Web Cache Poisoning via l'en-tête `Host` ?
- [ ] Non — aucun mécanisme de mise en cache n'est présent ou l'en-tête `Host` fait partie de la clé de cache (cache key)  
- [ ] Oui — un cache est présent mais il valide strictement ou ignore l'en-tête `Host` pour les entrées hors clé (unkeyed inputs)  
- [ ] Oui — un cache existe et **peut** être empoisonné par un en-tête `Host` injecté pour servir du contenu malveillant à d'autres utilisateurs  

### Le serveur accepte-t-il des en-têtes `Host` arbitraires pour le routage d'hôtes virtuels (virtual host routing) ?
- [ ] Non — le serveur renvoie une erreur ou une page par défaut pour les en-têtes `Host` non reconnus  
- [ ] Oui — le serveur accepte n'importe quel en-tête `Host`, ce qui **est** utile pour la reconnaissance ou l'énumération de services internes  

---