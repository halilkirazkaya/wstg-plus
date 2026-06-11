## WSTG-SESS-10 — Testen von JSON Web Tokens

JSON Web Tokens (JWTs) werden häufig für das stateless Session-Management und die Identitätspropagation verwendet, aber ihre Sicherheit beruht vollständig auf der korrekten Implementierung von Signaturalgorithmen und einer strengen Claim-Verifizierung. Angreifer versuchen häufig, die Authentifizierung zu umgehen, indem sie Schwachstellen im „none“-Algorithmus ausnutzen, schwache HS256-Secret-Keys mittels Brute Force angreifen oder Algorithm-Switching-Angriffe durchführen, bei denen ein asymmetrischer öffentlicher Schlüssel als symmetrisches HMAC-Secret verwendet wird. Diese Schwachstellen treten typischerweise in Session-Cookies oder Authorization-Headern auf und ermöglichen es einem Angreifer potenziell, Berechtigungen zu erweitern (Privilege Escalation) oder beliebige Benutzer zu imitieren (Impersonation), indem Claims wie `role` oder `user_id` modifiziert werden, ohne die kryptografische Integrität des Tokens zu verletzen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Tools:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### Wird der „none“-Algorithmus vom Server akzeptiert?
- [ ] Nein — der Server **weist** Tokens mit `alg: none` im Header **ab**  
- [ ] Ja — der Server **akzeptiert** Tokens mit `alg: none` und einer leeren Signatur *(Kritisch)*  

### Wie wird die JWT-Signatur verifiziert und gegen Brute Force geschützt?
- [ ] Ja — es werden starke asymmetrische Schlüssel (RS256/ES256) oder HMAC-Secrets mit hoher Entropie verwendet  
- [ ] Ja — HS256 wird verwendet, aber das Secret **kann** aufgrund geringer Entropie oder Standardwerten mittels Brute Force geknackt werden  
- [ ] Nein — die Signatur-Verifizierung **wird nicht angewendet** oder **kann** durch Algorithm-Switching (RS256 zu HS256) umgangen werden  

### Werden Standard-Claims (exp, nbf, iat) vom Backend strikt erzwungen?
- [ ] Ja — `exp` (Expiration) ist vorhanden und **kann nicht** umgangen werden  
- [ ] Ja — `exp` ist vorhanden, aber der Server **erzwingt** die Prüfung während der Validierung **nicht**  
- [ ] Nein — Expiration-Claims **fehlen** oder werden ignoriert, was ein zeitlich unbegrenztes Session Replay ermöglicht  

### Verhindert die Implementierung Header-basierte Key-Injection (kid, jku, x5u)?
- [ ] Ja — Header werden bereinigt (Sanitization) und nur vertrauenswürdige interne Schlüssel/Quellen sind **erlaubt**  
- [ ] Nein — der `kid` (Key ID) Header ist anfällig für Directory Traversal oder SQL Injection, um auf eine bekannte Datei zu verweisen  
- [ ] Nein — die `jku` oder `x5u` Header **können** auf vom Angreifer kontrollierte URLs verwiesen werden, um bösartige Schlüssel zu laden  

---