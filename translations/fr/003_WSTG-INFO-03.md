## WSTG-INFO-03 — Examen des métafichiers du serveur Web pour les fuites d'informations

Les métafichiers du serveur Web tels que `robots.txt`, `sitemap.xml` et `security.txt` sont destinés à guider les crawlers (robots d'indexation) et à fournir des métadonnées administratives, mais ils révèlent souvent par inadvertance des structures de répertoires sensibles ou des points de terminaison (endpoints) cachés. Les attaquants analysent ces fichiers pour énumérer la surface d'attaque de l'application, identifiant les directives "Disallow" qui pointent fréquemment vers des panneaux d'administration non liés, des répertoires de sauvegarde ou des environnements de développement. Au-delà des instructions destinées aux moteurs de recherche, les fichiers du répertoire `.well-known/` ou des fichiers comme `humans.txt` peuvent divulguer des piles technologiques (technology stacks), des informations sur les développeurs ou des contacts de sécurité organisationnels. Un examen systématique de ces fichiers permet à un testeur d'obtenir des informations structurelles et techniques sans avoir recours à une découverte agressive par force brute (Brute Force).

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Statut du test** | Non effectué |
| **Sévérité** | Faible / Informationnel* |

> *La sévérité devient "Moyenne" si les métafichiers révèlent des interfaces d'administration non authentifiées ou des sauvegardes de configuration sensibles.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Le fichier `robots.txt` existe-t-il et expose-t-il des répertoires sensibles ?
- [ ] Non — `robots.txt` n'existe **pas**  
- [ ] Oui — `robots.txt` existe mais ne contient que des chemins publics standards  
- [ ] Oui — `robots.txt` révèle des chemins de répertoires sensibles ou des interfaces d'administration  
- [ ] Oui — `robots.txt` révèle des identifiants (credentials) ou des versions spécifiques de logiciels dans les commentaires  

### Un fichier `sitemap.xml` est-il présent et révèle-t-il une structure d'application cachée ?
- [ ] Non — `sitemap.xml` n'existe **pas**  
- [ ] Oui — `sitemap.xml` est présent mais ne liste que des URL publiques  
- [ ] Oui — `sitemap.xml` liste des points de terminaison non liés ou des ressources internes qui **peuvent** être consultés  

### Les métafichiers de sécurité et d'organisation exposent-ils des détails techniques ?
- [ ] Non — aucun fichier de métadonnées trouvé à la racine ou dans `.well-known/`  
- [ ] Oui — `security.txt` ou `humans.txt` sont présents mais contiennent des informations standards  
- [ ] Oui — les métafichiers divulguent des noms d'hôtes internes, des identités de développeurs ou des détails sur la pile technologique qui **peuvent** faciliter l'ingénierie sociale ou d'autres attaques  

### Existe-t-il des métafichiers hérités ou spécifiques à un framework ?
- [ ] Non — aucun fichier spécifique à un framework (ex. : `info.php`, `composer.json`, `package.json`) trouvé  
- [ ] Oui — des fichiers comme `README.md`, `CHANGELOG.txt` ou `.DS_Store` sont présents mais nettoyés  
- [ ] Oui — des fichiers spécifiques à un framework ou à une version sont présents et **permettent** un fingerprinting technologique précis  

---