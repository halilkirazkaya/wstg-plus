## WSTG-CLNT-05 — Test d'Injection CSS

L'Injection CSS (CSS Injection) se produit lorsqu'une application permet à une entrée fournie par l'utilisateur d'influencer les feuilles de style en cascade (CSS) d'une page web sans assainissement ou échappement approprié. Bien que souvent perçue comme un problème cosmétique, les attaquants peuvent exploiter l'injection CSS pour exfiltrer des données sensibles, telles que des jetons CSRF ou des identifiants de session, en utilisant des sélecteurs d'attributs et des propriétés de type background-image pour déclencher des requêtes externes. Cette vulnérabilité survient généralement dans les points de terminaison où les préférences de l'utilisateur (par exemple, thèmes personnalisés, couleurs de police) sont reflétées à l'intérieur de blocs `<style>` ou d'attributs `style` en ligne. Du point de vue d'un attaquant, une exploitation réussie peut mener à un détournement d'interface utilisateur (UI Redressing), à du phishing par modification non autorisée de la mise en page, ou à une exfiltration furtive de données dans des environnements où des politiques de sécurité de contenu (CSP) strictes pourraient autrement bloquer les attaques basées sur JavaScript.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si le point d'injection permet l'exfiltration d'attributs sensibles comme des jetons CSRF ou s'il peut être utilisé pour du UI Redressing dans des flux de travail sensibles.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### L'application reflète-t-elle les entrées contrôlées par l'utilisateur dans les contextes CSS ?
- [ ] Non — l'entrée n'est **pas** reflétée dans les balises de style, les attributs ou les fichiers CSS  
- [ ] Oui — l'entrée est reflétée mais strictement limitée à des valeurs alphanumériques sûres  
- [ ] Oui — l'entrée est reflétée à l'intérieur d'un attribut `style` ou d'un bloc `<style>`  

### Les métacaractères et fonctions spécifiques au CSS sont-ils correctement assainis ?
- [ ] Oui — les caractères tels que `{`, `}`, `:` et les fonctions comme `url()` sont **correctement échappés**  
- [ ] Oui — l'assainissement est en place mais un contournement **est possible** via l'encodage ou les commentaires CSS  
- [ ] Non — aucun assainissement n'est appliqué aux métacaractères CSS  

### L'exfiltration de données est-elle possible en utilisant des sélecteurs CSS ?
- [ ] Non — les sélecteurs **ne peuvent pas** déclencher de requêtes externes ou fuiter des données  
- [ ] Oui — une exfiltration partielle de données **est possible** via les sélecteurs d'attributs et `background-image`  
- [ ] Oui — une exfiltration complète de jetons sensibles **est possible** en utilisant des techniques automatisées de Brute Force  

### Une politique de sécurité de contenu (CSP) atténue-t-elle le risque d'injection CSS ?
- [ ] Oui — `style-src` est restreint à 'self' et `img-src` ou `connect-src` **est restreint**  
- [ ] Oui — une CSP est présente mais utilise 'unsafe-inline' ou autorise des domaines externes génériques  
- [ ] Non — aucune CSP n'est implémentée pour empêcher le chargement de ressources externes via CSS  

### La vulnérabilité peut-elle être utilisée pour effectuer du UI Redressing ou du Phishing ?
- [ ] Non — la portée de l'injection est trop limitée pour modifier la mise en page de manière significative  
- [ ] Oui — la modification de l'interface utilisateur (UI) **est possible**, permettant la superposition d'éléments malveillants  
- [ ] Oui — un contrôle total de la mise en page est **possible**, facilitant des attaques de phishing hautement convaincantes  

---