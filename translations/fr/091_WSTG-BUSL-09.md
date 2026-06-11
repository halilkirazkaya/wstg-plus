## WSTG-BUSL-09 — Test de téléchargement de fichiers malveillants (Upload of Malicious Files)

La fonctionnalité de téléchargement de fichiers permet aux utilisateurs de soumettre des données au serveur, offrant ainsi un vecteur direct pour l'introduction de contenu malveillant dans l'environnement de l'application. Les attaquants exploitent des logiques de validation faibles pour télécharger des Web Shells, des malwares ou des fichiers spécialement conçus — tels que SVG ou HTML — afin de réaliser une exécution de code à distance (Remote Code Execution (RCE)) ou du Cross-Site Scripting (XSS). Cette vulnérabilité se retrouve généralement dans des fonctionnalités telles que les paramètres de profil, les systèmes de gestion documentaire et les gestionnaires de pièces jointes, où le serveur ne parvient pas à vérifier correctement les types de fichiers, leur contenu et les permissions d'exécution. Une exploitation réussie conduit souvent à une compromission totale du système, à l'exfiltration de données ou à l'utilisation du serveur comme point de pivot pour des attaques sur le réseau interne.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **État du test** | Non effectué |
| **Sévérité** | Haute / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Outils :** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### L'application propose-t-elle une fonctionnalité de téléchargement de fichiers ?
- [ ] Non — aucune fonctionnalité de téléchargement de fichiers n'existe  
- [ ] Oui — la fonctionnalité existe pour les utilisateurs authentifiés  
- [ ] Oui — la fonctionnalité est disponible pour les utilisateurs **non authentifiés** *(Critique)*  

### La validation de l'extension de fichier est-elle appliquée côté serveur ?
- [ ] Oui — une allowlist (liste d'autorisation) stricte d'extensions est **appliquée** et le contournement est **impossible**  
- [ ] Oui — une allowlist est utilisée mais un contournement **est possible** via la sensibilité à la casse ou les doubles extensions  
- [ ] Oui — une blocklist (liste de blocage) est utilisée, laquelle **peut** être contournée avec des extensions alternatives (ex : `.phtml`, `.asa`)  
- [ ] Non — aucune validation d'extension n'est **appliquée** côté serveur  

### Le contenu du fichier est-il validé au-delà de l'extension ou du type MIME ?
- [ ] Oui — l'application vérifie les magic bytes et effectue une inspection approfondie du contenu  
- [ ] Oui — l'application vérifie uniquement l'en-tête `Content-Type`, qui **est** facilement falsifiable  
- [ ] Non — l'application se repose entièrement sur l'extension du fichier pour la validation  

### Les fichiers téléchargés sont-ils stockés dans un répertoire disposant de permissions d'exécution ?
- [ ] Non — les fichiers sont stockés sur un service de stockage dédié (ex : S3) ou dans un répertoire non exécutable  
- [ ] Oui — les fichiers sont stockés sur le serveur web mais l'exécution est **désactivée** via la configuration  
- [ ] Oui — les fichiers sont stockés dans un répertoire accessible par le web et l'exécution **est activée** *(Critique)*  

### Les filtres de sécurité peuvent-ils être contournés via la manipulation du nom de fichier ?
- [ ] Non — les noms de fichiers sont assainis (sanitized) ou régénérés aléatoirement par le serveur  
- [ ] Oui — les filtres **peuvent** être contournés en utilisant des null bytes (ex : `shell.php%00.jpg`)  
- [ ] Oui — des caractères de Path Traversal (ex : `../../`) **peuvent** être utilisés pour télécharger des fichiers dans des répertoires arbitraires  

---