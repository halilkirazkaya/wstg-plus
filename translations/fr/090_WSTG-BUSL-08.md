## WSTG-BUSL-08 — Test de téléchargement de types de fichiers inattendus

Le téléchargement de types de fichiers inattendus permet aux attaquants de contourner les contraintes de la logique métier en soumettant des fichiers que l'application n'a pas l'intention de traiter, ce qui peut potentiellement mener à une exécution de code à distance (Remote Code Execution), à du Cross-Site Scripting (XSS) ou à un déni de service (Denial of Service). Les attaquants sondent ces vulnérabilités en modifiant les extensions de fichiers, les types MIME et les Magic Bytes pour tromper la logique de validation côté serveur et l'inciter à accepter du contenu malveillant. Cela se produit généralement lors du téléchargement de photos de profil, dans les systèmes de gestion de documents ou les pièces jointes de tickets de support où la validation côté back-end est insuffisante. Une exploitation réussie peut compromettre le serveur hôte si le fichier téléchargé est exécuté ou mener à des attaques côté client si le fichier est renvoyé à d'autres utilisateurs avec un Content-Type incorrect.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Statut du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Outils :** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### L'application restreint-elle le téléchargement de fichiers à un ensemble spécifique d'extensions ?
- [ ] Oui — une liste blanche (Whitelist) stricte d'extensions est appliquée et un contournement n'est **pas possible**  
- [ ] Oui — une liste blanche d'extensions est présente mais un contournement **est possible** via des doubles extensions ou des octets nuls (Null Bytes)  
- [ ] Non — n'importe quelle extension de fichier **peut** être téléchargée  

### Le contenu du fichier est-il validé au-delà de l'extension du fichier ?
- [ ] Oui — la logique côté serveur vérifie les Magic Bytes et effectue une inspection approfondie du contenu  
- [ ] Oui — la logique côté serveur vérifie le type MIME via l'en-tête `Content-Type` uniquement  
- [ ] Non — aucune validation de contenu n'est **appliquée** après la vérification de l'extension  

### Un attaquant peut-il exécuter du code en téléchargeant un Web Shell ou un script ?
- [ ] Non — les fichiers téléchargés sont stockés dans un répertoire non exécutable ou un compartiment de stockage (S3/Azure Blob)  
- [ ] Oui — les fichiers sont stockés dans un répertoire accessible par le Web, mais l'exécution **est désactivée** via la configuration du serveur  
- [ ] Oui — les scripts malveillants téléchargés **peuvent** être exécutés directement via un chemin URL prévisible *(Critique)*  

### Des attaques côté client comme le XSS sont-elles possibles via les types de fichiers téléchargés ?
- [ ] Non — les fichiers sont servis avec `Content-Disposition: attachment` et `X-Content-Type-Options: nosniff`  
- [ ] Oui — des fichiers SVG ou HTML **peuvent** être téléchargés et rendus dans le navigateur, menant à un XSS  
- [ ] Oui — les métadonnées d'image (EXIF) **sont traitées** et reflétées dans l'interface utilisateur sans assainissement (Sanitization)  

---