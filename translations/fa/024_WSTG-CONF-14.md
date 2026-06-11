## WSTG-CONF-14 — تست پیکربندی‌های نادرست سایر هدرهای امنیتی HTTP

تست پیکربندی‌های نادرست هدرهای امنیتی اچ‌تی‌تی‌پی (HTTP security header misconfigurations) شامل بررسی وجود و پیاده‌سازی صحیح هدرهای دفاعی است که محافظت‌های اساسی را در برابر بردارهای حمله (Attack Vectors) رایج مبتنی بر وب فراهم می‌کنند. اگرچه این هدرها آسیب‌پذیری‌های زیربنایی کد را برطرف نمی‌کنند، اما عدم وجود آن‌ها خطر حملات مرد میانی (Man-in-the-middle - MITM)، سوءاستفاده از تشخیص نوع فایل (MIME-sniffing) و نشت ناخواسته داده‌های بین‌منشائی (Cross-origin data leakage) از طریق ارجاع‌دهنده‌ها (Referrers) را به میزان قابل توجهی افزایش می‌دهد. از دیدگاه یک مهاجم، فقدان هدرهای امنیتی نشانگرهای بارزی از یک محیط مقاوم‌سازی نشده (Hardened) هستند که اغلب زنجیره‌های اکسپلویت (Exploit) پیچیده‌تری مانند ربودن نشست (Session Hijacking) یا استخراج اطلاعات (Information Exfiltration) را تسهیل می‌کنند. این پیکربندی‌ها معمولاً در سطح سرور ارزیابی می‌شوند و باید به طور مداوم برای همه انواع پاسخ‌ها، از جمله نقاط پایانی (API endpoints) و منابع ایستا (Static resources)، اعمال گردند.


| فیلد | مقدار |
|---|---|
| **شناسه WSTG** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **وضعیت تست** | انجام نشده |
| **شدت** | پایین / متوسط |

**منابع:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ابزارها:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### ممیزی حضور هدرهای امنیتی
| هدر | موجود | مفقود | پیکربندی پیشنهادی |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` or `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Feature-specific, e.g., `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### هدرهای امنیتی چگونه در محدوده اپلیکیشن اعمال می‌شوند؟
- [ ] بله — هدرها به صورت سراسری از طریق یک بارگذارنده بار (Load Balancer) یا پروکسی معکوس (Reverse Proxy) مرکزی اعمال شده‌اند و **نمی‌توان** آن‌ها را بازنویسی (Override) کرد.
- [ ] بله — هدرها برای پاسخ‌های مستندات اصلی اعمال شده‌اند اما در پاسخ‌های API یا دارایی‌های ایستا **موجود نیستند**.
- [ ] خیر — هدرها به صورت ناهماهنگ اعمال شده‌اند یا در کل دامنه اپلیکیشن **مفقود** هستند.

### آیا هدر Strict-Transport-Security (HSTS) به درستی پیکربندی شده است؟
- [ ] بله — مقدار `max-age` برای مدت طولانی (مثلاً ۲ سال) تنظیم شده و `includeSubDomains` **فعال** است.
- [ ] بله — مقدار `max-age` تنظیم شده اما `includeSubDomains` **غیرفعال** است که باعث آسیب‌پذیری زیردامنه‎‌ها می‌شود.
- [ ] خیر — هدر HSTS **موجود نیست** یا `max-age` روی ۰ تنظیم شده است که اجازه حملات تقلیل پروتکل (Protocol Downgrade Attacks) را می‌دهد.

### آیا Referrer-Policy از نشت پارامترهای حساس URL جلوگیری می‌کند؟
- [ ] بله — سیاست روی `no-referrer` یا `same-origin` تنظیم شده است *(امن‌ترین حالت)*.
- [ ] بله — سیاست روی `strict-origin-when-cross-origin` تنظیم شده است.
- [ ] خیر — سیاست **تنظیم نشده** یا روی `unsafe-url` قرار دارد که پتانسیل نشت توکن‌ها یا اطلاعات هویتی حساس (PII) را در هدرهای ارجاع‌دهنده فراهم می‌کند.

### آیا هدر X-Content-Type-Options از حملات MIME-sniffing جلوگیری می‌کند؟
- [ ] بله — دستورالعمل `nosniff` **موجود** است و به درستی پیاده‌سازی شده است.
- [ ] خیر — هدر **مفقود** است که پتانسیل اجازه دادن به مرورگر برای اجرای فایل‌های غیرقابل اجرا (مانند تصاویر) به عنوان اسکریپت را فراهم می‌کند.

---