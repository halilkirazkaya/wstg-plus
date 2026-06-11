## WSTG-INPV-13 — Format String Injection

L'injection de chaîne de format (Format String Injection) se produit lorsqu'une application web transmet une entrée contrôlée par l'utilisateur directement dans l'argument de chaîne de format d'une fonction variadique, telle que la famille `printf` en C ou des wrappers de journalisation (logging) de bas niveau. Bien que moins courante dans les frameworks web modernes à code managé, cette vulnérabilité reste critique dans les applications CGI héritées (legacy), les extensions natives ou les systèmes de journalisation spécifiques interagissant avec des bibliothèques système de bas niveau. Les attaquants exploitent cela en fournissant des spécificateurs de format tels que `%x` ou `%p` pour divulguer des adresses mémoire sensibles et des données de la pile (stack), ou le spécificateur `%n` pour effectuer des écritures mémoire arbitraires. Une exploitation réussie entraîne généralement un compromis complet du système via une exécution de code à distance (Remote Code Execution - RCE) ou, au minimum, une condition de déni de service (DoS) impactante par le biais de plantages de processus.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### L'application traite-t-elle les entrées utilisateur via du code natif ou des interfaces CGI héritées ?
- [ ] Non — l'application est entièrement construite sur des frameworks à code managé (ex: JVM, CLR)  
- [ ] Oui — l'application utilise JNI, des extensions C/C++ natives ou des CGI binaires hérités où des vulnérabilités de type format string **peuvent** exister  

### Les entrées fournies par l'utilisateur sont-elles transmises comme argument principal à des fonctions sensibles au format ?
- [ ] Non — les entrées sont transmises comme arguments de données, et les chaînes de format sont **statiques** et **codées en dur**  
- [ ] Oui — l'entrée utilisateur est directement concaténée dans l'argument de chaîne de format, mais une désinfection (sanitization) **est appliquée**  
- [ ] Oui — l'entrée utilisateur est directement utilisée comme chaîne de format et aucune désinfection **n'est appliquée**  

### Est-il possible de divulguer le contenu de la mémoire à l'aide de spécificateurs de format ?
- [ ] Non — l'entrée est validée et les spécificateurs tels que `%p`, `%x` ou `%s` ne sont **pas traités**  
- [ ] Oui — la fourniture de spécificateurs `%p` ou `%x` entraîne la réflexion d'adresses mémoire de la pile (stack) ou du tas (heap)  
- [ ] Oui — des données sensibles (ex: pointeurs, stack cookies) **peuvent** être exfiltrées via des spécificateurs de format  

### L'application peut-elle être plantée ou manipulée à l'aide du spécificateur `%n` ?
- [ ] Non — les spécificateurs permettant des écritures en mémoire sont **désactivés** ou bloqués par le compilateur/l'environnement  
- [ ] Oui — l'application plante (DoS) lorsque `%s%s%s%s` ou des chaînes similaires sont fournies  
- [ ] Oui — des écritures mémoire arbitraires via `%n` sont **possibles**, menant potentiellement à une Remote Code Execution (RCE)  

### Les protections binaires modernes (ASLR, DEP, Stack Canaries) sont-elles contournables via cette injection ?
- [ ] Non — l'environnement est durci et la fuite de mémoire **ne peut pas** être utilisée pour contourner les protections  
- [ ] Oui — l'injection de chaîne de format **peut** être utilisée pour divulguer des adresses de base, rendant le contournement de l'ASLR/DEP **possible**  

---