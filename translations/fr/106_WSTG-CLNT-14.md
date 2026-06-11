## WSTG-CLNT-14 — Testing for Reverse Tabnabbing

Le Reverse Tabnabbing est une vulnérabilité côté client où une page ouverte via `target="_blank"` conserve une référence fonctionnelle vers la page parente à travers l'objet `window.opener`. Cela permet à un site de destination contrôlé par un attaquant de rediriger par programmation l'onglet original de confiance vers une URL malveillante, généralement une page de Phishing conçue pour dérober des identifiants. L'attaque exploite la confiance inhérente de l'utilisateur dans l'onglet original, car il est peu probable qu'il remarque que la page qu'il vient de quitter a navigué vers un domaine différent. Les pratiques de sécurité modernes exigent l'utilisation des attributs `rel="noopener"` ou `rel="noreferrer"` sur les balises d'ancrage, ou la définition de la propriété `opener` à null en JavaScript, afin d'empêcher cette communication entre fenêtres.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Basse / Moyenne* |

> *La sévérité est Basse dans la plupart des navigateurs modernes car ils définissent désormais par défaut `target="_blank"` sur `rel="noopener"`. La sévérité devient Moyenne si l'application cible des navigateurs obsolètes ou utilise `window.open()` sans définir la propriété `opener` à null.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Des liens s'ouvrant dans une nouvelle fenêtre ou un nouvel onglet sont-ils présents ?
- [ ] Non — aucun lien n'utilise `target="_blank"` ou une redirection équivalente basée sur JavaScript  
- [ ] Oui — `target="_blank"` est utilisé exclusivement sur des liens internes de confiance  
- [ ] Oui — `target="_blank"` est utilisé sur des liens externes ou des URL fournies par l'utilisateur  

### La relation `window.opener` est-elle restreinte sur les balises d'ancrage ?
- [ ] Oui — `rel="noopener"` ou `rel="noreferrer"` est **appliqué de manière cohérente** à tous les liens avec `target="_blank"` *(Le plus sécurisé)*  
- [ ] Oui — les attributs sont appliqués mais sont **manquants** sur des liens à haut risque ou générés par l'utilisateur  
- [ ] Non — les attributs ne sont **pas appliqués**, permettant à la page enfant d'accéder à `window.opener`  

### L'application est-elle vulnérable au Reverse Tabnabbing via `window.open()` en JavaScript ?
- [ ] Non — `window.open()` n'est pas utilisé ou définit explicitement la propriété `opener` à null  
- [ ] Oui — `window.open()` est utilisé et la référence `opener` **reste accessible** pour la fenêtre enfant  

### Un attaquant peut-il contrôler la destination d'un lien qui s'ouvre dans un nouvel onglet ?
- [ ] Non — toutes les destinations de liens sont codées en dur et pointent vers des domaines de confiance  
- [ ] Oui — les destinations des liens sont fournies par l'utilisateur (ex: profils de réseaux sociaux, liens de biographie) et ne sont **pas** assainies pour les protections contre le tabnabbing  

---