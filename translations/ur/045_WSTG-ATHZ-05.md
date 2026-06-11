## WSTG-ATHZ-05 — OAuth کی کمزوریوں کی جانچ (Testing for OAuth Weaknesses)

OAuth کی کمزوریاں OAuth 2.0 یا OpenID Connect (OIDC) پروٹوکولز کے نفاذ (implementation) میں مختلف نقائص پر مشتمل ہوتی ہیں، جن کا نتیجہ اکثر مکمل اکاؤنٹ ٹیک اوور (Account Takeover) یا غیر مجاز ڈیٹا تک رسائی (Unauthorized Data Access) کی صورت میں نکلتا ہے۔ یہ کمزوریاں عام طور پر آتھورائزیشن فلو (Authorization Flow) کے دوران ظاہر ہوتی ہیں، خاص طور پر `redirect_uri` کی توثیق (validation)، `state` پیرامیٹر (parameter) کی اینٹروپی (entropy) اور تصدیق، اور ایکسیس یا آئی ڈی ٹوکنز (Access/ID Tokens) کی محفوظ ہینڈلنگ کے حوالے سے۔ حملہ آور آتھورائزیشن کوڈز (Authorization Codes) کو روک کر، اوپن ری ڈائریکٹس (Open Redirects) کے ذریعے حملہ آور کے زیرِ اثر ڈومینز پر ٹوکنز بھیج کر، یا کراس سائٹ ریکوسٹ فارجری (CSRF) کے ذریعے متاثرہ صارف کے سیشن کو حملہ آور کے اکاؤنٹ سے منسلک کر کے ان کا فائدہ اٹھاتے ہیں۔ چونکہ OAuth اکثر بنیادی آتھنٹیکیشن میکانزم (Authentication Mechanism) کے طور پر کام کرتا ہے، اس لیے ایک بھی غلط کنفیگریشن (Misconfiguration) پورے صارف شناختی نظام (identity system) کی سالمیت کو خطرے میں ڈال سکتی ہے۔


| فیلڈ (Field) | قدر (Value) |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **ٹیسٹ کی صورتحال (Test Status)** | انجام نہیں دیا گیا (Not Performed) |
| **سنگینی (Severity)** | ہائی / کریٹیکل (High / Critical) |

**حوالہ جات (References):**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**اوزار (Tools):** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### کیا CSRF سے بچنے کے لیے `state` پیرامیٹر نافذ اور تصدیق شدہ ہے؟
- [ ] جی ہاں — `state` لازمی ہے، ہر سیشن کے لیے منفرد ہے، اور سرور پر سختی سے اس کی توثیق کی جاتی ہے  
- [ ] جی ہاں — `state` موجود ہے لیکن مختلف سیشنز میں ساکن (static) یا قابلِ پیشن گوئی (predictable) ہے  
- [ ] جی ہاں — `state` موجود ہے لیکن ایپلی کیشن کال بیک (callback) پر اس کی توثیق **نہیں** کرتی  
- [ ] نہیں — آتھورائزیشن ریکوسٹ میں `state` پیرامیٹر استعمال **نہیں** کیا جاتا *(Critical)*  

### کیا `redirect_uri` کی وائٹ لسٹ (whitelist) کے خلاف سختی سے توثیق کی جاتی ہے؟
- [ ] جی ہاں — پہلے سے رجسٹرڈ وائٹ لسٹ کے ساتھ صرف عین مماثلت (exact matches) کو **قبول** کیا جاتا ہے  
- [ ] جی ہاں — توثیق لاگو ہے لیکن پاتھ ٹریورسل (path traversal) یا سب ڈومین مینیپولیشن (subdomain manipulation) کے ذریعے اسے بائی پاس (bypass) کرنا **ممکن ہے**  
- [ ] جی ہاں — توثیق لاگو ہے لیکن پیرامیٹر پولیوشن (parameter pollution) یا فریگمنٹ انجیکشن (fragment injection) کے ذریعے اسے بائی پاس کرنا **ممکن ہے**  
- [ ] نہیں — ایپلی کیشن `redirect_uri` پیرامیٹر میں من مانی (arbitrary) URLs کی **اجازت** دیتی ہے *(Critical)*  

### کیا فلو کو تبدیل کرنے کے لیے `response_type` میں ہیرا پھیری کی جا سکتی ہے؟
- [ ] نہیں — ایپلی کیشن صرف مطلوبہ فلو (مثلاً `code`) کو قبول کرتی ہے اور دوسروں کو مسترد کرتی ہے  
- [ ] جی ہاں — `code` سے `token` (Implicit flow) پر منتقل ہونا **ممکن ہے** اور اس سے URL میں ٹوکنز لیک ہو جاتے ہیں  
- [ ] جی ہاں — ہائبرڈ فلو (hybrid flows) یا غیر مجاز `response_type` ویلیوز **فعال** ہیں اور ان پر کارروائی کی جاتی ہے  

### کیا Referer ہیڈرز یا براؤزر ہسٹری کے ذریعے ایکسیس ٹوکنز یا آتھورائزیشن کوڈز لیک ہو رہے ہیں؟
- [ ] نہیں — ٹوکنز/کوڈز کو محفوظ طریقے سے ہینڈل کیا جاتا ہے اور حساس صفحات مناسب `Referrer-Policy` استعمال کرتے ہیں  
- [ ] جی ہاں — ٹوکنز/کوڈز `Referer` ہیڈر کے ذریعے تھرڈ پارٹی ڈومینز کو لیک **ہو رہے ہیں**  
- [ ] جی ہاں — غیر محفوظ کیشنگ (caching) یا GET ریکوسٹس کی وجہ سے ٹوکنز/کوڈز براؤزر ہسٹری میں **ظاہر ہو رہے ہیں**  

### کیا ایپلی کیشن OAuth اسکوپس (scopes) کے لیے "کم سے کم استحقاق" (Least Privilege) کے اصول کو نافذ کرتی ہے؟
- [ ] جی ہاں — ایپلی کیشن فعالیت کے لیے صرف ضروری کم از کم اسکوپس کی درخواست کرتی ہے  
- [ ] نہیں — ایپلی کیشن ڈیفالٹ طور پر ضرورت سے زیادہ یا انتظامی (administrative) اسکوپس کی درخواست کرتی ہے  
- [ ] نہیں — صارف رضامندی کے مرحلے (consent phase) کے دوران مخصوص اسکوپس کا جائزہ **نہیں** لے سکتا یا ان سے انکار (opt-out) نہیں کر سکتا  

---