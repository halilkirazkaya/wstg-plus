## WSTG-CONF-11 — Cloudopslag testen

Het testen op misconfiguraties van cloudopslag omvat het identificeren en auditen van publiekelijk toegankelijke object storage services zoals Amazon S3, Azure Blobs, of Google Cloud Storage. Deze bronnen bevatten vaak gevoelige gegevens, waaronder back-ups, broncode, PII of configuratiebestanden die zijn blootgesteld door te tolerante Access Control Lists (ACLs) of Identity and Access Management (IAM) policies. Aanvallers enumereren bucket-namen via reconnaissance van JavaScript-bestanden, HTML-broncode en brute-force technieken om toegangspunten te vinden voor data exfiltratie of ongeautoriseerde bestandsaanpassingen. Exploitatie kan leiden tot aanzienlijke impact, waaronder volledige datalekken of de mogelijkheid om kwaadaardige bestanden te serveren vanaf een vertrouwd domein.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek* |

> *De ernst wordt als Kritiek beschouwd als PII, credentials of productieback-ups toegankelijk zijn, of als ongeauthenticeerde schrijftoegang is ingeschakeld.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Zijn de cloudopslag-endpoints (buckets/containers) geïdentificeerd en geënumereerd?
- [ ] Nee — er worden geen cloudopslag-endpoints gebruikt of deze zijn niet vindbaar  
- [ ] Ja — storage-endpoints zijn geïdentificeerd via code-analyse of verkeersmonitoring  
- [ ] Ja — storage-endpoints zijn ontdekt via brute-force op basis van naamgevingsconventies  

### Kunnen ongeauthenticeerde gebruikers de inhoud van de cloudopslag listen?
- [ ] Nee — bucket-listing is **uitgeschakeld** en retourneert 403 Forbidden of 404 Not Found  
- [ ] Ja — listing is **ingeschakeld**, maar er zijn geen gevoelige bestanden aanwezig  
- [ ] Ja — listing is **ingeschakeld** en gevoelige bestandsnamen/metadata zijn zichtbaar  

### Is het downloaden van gevoelige bestanden uit geïdentificeerde buckets mogelijk?
- [ ] Nee — bestanden zijn beveiligd door IAM-policies of een geldige authenticatie is vereist  
- [ ] Ja — bestanden zijn toegankelijk, maar vereisen een signed URL die **niet** eenvoudig te raden is  
- [ ] Ja — gevoelige bestanden (back-ups, `.env`, PII) zijn **publiekelijk toegankelijk** voor download *(Kritiek)*  

### Is ongeauthenticeerde bestandsupload of -wijziging toegestaan?
- [ ] Nee — ongeauthenticeerde uploads of wijzigingen zijn **niet mogelijk**  
- [ ] Ja — geauthenticeerde upload is vereist, maar een bypass **is mogelijk** via zwakke policy-voorwaarden  
- [ ] Ja — ongeauthenticeerd schrijven, overschrijven of verwijderen van bestanden is **mogelijk** *(Kritiek)*  

### Zijn de Cross-Origin Resource Sharing (CORS) policies op het storage-endpoint beperkend?
- [ ] Ja — CORS is **uitgeschakeld** of beperkt tot specifieke vertrouwde origins  
- [ ] Nee — CORS is **ingeschakeld** met een wildcard `*` origin, wat ongeautoriseerde webgebaseerde toegang mogelijk maakt  

---