## WSTG-BUSL-05 — Test des limites du nombre de fois qu'une fonction peut être utilisée

Ce test évalue si une application applique des contraintes de logique métier sur la fréquence et le nombre total de fois qu'une action ou fonction spécifique peut être exécutée par un utilisateur, une session ou une adresse IP unique. Le défaut de mise en œuvre de ces limites permet à des attaquants d'abuser de fonctions à haute valeur ajoutée — telles que l'envoi de SMS OTP, le déclenchement d'e-mails de réinitialisation de mot de passe, l'application de codes de réduction ou l'exécution d'exports de données volumineux — entraînant des pertes financières, l'épuisement des ressources ou des dommages à la réputation. Ces vulnérabilités se trouvent généralement dans les points de terminaison transactionnels ou les fonctionnalités de communication et sont exploitées via des scripts automatisés qui répètent l'action bien au-delà des seuils métiers prévus. Du point de vue d'un attaquant, il s'agit d'une cible privilégiée pour le SMS pumping, le mail bombing ou le Brute Force de workflows manquant de Rate Limiting explicite ou de régulation basée sur un compteur.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si la fonction implique des transactions financières, des coûts de SMS ou impacte la disponibilité globale du système.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### L'application définit-elle et applique-t-elle des limites sur les fonctions métier sensibles ?
- [ ] Oui — les limites sont strictement appliquées et un contournement n'est **pas possible**  
- [ ] Oui — les limites sont appliquées mais sont trop élevées pour empêcher l'abus  
- [ ] Non — aucune limite n'est appliquée à l'exécution de la fonction  

### Les limites de débit (Rate Limiting) et les compteurs d'utilisation sont-ils appliqués côté serveur ?
- [ ] Oui — l'application est strictement effectuée côté serveur et **ne peut pas** être manipulée  
- [ ] Non — l'application repose sur une logique côté client (ex: JavaScript ou champs cachés)  
- [ ] Non — aucune application n'existe à quelque niveau que ce soit  

### Les limites d'utilisation peuvent-elles être contournées par manipulation d'en-tête ou de session ?
- [ ] Non — le contournement via `X-Forwarded-For`, `Origin`, ou rotation de session n'est **pas possible**  
- [ ] Oui — les limites **peuvent** être contournées en usurpant les en-têtes IP ou en supprimant les cookies  
- [ ] Oui — les limites **peuvent** être contournées en créant plusieurs comptes ou sessions  

### Le dépassement de la limite déclenche-t-il une réponse défensive appropriée ?
- [ ] Oui — l'application renvoie `429 Too Many Requests` et journalise l'événement *(Le plus sécurisé)*  
- [ ] Oui — l'application renvoie une erreur mais ne journalise **pas** l'abus potentiel  
- [ ] Non — l'application continue de traiter les requêtes mais ignore la sortie  
- [ ] Non — l'application ne fournit aucune réponse ou plante sous la charge  

### L'impact de l'abus de la fonction est-il significatif pour l'entreprise ?
- [ ] Non — l'abus de la fonction a un coût ou un impact sur les ressources négligeable  
- [ ] Oui — l'abus **peut** entraîner une consommation mineure de ressources ou une gêne pour l'utilisateur  
- [ ] Oui — l'abus **peut** entraîner des coûts financiers importants (ex: frais de SMS) ou un déni de service *(Critique)*  

---