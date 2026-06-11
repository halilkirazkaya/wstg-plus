## WSTG-CLNT-08 — کراس سائٹ فلیشنگ کی جانچ کرنا

کراس سائٹ فلیشنگ (Cross-Site Flashing - XSF) ایک کلائنٹ سائیڈ کمزوری (Client-side vulnerability) ہے جو اس وقت پیدا ہوتی ہے جب ایک فلیش (SWF) فائل صارف کے زیرِ اثر ان پٹ کو غلط طریقے سے ہینڈل کرتی ہے، جس سے حملہ آور کو نقصان دہ ایکشن اسکرپٹ (ActionScript) کوڈ چلانے یا براؤزر کے جاوا اسکرپٹ (JavaScript) ماحول میں داخل ہونے کی اجازت ملتی ہے۔ `FlashVars` جیسے پیرامیٹرز یا URL کیوری سٹرنگز (query strings)، جو `ExternalInterface.call` ،`getURL` یا `loadMovie` جیسے سنکس (sinks) کو بھیجی جاتی ہیں، ان میں رد و بدل کر کے حملہ آور کمزور ڈومین کے سیاق و سباق میں اقدامات کر سکتا ہے، بشمول سیشن ہائی جیکنگ (Session hijacking) اور ڈیٹا ایکس فلٹریشن (Data exfiltration)۔ یہ کمزوری بنیادی طور پر پرانی (Legacy) انٹرپرائز ایپلی کیشنز میں پائی جاتی ہے جو اب بھی SWF فائلیں ہوسٹ کرتی ہیں، جہاں ناکافی ان پٹ ویلیڈیشن (Input validation) فلیش مووی کو کراس سائٹ اسکرپٹنگ (Cross-Site Scripting - XSS) کے ایک ذریعے کے طور پر استعمال کرنے یا ضرورت سے زیادہ اجازت دینے والی کراس ڈومین کنفیگریشنز کے ذریعے سیم اوریجن پالیسی (Same-Origin Policy - SOP) کو بائی پاس (Bypass) کرنے کی اجازت دیتی ہے۔


| فیلڈ | ویلیو |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **ٹیسٹ کی حیثیت** | انجام نہیں دیا گیا (Not Performed) |
| **سنگینی** | درمیانی / زیادہ |

**حوالہ جات:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**اوزار (Tools):** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### کیا ایپلی کیشن پر پرانی ایڈوبی فلیش (.swf) فائلیں ہوسٹ کی گئی ہیں؟
- [ ] نہیں — ایپلی کیشن کے دائرہ کار (scope) میں کوئی فلیش فائل ہوسٹ یا ریفر نہیں کی گئی ہے  
- [ ] ہاں — SWF فائلیں موجود ہیں لیکن صرف جامد مواد (static content) فراہم کرتی ہیں  
- [ ] ہاں — SWF فائلیں موجود ہیں اور `FlashVars` یا URL سٹرنگز کے ذریعے متحرک پیرامیٹرز (dynamic parameters) قبول کرتی ہیں  

### کیا ایکشن اسکرپٹ (ActionScript) سنکس کو بھیجا گیا ان پٹ صاف (sanitized) کیا گیا ہے؟
- [ ] ہاں — تمام ان پٹس کی سختی سے توثیق (validation) کی گئی ہے اور ایکشن اسکرپٹ سنکس میں رد و بدل **نہیں** کیا جا سکتا  
- [ ] ہاں — توثیق لاگو ہے لیکن مخصوص انکوڈنگ تکنیکوں کے ذریعے اسے بائی پاس کرنا **ممکن ہے**  
- [ ] نہیں — ان پٹ براہ راست حساس سنکس جیسے `ExternalInterface.call` یا `getURL` کو بھیجا جاتا ہے *(انتہائی نازک - Critical)*  

### کیا فلیش فائل کو من مانی جاوا اسکرپٹ (XSS) چلانے کے لیے استعمال کیا جا سکتا ہے؟
- [ ] نہیں — `ExternalInterface` **غیر فعال** ہے یا `allowScriptAccess` پیرامیٹر کو `never` پر سیٹ کیا گیا ہے  
- [ ] ہاں — `allowScriptAccess` کو `sameDomain` پر سیٹ کیا گیا ہے لیکن SWF ٹارگٹ ڈومین پر ہوسٹ کی گئی ہے  
- [ ] ہاں — `allowScriptAccess` کو `always` پر سیٹ کیا گیا ہے، جو کسی بھی ڈومین سے XSS کی اجازت دیتا ہے  

### کیا `crossdomain.xml` پالیسی غیر مجاز کراس اوریجن (cross-origin) درخواستوں کو روکتی ہے؟
- [ ] ہاں — `crossdomain.xml` محدود ہے اور صرف قابلِ بھروسہ، مخصوص اوریجنز کی اجازت دیتی ہے  
- [ ] نہیں — `crossdomain.xml` فائل **موجود نہیں ہے**  
- [ ] ہاں — پالیسی ضرورت سے زیادہ اجازت دینے والی ہے (مثال کے طور پر، `<allow-access-from domain="*" />`)  

### کیا SWF فائل کو بیرونی، حملہ آور کے زیرِ اثر موویز لوڈ کرنے پر مجبور کیا جا سکتا ہے؟
- [ ] نہیں — ایپلی کیشن کو بیرونی SWF فائلیں لوڈ کرنے پر مجبور **نہیں** کیا جا سکتا  
- [ ] ہاں — `loadMovie` یا `loadMovieNum` فنکشنز غیر تصدیق شدہ بیرونی URLs قبول کرتے ہیں  

---