## WSTG-BUSL-04 — Test auf Process Timing

Process Timing-Angriffe beinhalten das Messen der Zeit, die eine Anwendung zur Verarbeitung spezifischer Anfragen benötigt, um Rückschlüsse auf sensible Informationen über interne Zustände oder die Existenz von Daten zu ziehen. Durch die Analyse von Abweichungen im Millisekundenbereich bei den Antwortzeiten — oft verursacht durch Early-Exit-Logik, Datenbankabfragen oder kryptographische Vergleiche mit nicht-konstanter Laufzeit — können Angreifer eine Seitenkanal-Analyse (Side-Channel Analysis) durchführen, um valide Benutzernamen aufzuzählen (Enumerate), Passwortfragmente zu verifizieren oder die Existenz spezifischer Datensätze zu bestätigen. Diese Schwachstellen treten typischerweise an Authentifizierungs-Endpunkten, Passwort-Reset-Formularen und ressourcenintensiven API-Filtern auf, bei denen die Backend-Logik basierend auf der Gültigkeit der Eingabe verzweigt. Aus der Sicht eines Angreifers dienen diese Zeitunterschiede als „Boolean Oracle“, das Wahrheiten über die Backend-Daten offenbart, selbst wenn die Anwendung identische, generische Fehlermeldungen an den Benutzer zurückgibt.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Test Status** | Nicht durchgeführt |
| **Schweregrad** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Tools:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### Weist die Anwendung unterschiedliche Antwortzeiten für valide gegenüber invaliden Ressourcenanfragen auf?
- [ ] Nein — Antwortzeiten sind unabhängig von der Gültigkeit der Eingabe identisch  
- [ ] Ja — es existieren vernachlässigbare Zeitunterschiede, die jedoch **nicht** statistisch signifikant sind  
- [ ] Ja — konsistente Zeitunterschiede sind **beobachtbar** und bieten ein zuverlässiges Oracle  

### Sind Vergleichsfunktionen mit konstanter Laufzeit (Constant-Time Comparison) oder künstliche Verzögerungen implementiert, um die Antwortzeiten zu normalisieren?
- [ ] Ja — Constant-Time-Algorithmen werden auf alle sensiblen Vergleichsoperationen **angewendet** *(Am sichersten)*  
- [ ] Ja — künstliche Verzögerungen oder Jitter sind **aktiviert**, aber die zugrunde liegende Logik bleibt ohne konstante Laufzeit  
- [ ] Nein — es werden keine Maßnahmen zur zeitlichen Absicherung (Timing Mitigations) **angewendet**, was die direkte Messung von Logikverzweigungen ermöglicht  

### Kann ein Angreifer statistische Analysen nutzen, um das Timing-Signal vom Netzwerkrauschen zu isolieren?
- [ ] Nein — Netzwerk-Jitter und Anwendungsrauschen machen eine Timing-Analyse **unmöglich**  
- [ ] Ja — mit einer ausreichenden Stichprobengröße **kann** das Timing-Signal vom Rauschen isoliert werden  
- [ ] Ja — das Timing-Delta ist groß genug, sodass eine statistische Normalisierung **nicht** erforderlich ist  

### Ist eine Account Enumeration über die Login- oder Passwort-Reset-Funktionalität möglich?
- [ ] Nein — die Existenz eines Accounts kann nicht durch Timing bestimmt werden  
- [ ] Ja — Zeitdiskrepanzen ermöglichen es einem Remote-Angreifer, valide Benutzernamen oder E-Mails **aufzuzählen (Enumerate)**  

### Lassen ressourcenintensive Operationen (z. B. Dateiverarbeitung, komplexe Suchen) die Existenz von Daten über das Timing erkennen?
- [ ] Nein — der Ressourcenverbrauch ist einheitlich, unabhängig davon, ob ein Datensatz gefunden wird  
- [ ] Ja — die Existenz sensibler Datensätze **kann** durch Messung der Dauer der Backend-Verarbeitung abgeleitet werden  

---