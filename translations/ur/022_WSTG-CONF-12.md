## WSTG-CONF-12 — کنٹینٹ سیکیورٹی پالیسی (Content Security Policy - CSP)

کنٹینٹ سیکیورٹی پالیسی (Content Security Policy - CSP) ایک اہم "گہرائی میں دفاع" (Defense-in-depth) کا میکانزم ہے جسے HTTP ریسپانس ہیڈرز (HTTP response headers) کے ذریعے لاگو کیا جاتا ہے تاکہ کراس سائٹ اسکرپٹنگ (Cross-Site Scripting - XSS)، کلک جیکنگ (Clickjacking)، اور ڈیٹا انجیکشن (Data injection) جیسے حملوں کے خطرات کو کم کیا جا سکے۔ یہ واضح کر کے کہ کن متحرک وسائل (Dynamic resources) کو لوڈ کرنے کی اجازت ہے، ایک بہتر طور پر ترتیب دی گئی CSP کسی حملہ آور کی غیر مجاز اسکرپٹس چلانے یا حساس ڈیٹا کو بیرونی ڈومینز پر چوری (Exfiltrate) کرنے کی صلاحیت کو محدود کرتی ہے۔ پینٹیسٹرز (Pentesters) پالیسی کا جائزہ لیتے ہیں کہ آیا اس میں ضرورت سے زیادہ کھلی ہدایات (Directives)، غیر محفوظ کلیدی الفاظ جیسے `'unsafe-inline'` کا استعمال، یا غیر محفوظ CDNs پر انحصار تو نہیں کیا گیا جو بائی پاس (Bypass) کی سہولت فراہم کر سکتے ہیں۔ ایک کمزور یا غیر موجود CSP براہ راست کوئی کمزوری (Vulnerability) پیدا نہیں کرتی لیکن یہ تحفظ کی ثانوی تہہ فراہم کرنے میں ناکامی کے ذریعے کامیاب انجیکشن حملوں کے اثرات میں نمایاں اضافہ کر دیتی ہے۔


| فیلڈ | قدر |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **ٹیسٹ کی صورتحال** | انجام نہیں دیا گیا (Not Performed) |
| **شدت** | کم / درمیانی (Low / Medium) |

**حوالہ جات:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**ٹولز:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### کیا ایپلی کیشن کے ریسپانسز میں Content Security Policy (CSP) ہیڈر موجود ہے؟
- [ ] ہاں — `Content-Security-Policy` ہیڈر **موجود** ہے اور **نافذ** ہے  
- [ ] ہاں — `Content-Security-Policy-Report-Only` ٹیسٹنگ کے لیے **موجود** ہے  
- [ ] نہیں — ریسپانسز میں کوئی CSP ہیڈر **موجود نہیں** ہے  

### کیا `script-src` یا `default-src` ہدایات (Directives) مناسب طور پر محدود ہیں؟
- [ ] ہاں — ہدایات میں سخت وائٹ لسٹنگ، nonces، یا hashes استعمال کیے گئے ہیں اور بائی پاس **ممکن نہیں** ہے  
- [ ] ہاں — ہدایات **درست طریقے سے ترتیب دی گئی ہیں** لیکن وائٹ لسٹ میں ایسے CDNs شامل ہیں جنہیں بائی پاس کیا جا سکتا ہے  
- [ ] نہیں — ہدایات میں وائلڈ کارڈز (`*`) یا `data:` اسکیمز استعمال کی گئی ہیں، جس سے بائی پاس **ممکن** ہے  

### کیا پالیسی غیر محفوظ ان لائن اسکرپٹس (Inline scripts) یا eval کو چلانے کی اجازت دیتی ہے؟
- [ ] نہیں — `'unsafe-inline'` اور `'unsafe-eval'` **موجود نہیں** ہیں  
- [ ] ہاں — `'unsafe-inline'` **موجود** ہے لیکن `nonce` یا `hash` کے ذریعے محفوظ ہے  
- [ ] ہاں — `'unsafe-inline'` یا `'unsafe-eval'` بغیر کسی اضافی تحفظ کے **فعال** ہیں *(Critical)*  

### کیا ایپلی کیشن `frame-ancestors` ہدایت کے ذریعے کلک جیکنگ سے محفوظ ہے؟
- [ ] ہاں — `frame-ancestors` کو `'none'` یا `'self'` پر سیٹ کیا گیا ہے  
- [ ] ہاں — `frame-ancestors` **فعال** ہے لیکن مخصوص قابل اعتماد تھرڈ پارٹی ڈومینز کی اجازت دیتا ہے  
- [ ] نہیں — `frame-ancestors` **مفقود** ہے، اور یہ پرانے `X-Frame-Options` یا کسی بھی تحفظ کے بغیر کام کر رہا ہے  

### کیا CSP کی خلاف ورزیوں کی رپورٹنگ کا کوئی میکانزم موجود ہے؟
- [ ] ہاں — `report-uri` یا `report-to` **ترتیب دیا گیا** اور فعال ہے  
- [ ] نہیں — خلاف ورزی کی رپورٹنگ **غیر فعال** ہے یا **ترتیب نہیں دی گئی** ہے  

---