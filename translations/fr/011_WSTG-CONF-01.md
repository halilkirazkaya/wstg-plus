## WSTG-CONF-01 — Tester la configuration de l'infrastructure réseau

Le test de configuration de l'infrastructure réseau consiste à identifier les faiblesses des composants serveur et réseau sous-jacents qui supportent l'application web. Les attaquants ciblent les services mal configurés, les protocoles obsolètes et les ports ouverts inutiles pour établir un point d'appui, effectuer des déplacements latéraux ou mener une phase de reconnaissance. Les vulnérabilités courantes incluent des versions TLS non sécurisées, des bannières de service verbeuses et des interfaces d'administration exposées qui devraient être restreintes aux réseaux internes. En exploitant ces failles au niveau de l'infrastructure, un adversaire peut compromettre la confidentialité des données en transit ou obtenir un accès non autorisé au système d'exploitation hôte.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Statut du test** | Non effectué |
| **Sévérité** | Faible / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Outils :** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Existe-t-il des ports ouverts inutiles ou des services exposés sur l'hôte de l'application ?
- [ ] Non — seuls les ports requis (ex : 443) sont **accessibles**  
- [ ] Oui — des services supplémentaires sont exposés mais suivent une configuration **sécurisée**  
- [ ] Oui — des services non essentiels (ex : FTP, Telnet, SMB) sont **exposés** publiquement  

### Les bannières ou les en-têtes de service divulguent-ils des informations de version sensibles ?
- [ ] Non — les bannières sont supprimées ou génériques  
- [ ] Oui — les bannières révèlent le logiciel serveur (ex : Apache/nginx) mais **pas** les numéros de version  
- [ ] Oui — les bannières verbeuses révèlent des versions logicielles spécifiques, facilitant l'**Exploit**  

### La configuration TLS respecte-t-elle les meilleures pratiques de sécurité modernes ?
- [ ] Oui — seuls TLS 1.2/1.3 sont **activés** avec des suites de chiffrement fortes  
- [ ] Oui — des protocoles hérités (TLS 1.0/1.1) sont **activés**  
- [ ] Non — des chiffrements faibles ou des protocoles non sécurisés (SSLv2/v3) sont **activés**  

### Les interfaces d'administration ou de gestion sont-elles restreintes aux réseaux autorisés ?
- [ ] Oui — les interfaces (ex : SSH, RDP, panneaux de contrôle) ne sont **pas accessibles** depuis l'internet public  
- [ ] Non — les interfaces sont **accessibles** publiquement mais nécessitent une authentification multi-facteurs  
- [ ] Non — les interfaces sont **accessibles** publiquement avec une authentification à facteur unique ou des identifiants par défaut (Default Credentials)  

---