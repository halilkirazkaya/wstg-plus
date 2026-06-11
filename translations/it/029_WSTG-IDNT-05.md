## WSTG-IDNT-05 — Testing for Weak or Unenforced Username Policy

La verifica di policy degli username deboli o non applicate si concentra sulle regole che governano la creazione degli identificativi degli account e sulla loro suscettibilità all'enumeration o allo spoofing. Policy deboli spesso permettono sequenze prevedibili, parole comuni di dizionario o identificativi che rispecchiano gli ID interni dei dipendenti, il che riduce significativamente lo spazio di ricerca per attacchi di Brute Force e Credential Stuffing. Dal punto di vista di un attaccante, l'identificazione di questi pattern è il primo passo per la scoperta di account su larga scala, spesso ottenuta analizzando i messaggi di errore di registrazione, il comportamento del reset della password o i metadati nei profili pubblici. Garantire una policy robusta impedisce agli attaccanti di mappare sistematicamente la base utenti e facilita la difesa contro phishing mirato e tentativi automatizzati di Authentication Bypass.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Bassa / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Gli username si basano su pattern altamente prevedibili o sequenziali?
- [ ] No — gli username sono stringhe casuali, definite dall'utente o ad alta entropia  
- [ ] Sì — gli username seguono un formato prevedibile (es. `user1001`, `user1002`) ma l'autenticazione è robusta  
- [ ] Sì — gli username sono **rigorosamente sequenziali** o seguono un formato aziendale noto, facilitando l'enumeration  

### L'applicazione impone requisiti minimi di lunghezza e complessità per gli username?
- [ ] Sì — policy rigorose sulla lunghezza e sul set di caratteri **sono applicate** durante la registrazione  
- [ ] Sì — esistono delle policy ma permettono username estremamente brevi (es. 1-2 caratteri) o eccessivamente semplici  
- [ ] No — **nessuna policy** viene applicata riguardo alla lunghezza dello username o ai tipi di caratteri  

### È possibile enumerare username validi attraverso le risposte dell'applicazione?
- [ ] No — l'applicazione restituisce messaggi di errore generici e mostra un timing coerente per tutti i tentativi  
- [ ] Sì — l'applicazione restituisce messaggi di errore distinti (es. "Username già in uso") ma il Rate Limiting **viene applicato**  
- [ ] Sì — l'applicazione **rivela (leaks)** username validi tramite errori di registrazione, reset della password o differenze di timing  

### La policy impedisce la registrazione di username amministrativi comuni o riservati?
- [ ] Sì — l'applicazione blocca nomi comuni (es. `admin`, `root`, `support`) e termini riservati di sistema  
- [ ] No — gli utenti **possono** registrare account con nomi sensibili o amministrativi  

### La policy degli username è coerente in tutti i punti di ingresso (API, Mobile, Web)?
- [ ] Sì — le policy **sono applicate** in modo coerente in tutte le interfacce e versioni  
- [ ] No — gli endpoint legacy o versioni specifiche delle API **non impongono** gli stessi vincoli sugli username  

---