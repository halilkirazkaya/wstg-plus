## WSTG-INFO-04 — Applicaties op de webserver enumereren

Applicatie-enumeratie is het proces van het identificeren van alle webapplicaties en diensten die worden gehost op een enkele webserver of IP-adres. Omdat moderne webservers vaak meerdere Virtual Hosts, verouderde staging-omgevingen of administratieve interfaces op niet-standaard poorten en paden hosten, blijft een aanzienlijk deel van het aanvalsoppervlak (attack surface) ongetest als deze verborgen assets niet worden geïdentificeerd. Aanvallers maken gebruik van technieken zoals DNS zone transfers, virtual host brute-forcing en reverse IP lookups om deze vergeten of obscure toegangspunten te ontdekken, die vaak minder veilig zijn dan de primaire productie-applicatie.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Teststatus** | Niet uitgevoerd |
| **Severity** | Informatief / Laag |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Tools:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Zijn er meerdere IP-adressen of DNS-aliassen gekoppeld aan de doel-infrastructuur?
- [ ] Nee — er bestaat slechts één IP en A-record  
- [ ] Ja — er zijn meerdere IP-adressen of aliassen geïdentificeerd, wat de scope vergroot  

### Host de webserver meerdere Virtual Hosts (vhosts) op hetzelfde IP-adres?
- [ ] Nee — alleen het primaire domein wordt door de server bediend  
- [ ] Ja — aanvullende Virtual Hosts zijn geïdentificeerd, maar toegang is niet mogelijk (bijv. 403 Forbidden)  
- [ ] Ja — verborgen, development- of staging-Virtual Hosts zijn geïdentificeerd en toegankelijk  

### Worden applicaties gehost op niet-standaard poorten?
- [ ] Nee — alleen standaardpoorten (80/443) zijn open  
- [ ] Ja — niet-standaard poorten zijn open, maar hosten geen webapplicaties  
- [ ] Ja — aanvullende webapplicaties zijn geïdentificeerd op niet-standaard poorten (bijv. 8080, 8443, 9000)  

### Zijn er afzonderlijke applicaties toegewezen aan specifieke URI-paden?
- [ ] Nee — één enkele applicatie bedient de volledige root-directory  
- [ ] Ja — er zijn meerdere applicaties ontdekt via directory-enumeratie (bijv. `/api`, `/portal`, `/backup`)  

### Onthullen reconnaissance-diensten van derden historische of secundaire applicaties?
- [ ] Nee — geen significante bevindingen via `Shodan`, `Censys` of `Wayback Machine`  
- [ ] Ja — historische snapshots of scans onthullen applicaties die momenteel actief zijn, maar niet gelinkt  

---