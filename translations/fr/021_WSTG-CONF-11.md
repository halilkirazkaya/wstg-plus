## WSTG-CONF-11 — Tester le stockage Cloud

Le test des erreurs de configuration du stockage cloud implique l'identification et l'audit des services de stockage d'objets accessibles publiquement tels que Amazon S3, Azure Blobs ou Google Cloud Storage. Ces ressources contiennent souvent des données sensibles, notamment des sauvegardes, du code source, des PII ou des fichiers de configuration exposés en raison de listes de contrôle d'accès (ACL) ou de politiques de gestion des identités et des accès (IAM) trop permissives. Les attaquants énumèrent les noms des buckets par le biais de la reconnaissance des fichiers JavaScript, du code source HTML et de techniques de Brute Force afin de trouver des points d'entrée pour l'exfiltration de données ou la modification non autorisée de fichiers. L'exploitation peut entraîner un impact significatif, y compris des violations de données complètes ou la capacité de servir des fichiers malveillants à partir d'un domaine de confiance.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Élevée / Critique* |

> *La sévérité devient Critique si des PII, des identifiants ou des sauvegardes de production sont accessibles, ou si l'accès en écriture non authentifié est activé.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Les points de terminaison de stockage cloud (buckets/conteneurs) sont-ils identifiés et énumérés ?
- [ ] Non — aucun point de terminaison de stockage cloud n'est utilisé ou découvrable  
- [ ] Oui — les points de terminaison de stockage sont identifiés via l'analyse du code ou la surveillance du trafic  
- [ ] Oui — les points de terminaison de stockage sont découverts via Brute Force sur les conventions de nommage  

### Les utilisateurs non authentifiés peuvent-ils lister le contenu du stockage cloud ?
- [ ] Non — le listage du bucket est **désactivé** et renvoie 403 Forbidden ou 404 Not Found  
- [ ] Oui — le listage est **activé** mais aucun fichier sensible n'est présent  
- [ ] Oui — le listage est **activé** et les noms de fichiers/métadonnées sensibles sont visibles  

### Le téléchargement de fichiers sensibles à partir des buckets identifiés est-il possible ?
- [ ] Non — les fichiers sont protégés par des politiques IAM ou une authentification valide est requise  
- [ ] Oui — les fichiers sont accessibles mais nécessitent une URL signée qui **ne peut pas** être facilement devinée  
- [ ] Oui — les fichiers sensibles (sauvegardes, `.env`, PII) sont **publiquement accessibles** pour le téléchargement *(Critique)*  

### L'upload ou la modification de fichiers non authentifiés est-il autorisé ?
- [ ] Non — les uploads ou modifications non authentifiés ne sont **pas possibles**  
- [ ] Oui — un upload authentifié est requis mais un contournement **est possible** via des conditions de politique faibles  
- [ ] Oui — l'écriture, l'écrasement ou la suppression de fichiers non authentifiés est **possible** *(Critique)*  

### Les politiques de Cross-Origin Resource Sharing (CORS) sur le point de terminaison de stockage sont-elles restrictives ?
- [ ] Oui — CORS est **désactivé** ou restreint à des origines de confiance spécifiques  
- [ ] Non — CORS est **activé** avec un caractère joker `*` (wildcard), permettant un accès Web non autorisé  

---