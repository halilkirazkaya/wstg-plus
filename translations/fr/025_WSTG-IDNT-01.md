## WSTG-IDNT-01 — Test des définitions de rôles

Le test des définitions de rôles consiste à identifier et à documenter les différents rôles d'utilisateurs et niveaux de permission définis au sein de l'application afin de s'assurer qu'ils sont logiquement séparés et qu'ils respectent le principe du moindre privilège (least privilege). Un attaquant analyse l'application pour cartographier les rôles à hauts privilèges par rapport aux rôles d'utilisateurs standards, à la recherche de toute ambiguïté dans les limites de permissions ou de fonctions administratives non documentées. L'identification de ces rôles est la première étape de la découverte de chemins d'escalade de privilèges (privilege escalation), car les incohérences dans les définitions de rôles mènent souvent à un accès non autorisé à des données sensibles ou à des fonctionnalités administratives. Cette phase se déroule généralement lors de la reconnaissance initiale et de la cartographie de la logique métier, en se concentrant sur les profils utilisateurs, les tableaux de bord administratifs et les structures de permissions d'API.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Statut du test** | Non effectué |
| **Sévérité** | Basse / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Les rôles sont-ils clairement définis et documentés dans l'application ou sa documentation ?
- [ ] Oui — les rôles sont entièrement documentés et suivent le principe du **moindre privilège**  
- [ ] Oui — les rôles sont documentés mais contiennent des **permissions redondantes**  
- [ ] Non — aucune documentation ou définition formelle des rôles n'existe  

### Existe-t-il une séparation claire entre les fonctions administratives et les fonctions utilisateur standard ?
- [ ] Oui — les fonctions administratives sont **complètement isolées** des vues utilisateurs standards  
- [ ] Oui — la séparation est présente mais les points de terminaison (endpoints) administratifs sont **prévisibles ou devinables**  
- [ ] Non — les fonctions administratives et standards sont **mélangées** au sein des mêmes interfaces  

### L'application utilise-t-elle un mécanisme centralisé pour le contrôle d'accès basé sur les rôles (RBAC) ?
- [ ] Oui — un RBAC ou ABAC centralisé est **implémenté** et appliqué de manière cohérente  
- [ ] Oui — des contrôles décentralisés existent mais sont **appliqués de manière incohérente** à travers les modules  
- [ ] Non — la logique de contrôle d'accès est **codée en dur** dans les pages ou composants individuels  

### Existe-t-il des rôles non documentés ou cachés dans le système ?
- [ ] Non — tous les rôles découverts correspondent aux fonctionnalités documentées  
- [ ] Oui — des rôles cachés ou "shadow" (ex: `super_admin`, `support`, `test`) **sont présents**  

### Un utilisateur à bas privilèges peut-il énumérer les permissions ou les rôles d'autres utilisateurs ?
- [ ] Non — les utilisateurs **ne peuvent pas** voir ou énumérer les rôles/permissions d'autres entités  
- [ ] Oui — les utilisateurs **peuvent** énumérer les rôles via les pages de profil, les métadonnées ou les réponses d'API *(Moyenne)*  

---