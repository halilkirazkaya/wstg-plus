## WSTG-BUSL-02 — Tester la capacité à forger des requêtes

La falsification de requêtes dans un contexte de logique métier implique la manipulation de paramètres définis par l'application pour effectuer des actions en dehors du flux de travail ou du niveau d'autorisation prévu. Les attaquants ciblent les processus en plusieurs étapes, tels que les systèmes de paiement ou l'enregistrement de compte, où l'état est maintenu via des paramètres côté client que le serveur ne parvient pas à re-valider par rapport à une source de confiance côté backend. En modifiant des champs cachés, des valeurs JSON ou des paramètres REST tels que les prix, les quantités ou les indicateurs d'état internes, un attaquant peut contourner les exigences de paiement, réaliser une escalade de privilèges ou déclencher des changements d'état imprévus. Cette vulnérabilité se manifeste généralement dans les formulaires web avec état ou les API où le serveur accorde une confiance implicite à la représentation de l'état métier fournie par le client lors d'une transaction.


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Élevée / Critique* |

> *La sévérité devient Critique si la requête forgée permet une perte financière, des actions administratives non autorisées ou une prise de contrôle totale de compte (Account Takeover).

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### L'application valide-t-elle les paramètres transactionnels par rapport à une « source de vérité » côté serveur ?
- [ ] Oui — tous les paramètres sensibles (prix, rôle, ID) sont validés par rapport à la base de données et le contournement est **impossible**  
- [ ] Oui — la validation est appliquée mais peut être contournée via une pollution de paramètres (Parameter Pollution) ou un encodage alternatif  
- [ ] Non — l'application fait confiance aux valeurs côté client pour déterminer le coût ou l'issue d'une transaction *(Critique)*  

### Les valeurs critiques pour le métier (ex : quantités, montants) peuvent-elles être modifiées en valeurs non autorisées ?
- [ ] Non — les valeurs négatives, les valeurs nulles ou les valeurs excessivement élevées sont rejetées par le serveur  
- [ ] Oui — le serveur accepte les valeurs modifiées mais seulement dans une plage limitée  
- [ ] Oui — des valeurs négatives ou nulles **peuvent** être soumises, entraînant des contournements financiers ou de logique  

### Des champs cachés ou des paramètres d'API sont-ils utilisés pour maintenir l'état au cours de processus en plusieurs étapes ?
- [ ] Non — l'état est maintenu de manière sécurisée via des sessions côté serveur ou des jetons (tokens) signés  
- [ ] Oui — l'état est transmis via le client mais les contrôles d'intégrité (HMAC/Signatures) sont **activés** et robustes  
- [ ] Oui — l'état est transmis via le client et les contrôles d'intégrité sont **désactivés** ou peuvent être supprimés  

### Est-il possible de forger des requêtes qui ignorent des étapes intermédiaires obligatoires dans un flux de travail (workflow) ?
- [ ] Non — le serveur impose une séquence stricte d'opérations pour chaque transaction  
- [ ] Oui — des étapes **peuvent** être ignorées mais l'action finale nécessite toujours des données valides provenant des étapes précédentes  
- [ ] Oui — la requête finale de « soumission » ou de « finalisation » **peut** être forgée directement pour contourner l'autorisation ou les étapes de paiement  

---