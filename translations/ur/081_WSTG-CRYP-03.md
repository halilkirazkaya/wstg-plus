## WSTG-CRYP-03 — غیر انکرپٹڈ چینلز کے ذریعے بھیجی گئی حساس معلومات کی جانچ (Testing for Sensitive Information Sent Via Unencrypted Channels)

حساس ڈیٹا (Sensitive Data) کو غیر انکرپٹڈ چینلز (Unencrypted Channels)، جیسے کہ سادہ HTTP، کے ذریعے منتقل کرنا معلومات کو مین ان دی مڈل (MITM) حملوں کے ذریعے روکے جانے اور ان میں تبدیلی کیے جانے کے خطرے سے دوچار کرتا ہے۔ یہ کمزوری (Vulnerability) عام طور پر تب پیدا ہوتی ہے جب ویب ایپلی کیشنز تمام اینڈ پوائنٹس (Endpoints) پر ٹرانسپورٹ لیئر سیکیورٹی (TLS) کو نافذ کرنے میں ناکام رہتی ہیں، خاص طور پر پرانے اجزاء (Legacy Components)، لاگ ان فارمز، یا HTTP سے HTTPS پر منتقلی کے ابتدائی مرحلے کے دوران۔ ایک حملہ آور کے نقطہ نظر سے، یہ عوامی وائی فائی جیسے غیر محفوظ نیٹ ورک سیگمنٹس پر تصدیق کی اسناد (Authentication Credentials)، سیشن ٹوکنز (Session Tokens)، اور ذاتی طور پر قابل شناخت معلومات (PII) کی غیر فعال سنفنگ (Sniffing) کی اجازت دیتا ہے۔ اس بات کو یقینی بنانا کہ تمام حساس ڈیٹا ایک مضبوط TLS ٹنل کے اندر محفوظ ہے، صارفین کے تعاملات کی رازداری اور سالمیت (Confidentiality and Integrity) کو برقرار رکھنے اور سیشن ہائی جیکنگ (Session Hijacking) کو روکنے کے لیے بنیادی اہمیت رکھتا ہے۔

| فیلڈ | ویلیو |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **ٹیسٹ کی صورتحال** | انجام نہیں دیا گیا |
| **شدت (Severity)** | اعلیٰ / درمیانی |

> *شدت (Severity) اس صورت میں 'اعلیٰ' (High) ہو جاتی ہے اگر تصدیق کی اسناد (Authentication Credentials)، سیشن آئیڈنٹیفائرز (Session Identifiers)، یا ذاتی طور پر قابل شناخت معلومات (PII) کلیئر ٹیکسٹ (Cleartext) میں منتقل کی جائیں۔

**حوالہ جات:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ٹولز (Tools):** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### کیا ایپلی کیشن حساس اینڈ پوائنٹس کے لیے غیر انکرپٹڈ HTTP پر مواصلت کی اجازت دیتی ہے؟
- [ ] نہیں — تمام ایپلی کیشن ٹریفک کو سختی سے HTTPS پر مجبور کیا جاتا ہے *(انتہائی محفوظ)*  
- [ ] ہاں — غیر حساس صفحات HTTP پر دستیاب ہیں لیکن حساس اینڈ پوائنٹس **نہیں ہیں**  
- [ ] ہاں — حساس اینڈ پوائنٹس (Login, Checkout, Profile) غیر انکرپٹڈ HTTP پر **دستیاب ہیں** *(اعلیٰ خطرہ)*  

### کیا HTTP سے HTTPS پر عالمی ری ڈائریکشن (Global Redirection) موجود ہے؟
- [ ] ہاں — ایپلی کیشن تمام HTTP درخواستوں کے لیے HTTPS پر 301/302 ری ڈائریکٹ کرتی ہے  
- [ ] ہاں — ری ڈائریکشن صرف مخصوص حساس اینڈ پوائنٹس کے لیے کی جاتی ہے  
- [ ] نہیں — HTTP سے HTTPS پر کوئی ری ڈائریکشن **لاگو نہیں ہے**  

### کیا ایچ ٹی ٹی پی اسٹرکٹ ٹرانسپورٹ سیکیورٹی (HSTS) لاگو ہے؟
- [ ] ہاں — HSTS ایک طویل `max-age` کے ساتھ **فعال** ہے اور اس میں `includeSubDomains` اور `preload` ہدایات شامل ہیں  
- [ ] ہاں — HSTS **فعال** ہے لیکن اس میں `includeSubDomains` یا `preload` ہدایات موجود نہیں ہیں  
- [ ] نہیں — ایپلی کیشن سرور پر HSTS **فعال نہیں ہے**  

### کیا حساس کوکیز (Cookies) کلیئر ٹیکسٹ ٹرانسمیشن سے محفوظ ہیں؟

| کوکی کا نام | سیکیور فلیگ (Secure Flag) موجود ہے | سیکیور فلیگ غائب ہے |
|-------------|:-------------------:|:------------------:|
| `SessionID` |                     |                    |
| `AuthToken` |                     |                    |
| `CSRF-Token`|                     |                    |

### کیا ایپلی کیشن غیر انکرپٹڈ چینلز کے ذریعے تھرڈ پارٹی APIs کو حساس ڈیٹا منتقل کرتی ہے؟
- [ ] نہیں — تمام بیرونی API کالز انکرپٹڈ (HTTPS) ٹنلز کے ذریعے کی جاتی ہیں  
- [ ] ہاں — کچھ غیر حساس ٹیلی میٹری (Telemetry) ڈیٹا HTTP کے ذریعے بھیجا جاتا ہے  
- [ ] ہاں — API Keys، PII، یا اسناد (Credentials) غیر انکرپٹڈ HTTP کے ذریعے تھرڈ پارٹیز کو **بھیجی جاتی ہیں**  

---