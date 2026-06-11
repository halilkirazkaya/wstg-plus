## WSTG-CONF-14 — دیگر HTTP سیکیورٹی ہیڈر کی غلط کنفیگریشنز (HTTP Security Header Misconfigurations) کی جانچ کرنا

ایچ ٹی ٹی پی سیکیورٹی ہیڈر کی غلط کنفیگریشنز (HTTP security header misconfigurations) کی جانچ میں ان دفاعی ہیڈرز کی موجودگی اور درست نفاذ کی تصدیق کرنا شامل ہے جو عام ویب پر مبنی حملوں کے خلاف ضروری تحفظ فراہم کرتے ہیں۔ اگرچہ یہ ہیڈرز بنیادی کوڈ کی کمزوریوں کو ٹھیک نہیں کرتے، لیکن ان کی عدم موجودگی سے "درمیان میں موجود حملہ آور" (Man-in-the-middle - MITM) حملوں، مائم سنفنگ (MIME-sniffing) کے استحصال، اور ریفررز (Referrers) کے ذریعے غیر ارادی کراس اوریجن ڈیٹا لیکیج (Cross-origin data leakage) کے خطرات میں نمایاں اضافہ ہو جاتا ہے۔ حملہ آور کے نقطہ نظر سے، غائب سیکیورٹی ہیڈرز ایک کمزور سیکیورٹی والے ماحول کی واضح علامت ہیں، جو اکثر سیشن ہائی جیکنگ (Session hijacking) یا معلومات کے اخراج (Information exfiltration) جیسے پیچیدہ حملوں کی راہ ہموار کرتے ہیں۔ ان کنفیگریشنز کا جائزہ عام طور پر سرور کی سطح پر لیا جاتا ہے اور انہیں API اینڈ پوائنٹس (API endpoints) اور ساکن ذرائع (Static resources) سمیت تمام رسپانسز پر مستقل طور پر لاگو کیا جانا چاہیے۔


| فیلڈ | ویلیو |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **ٹیسٹ کی صورتحال** | انجام نہیں دیا گیا |
| **شدت** | کم / درمیانی |

**حوالہ جات:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ٹولز:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### سیکیورٹی ہیڈر کی موجودگی کا آڈٹ (Security Header Presence Audit)
| ہیڈر | موجود | غائب | تجویز کردہ کنفیگریشن |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` or `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (فیچر کے لحاظ سے، مثلاً `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### ایپلیکیشن کے دائرہ کار میں سیکیورٹی ہیڈرز کو کس طرح نافذ کیا گیا ہے؟
- [ ] جی ہاں — ہیڈرز مرکزی لوڈ بیلنسر (Load balancer) یا ریورس پراکسی (Reverse proxy) کے ذریعے عالمی سطح پر لاگو کیے گئے ہیں اور انہیں اوور رائڈ (Override) نہیں کیا جا سکتا  
- [ ] جی ہاں — ہیڈرز مرکزی دستاویز کے رسپانسز پر لاگو ہیں لیکن API رسپانسز یا ساکن اثاثوں پر غائب ہیں  
- [ ] نہیں — ہیڈرز غیر مستقل طور پر لاگو ہیں یا پوری ایپلیکیشن ڈومین میں غائب ہیں  

### کیا اسٹرکٹ ٹرانسپورٹ سیکیورٹی (Strict-Transport-Security - HSTS) ہیڈر درست طریقے سے کنفیگر ہے؟
- [ ] جی ہاں — `max-age` ایک طویل مدت (مثلاً 2 سال) کے لیے سیٹ ہے اور `includeSubDomains` فعال (Enabled) ہے  
- [ ] جی ہاں — `max-age` سیٹ ہے لیکن `includeSubDomains` غیر فعال (Disabled) ہے، جس سے سب ڈومینز خطرے میں رہ جاتے ہیں  
- [ ] نہیں — HSTS ہیڈر موجود نہیں ہے یا `max-age` کو 0 پر سیٹ کیا گیا ہے، جس سے پروٹوکول ڈاؤن گریڈ (Protocol downgrade) حملوں کی اجازت ملتی ہے  

### کیا ریفرر پالیسی (Referrer-Policy) حساس URL پیرامیٹرز کے اخراج کو روکتی ہے؟
- [ ] جی ہاں — پالیسی کو `no-referrer` یا `same-origin` پر سیٹ کیا گیا ہے (سب سے زیادہ محفوظ)  
- [ ] جی ہاں — پالیسی کو `strict-origin-when-cross-origin` پر سیٹ کیا گیا ہے  
- [ ] نہیں — پالیسی سیٹ نہیں ہے یا اسے `unsafe-url` پر سیٹ کیا گیا ہے، جس سے ریفرر ہیڈرز میں ٹوکنز یا حساس ذاتی شناختی معلومات (PII) کے اخراج کا خدشہ ہے  

### کیا X-Content-Type-Options ہیڈر مائم سنفنگ (MIME-sniffing) کو روک رہا ہے؟
- [ ] جی ہاں — `nosniff` ڈائریکٹو موجود ہے اور درست طریقے سے نافذ ہے  
- [ ] نہیں — ہیڈر غائب ہے، جس سے ممکنہ طور پر براؤزر کو غیر ایگزیکیوٹیبل فائلوں (مثلاً تصاویر) کو اسکرپٹس کے طور پر چلانے کی اجازت مل سکتی ہے  

---