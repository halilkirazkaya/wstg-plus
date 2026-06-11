## WSTG-BUSL-10 — Test des fonctionnalités de paiement

Le test des fonctionnalités de paiement consiste à identifier les failles de logique métier qui permettent à un attaquant de contourner les passerelles de paiement, de manipuler les totaux des commandes ou d'usurper des signaux de transaction réussie. Ces vulnérabilités résident généralement dans la communication entre l'application, le navigateur client et les processeurs de paiement tiers tels que Stripe, PayPal, ou les systèmes de comptabilité internes. Un attaquant peut tenter de modifier les paramètres de prix en transit, de soumettre des quantités négatives pour réduire le coût total, ou de rejouer les rappels (callbacks) de transaction réussie pour honorer des commandes sans paiement réel. Une exploitation réussie entraîne une perte financière directe, des écarts d'inventaire et un compromis potentiel de l'intégrité du e-commerce de l'application.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### Le montant de la transaction est-il validé côté serveur avant le traitement ?
- [ ] Oui — le montant est strictement validé par rapport à la base de données produit côté serveur *(Le plus sécurisé)*  
- [ ] Oui — le montant est validé mais un contournement de la logique **est possible** via la pollution de paramètres (parameter pollution) ou le changement de devise  
- [ ] Non — le montant de la transaction **peut** être modifié dans la requête côté client et est accepté par le backend  

### La quantité d'articles peut-elle être manipulée pour aboutir à un total nul ou négatif ?
- [ ] Non — les quantités négatives ou nulles sont rejetées par la logique de validation de l'application  
- [ ] Oui — les quantités négatives sont acceptées dans le panier mais le total est corrigé lors du paiement  
- [ ] Oui — des quantités négatives **peuvent** être utilisées pour réduire le montant total de la commande ou obtenir un crédit *(Critique)*  

### L'application gère-t-elle de manière sécurisée les rappels (callbacks) ou les webhooks des passerelles de paiement ?
- [ ] Oui — les rappels nécessitent une signature cryptographique valide qui **ne peut pas** être usurpée  
- [ ] Oui — les signatures sont vérifiées mais le secret est faible ou la vérification **peut** être contournée  
- [ ] Non — l'application s'appuie sur des rappels non authentifiés ou non vérifiés pour confirmer le statut du paiement  

### L'application est-elle vulnérable aux conditions de concurrence (race conditions) pendant la phase de paiement ou de validation de commande ?
- [ ] Non — l'intégrité transactionnelle et les mécanismes de verrouillage de base de données sont **activés**  
- [ ] Oui — des conditions de concurrence **sont possibles** mais n'impactent pas le prix final ou l'exécution de la commande  
- [ ] Oui — des requêtes concurrentes **peuvent** mener à plusieurs exécutions de commande pour un seul paiement ou à une "double dépense" de cartes-cadeaux  

### Les codes de réduction ou les points de fidélité sont-ils sujets à manipulation ou réutilisation ?
- [ ] Non — les codes sont validés pour un usage unique et les contraintes logiques sont **appliquées**  
- [ ] Oui — les codes peuvent être réutilisés ou appliqués à des articles non éligibles, mais l'impact est limité  
- [ ] Oui — les attaquants **peuvent** appliquer plusieurs codes conflictuels ou contourner les limites d'expiration et d'utilisation  

---