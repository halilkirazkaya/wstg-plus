## WSTG-SESS-10 — JSON Web Tokens (JWTs) کی جانچ پڑتال

JSON Web Tokens (JWTs) عام طور پر بے ریاست سیشن مینجمنٹ (Stateless Session Management) اور شناخت کی ترسیل (Identity Propagation) کے لیے استعمال کیے جاتے ہیں، لیکن ان کی سیکیورٹی کا انحصار مکمل طور پر سائننگ الگورتھمز (Signing Algorithms) کے درست نفاذ اور کلیمز کی سخت تصدیق (Claim Verification) پر ہوتا ہے۔ حملہ آور اکثر "none" الگورتھم کی خامیوں کا فائدہ اٹھا کر، کمزور HS256 خفیہ کلیدوں (Secret Keys) پر بروٹ فورس (Brute-forcing) کر کے، یا الگورتھم سوئچنگ (Algorithm-switching) حملے انجام دے کر تصدیق کے عمل کو بائی پاس کرنے کی کوشش کرتے ہیں، جہاں ایک غیر متناسب عوامی کلید (Asymmetric Public Key) کو بطور متناسب HMAC خفیہ (Symmetric HMAC Secret) استعمال کیا جاتا ہے۔ یہ کمزوریاں عام طور پر سیشن کوکیز (Session Cookies) یا آتھرائزیشن ہیڈرز (Authorization Headers) میں ظاہر ہوتی ہیں، جو ممکنہ طور پر حملہ آور کو ٹوکن کی کرپٹوگرافک سالمیت (Cryptographic Integrity) کو متاثر کیے بغیر `role` یا `user_id` جیسے کلیمز (Claims) میں ترمیم کر کے مراعات میں اضافے (Privilege Escalation) یا کسی بھی صارف کا روپ دھارنے (Impersonation) کی اجازت دے سکتی ہیں۔


| فیلڈ | ویلیو |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **ٹیسٹ کی صورتحال** | انجام نہیں دیا گیا |
| **سنگینی** | زیادہ / انتہائی سنگین |

**حوالہ جات:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**اوزار:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### کیا سرور کے ذریعے "none" الگورتھم قبول کیا جاتا ہے؟
- [ ] نہیں — سرور ہیڈر میں `alg: none` استعمال کرنے والے ٹوکنز کو **مسترد** کر دیتا ہے  
- [ ] جی ہاں — سرور `alg: none` اور خالی دستخط (Empty Signature) والے ٹوکنز کو **قبول** کرتا ہے *(انتہائی سنگین)*  

### JWT دستخط کی تصدیق کیسے کی جاتی ہے اور اسے بروٹ فورس سے کیسے محفوظ رکھا جاتا ہے؟
- [ ] جی ہاں — مضبوط غیر متناسب کلیدیں (Asymmetric Keys - RS256/ES256) یا اعلیٰ اینٹروپی والے HMAC خفیہ کوڈز استعمال کیے جاتے ہیں  
- [ ] جی ہاں — HS256 استعمال کیا گیا ہے لیکن کم اینٹروپی یا ڈیفالٹ اقدار کی وجہ سے خفیہ کوڈ پر بروٹ فورس (Brute-force) **کیا جا سکتا ہے**  
- [ ] نہیں — دستخط کی تصدیق **نافذ نہیں ہے** یا الگورتھم سوئچنگ (RS256 سے HS256) کے ذریعے اسے بائی پاس **کیا جا سکتا ہے**  

### کیا بیک اینڈ کے ذریعے معیاری کلیمز (exp, nbf, iat) کو سختی سے نافذ کیا جاتا ہے؟
- [ ] جی ہاں — `exp` (Expiration) موجود ہے اور اسے بائی پاس **نہیں کیا جا سکتا**  
- [ ] جی ہاں — `exp` موجود ہے لیکن سرور توثیق (Validation) کے دوران اسے **نافذ نہیں کرتا**  
- [ ] نہیں — میعاد ختم ہونے کے کلیمز (Expiration Claims) **مفقود** ہیں یا انہیں نظر انداز کیا گیا ہے، جس سے غیر معینہ مدت کے لیے سیشن ری پلے (Session Replay) کی اجازت ملتی ہے  

### کیا نفاذ ہیڈر پر مبنی کلیدی انجیکشن (Key Injection - kid, jku, x5u) کو روکتا ہے؟
- [ ] جی ہاں — ہیڈرز کو صاف (Sanitize) کیا گیا ہے اور صرف قابل اعتماد اندرونی کلیدوں/ذرائع کی **اجازت ہے**  
- [ ] نہیں — `kid` (Key ID) ہیڈر ڈائرکٹری ٹراورسل (Directory Traversal) یا SQL انجیکشن (SQL Injection) کے لیے حساس ہے تاکہ کسی معروف فائل کی طرف اشارہ کیا جا سکے  
- [ ] نہیں — `jku` یا `x5u` ہیڈرز کو حملہ آور کے زیر کنٹرول URLs کی طرف موڑا **جا سکتا ہے** تاکہ بدنیتی پر مبنی کلیدیں لوڈ کی جا سکیں  

---