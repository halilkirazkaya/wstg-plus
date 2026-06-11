## WSTG-INPV-07 — Injection XML

L'injection XML (XML Injection) se produit lorsqu'une application incorpore des données fournies par l'utilisateur dans un document ou un flux XML sans validation, assainissement (sanitization) ou échappement (escaping) appropriés des méta-caractères XML. En injectant des balises XML spécifiques ou des éléments structurels, un attaquant peut manipuler la logique du document, contourner les mécanismes d'authentification ou interférer avec l'intégrité des données traitées par le backend. Cette vulnérabilité est couramment rencontrée dans les services web basés sur SOAP, les API REST acceptant le XML, les assertions SAML et les applications qui génèrent dynamiquement des fichiers de configuration ou des rapports XML. Du point de vue de l'attaquant, l'objectif est de sortir du contexte de données prévu pour altérer la structure du document, ce qui peut potentiellement conduire à une élévation de privilèges non autorisée ou à l'exécution d'une logique métier non souhaitée.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Haute / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Outils :** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### L'application accepte-t-elle et traite-t-elle des entrées au format XML provenant de l'utilisateur ?
- [ ] Non — l'application ne traite **pas** d'entrées XML  
- [ ] Oui — les entrées XML sont traitées via des API (SOAP/REST)  
- [ ] Oui — le XML est traité via des téléversements de fichiers (SVG, DOCX, etc.)  

### Les méta-caractères XML (ex : `<`, `>`, `&`, `'`, `"`) sont-ils correctement échappés ou assainis ?
- [ ] Oui — tous les méta-caractères sont **correctement** échappés ou assainis *(Le plus sécurisé)*  
- [ ] Oui — un certain filtrage est appliqué mais un contournement (bypass) **est possible** via l'encodage  
- [ ] Non — les entrées brutes sont directement intégrées dans les structures XML **sans** assainissement  

### La validation de schéma côté serveur (XSD/DTD) est-elle imposée pour toutes les entrées XML ?
- [ ] Oui — une validation de schéma stricte est **activée** et empêche les modifications structurelles  
- [ ] Oui — la validation est **activée** mais n'empêche **pas** l'injection de balises dans les champs de données  
- [ ] Non — la validation de schéma est **désactivée** ou non implémentée  

### Un attaquant peut-il injecter de nouvelles balises XML pour altérer la structure ou la logique du document ?
- [ ] Non — la structure reste intacte quelles que soient les entrées  
- [ ] Oui — des balises peuvent être injectées mais **ne peuvent pas** altérer la logique métier  
- [ ] Oui — l'injection de balises **est possible** et permet de contourner la logique ou de modifier les données *(Critique)*  

### L'analyseur XML (XML parser) peut-il être manipulé à l'aide de sections CDATA ou de commentaires XML ?
- [ ] Non — l'analyseur traite ou rejette correctement les marqueurs CDATA/commentaires  
- [ ] Oui — l'injection de `<![CDATA[...]]>` ou `<!-- ... -->` **est possible** pour masquer ou contourner les filtres  

---