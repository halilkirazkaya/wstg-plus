## WSTG-ATHZ-01 — Testing Directory Traversal File Include

Les vulnérabilités de traversée de répertoire (Directory Traversal) et d'inclusion de fichiers surviennent lorsqu'une application utilise une entrée contrôlable par l'utilisateur pour construire des chemins vers des fichiers ou des répertoires sans validation ou assainissement (sanitization) suffisants. Les attaquants exploitent ces failles en injectant des séquences telles que `../` pour naviguer en dehors du répertoire prévu, accédant potentiellement à des fichiers système sensibles, des données de configuration ou au code source de l'application. Dans des cas plus graves impliquant l'inclusion de fichiers locaux (LFI) ou l'inclusion de fichiers distants (RFI), un attaquant peut parvenir à une exécution de code à distance (RCE) en incluant des scripts malveillants ou en exploitant l'empoisonnement de logs (log poisoning) et les wrappers PHP. Ces vulnérabilités résident généralement dans les paramètres utilisés pour le chargement dynamique de contenu, les moteurs de template ou les points de terminaison de récupération d'images où la logique côté serveur gère les chemins du système de fichiers.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Statut du test** | Non effectué |
| **Sévérité** | Haute / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Outils :** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Les paramètres acceptent-ils des noms de fichiers ou des chemins pour un traitement côté serveur ?
- [ ] Non — aucun paramètre ne semble interagir avec le système de fichiers  
- [ ] Oui — des paramètres existent mais utilisent une liste d'autorisation (allowlist) stricte d'identifiants de fichiers  
- [ ] Oui — les paramètres acceptent directement des noms de fichiers ou des chemins  

### L'entrée est-elle assainie pour empêcher les séquences de traversée de répertoire ?
- [ ] Oui — l'entrée est validée par rapport à une liste d'autorisation stricte et les séquences de traversée ne sont **pas possibles**  
- [ ] Oui — l'entrée est assainie en supprimant les séquences `../`, mais un contournement récursif **est possible**  
- [ ] Non — aucun assainissement ou validation n'est **appliqué** aux entrées liées aux chemins  

### Est-il possible d'accéder à des fichiers en dehors du répertoire restreint via des séquences de traversée ?
- [ ] Non — l'application ou l'OS empêche l'accès aux fichiers en dehors du périmètre défini  
- [ ] Oui — l'accès aux fichiers à l'intérieur de la racine web (web root) **est possible**  
- [ ] Oui — l'accès à des fichiers système sensibles (ex : `/etc/passwd`, `C:\Windows\win.ini`) **est possible** *(Critique)*  

### Le serveur permet-il l'inclusion d'URLs distantes (RFI) ?
- [ ] Non — l'inclusion de fichiers distants est **désactivée** au niveau du serveur/de l'application  
- [ ] Oui — des fichiers distants **peuvent** être inclus mais l'exécution n'est **pas possible**  
- [ ] Oui — des fichiers distants **peuvent** être inclus et exécutés sur le serveur *(Critique)*  

### Les filtres peuvent-ils être contournés en utilisant l'encodage ou des caractères spéciaux ?
- [ ] Non — les filtres gèrent efficacement divers encodages et les octets nuls (null bytes)  
- [ ] Oui — le contournement **est possible** en utilisant l'encodage URL, le double encodage URL ou l'Unicode 16 bits  
- [ ] Oui — le contournement **est possible** en utilisant l'injection d'octets nuls (`%00`) ou des wrappers de système de fichiers (ex : `php://filter`)  

---