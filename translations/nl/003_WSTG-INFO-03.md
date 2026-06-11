## WSTG-INFO-03 — Review Webserver Metafiles op Information Leakage

Webserver-metafiles zoals `robots.txt`, `sitemap.xml` en `security.txt` zijn bedoeld om crawlers aan te sturen en administratieve metadata te verstrekken, maar ze onthullen vaak onbedoeld gevoelige mappenstructuren of verborgen endpoints. Aanvallers analyseren deze bestanden om het aanvalsoppervlak (Attack Surface) van de applicatie in kaart te brengen, waarbij ze "Disallow"-richtlijnen identificeren die vaak wijzen naar niet-gekoppelde administratieve panels, back-upmappen of ontwikkelomgevingen. Naast instructies voor zoekmachines kunnen bestanden in de `.well-known/` map of bestanden zoals `humans.txt` technologiestacks, ontwikkelaarsinformatie of beveiligingscontacten van de organisatie onthullen. Systematische beoordeling van deze bestanden stelt een tester in staat om structurele en technische inzichten te verkrijgen zonder agressieve Brute Force discovery uit te voeren.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Laag / Informatief* |

> *Severity wordt Medium als metafiles niet-geauthenticeerde administratieve interfaces of gevoelige configuratieback-ups onthullen.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Bestaat het `robots.txt` bestand en worden er gevoelige mappen blootgesteld?
- [ ] Nee — `robots.txt` bestaat **niet**  
- [ ] Ja — `robots.txt` bestaat maar bevat alleen standaard publieke paden  
- [ ] Ja — `robots.txt` onthult gevoelige mappaden of administratieve interfaces  
- [ ] Ja — `robots.txt` onthult credentials of specifieke softwareversies in commentaar  

### Is er een `sitemap.xml` bestand aanwezig en onthult dit de verborgen applicatiestructuur?
- [ ] Nee — `sitemap.xml` bestaat **niet**  
- [ ] Ja — `sitemap.xml` is aanwezig maar bevat alleen publiek toegankelijke URL's  
- [ ] Ja — `sitemap.xml` bevat niet-gekoppelde endpoints of interne bronnen die wel toegankelijk **zijn**  

### Onthullen beveiligings- en organisatorische metafiles technische details?
- [ ] Nee — geen metadata-bestanden gevonden in de root of `.well-known/`  
- [ ] Ja — `security.txt` of `humans.txt` zijn aanwezig maar bevatten standaardinformatie  
- [ ] Ja — metafiles onthullen interne hostnamen, identiteiten van ontwikkelaars of details over de technologiestack die kunnen helpen bij social engineering of verdere aanvallen  

### Zijn er legacy of framework-specifieke metafiles aanwezig?
- [ ] Nee — geen framework-specifieke bestanden (bijv. `info.php`, `composer.json`, `package.json`) gevonden  
- [ ] Ja — bestanden zoals `README.md`, `CHANGELOG.txt` of `.DS_Store` zijn aanwezig maar geschoond (sanitized)  
- [ ] Ja — framework- of versiespecifieke bestanden zijn aanwezig en **maken** nauwkeurige technology fingerprinting mogelijk  

---