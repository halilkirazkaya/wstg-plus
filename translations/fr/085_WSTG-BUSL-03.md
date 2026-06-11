## WSTG-BUSL-03 — Test des contrôles d'intégrité

Le test des contrôles d'intégrité se concentre sur la vérification que l'application garantit que les données restent inchangées et fiables lorsqu'elles circulent à travers divers processus métier ou entre le client et le serveur. Les attaquants ciblent cette logique en falsifiant des paramètres critiques—tels que les prix des produits, les quantités, les privilèges des utilisateurs ou les états de transaction—au sein des requêtes HTTP, des champs cachés ou des cookies. Si l'application manque de vérification côté serveur ou de contrôles d'intégrité robustes comme le Hashing ou les HMACs, un adversaire peut manipuler ces valeurs pour commettre une fraude, élever ses propres privilèges ou contourner les contraintes métier prévues. Ce test est essentiel pour toute application gérant des transactions financières, des transitions d'état sensibles ou des workflows en plusieurs étapes où la sortie d'une étape sert d'entrée de confiance pour la suivante.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si le défaut d'intégrité permet un vol financier, une élévation de privilèges non autorisée ou la modification d'enregistrements persistants.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Les paramètres métier sensibles sont-ils exposés à une falsification côté client ?
- [ ] Non — les valeurs sensibles sont gérées exclusivement côté serveur  
- [ ] Oui — des valeurs comme le prix, la quantité ou les indicateurs d'état **sont** exposées mais chiffrées  
- [ ] Oui — les valeurs **sont** exposées en texte clair dans des champs cachés, des paramètres URL ou des cookies  

### Un mécanisme d'intégrité (par ex., HMAC, Checksum) est-il appliqué aux paramètres contrôlés par le client ?
- [ ] Oui — un contrôle d'intégrité robuste **est appliqué** et vérifié à chaque requête  
- [ ] Oui — un contrôle d'intégrité **est appliqué** mais utilise un algorithme faible/prévisible ou un secret codé en dur  
- [ ] Non — aucun contrôle d'intégrité **n'est appliqué** aux paramètres sensibles  

### Le contrôle d'intégrité peut-il être contourné ou recalculé par un attaquant ?
- [ ] Non — la vérification de la signature est strictement appliquée et le contournement **n'est pas possible**  
- [ ] Oui — la vérification de la signature peut être contournée en omettant le paramètre de signature  
- [ ] Oui — la signature **peut** être recalculée car la clé secrète ou la logique est exposée/faible  

### L'application effectue-t-elle une revalidation backend des données par rapport à une source de confiance ?
- [ ] Oui — le serveur revalide toutes les valeurs par rapport à la base de données et le contournement **n'est pas possible**  
- [ ] Oui — le serveur effectue une validation partielle mais un contournement **est possible** via des cas particuliers (edge cases)  
- [ ] Non — le serveur fait confiance aux données fournies par le client sans vérification backend *(Critique)*  

### Est-il possible de falsifier l'état d'un processus en plusieurs étapes ?
- [ ] Non — l'état de la session est géré côté serveur et ne peut pas être manipulé  
- [ ] Oui — les informations d'état sont transmises via le client et une falsification **est possible** pour sauter des étapes ou répéter des actions  

---