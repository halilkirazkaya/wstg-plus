## WSTG-CONF-14 — اختبار سوء تكوين ترويسات أمان HTTP الأخرى (Test Other HTTP Security Header Misconfigurations)

يتضمن اختبار سوء تكوين ترويسات أمان HTTP (HTTP security header misconfigurations) التحقق من وجود وتنفيذ الترويسات الدفاعية بشكل صحيح، والتي توفر حماية أساسية ضد نواقل الهجوم الشائعة المستندة إلى الويب. ورغم أن هذه الترويسات لا تعالج الثغرات الأمنية الكامنة في الكود المصدري، إلا أن غيابها يزيد بشكل كبير من خطر هجمات "رجل في المنتصف" (Man-in-the-middle - MITM)، واستغلال "استنشاق نوع MIME" (MIME-sniffing)، وتسريب البيانات غير المقصود عبر الأصول (Cross-origin data leakage) من خلال المُحيل (Referrer). من وجهة نظر المهاجم، تُعد ترويسات الأمان المفقودة مؤشرات واضحة على بيئة ضعيفة الحماية، مما يسهل غالباً سلاسل استغلال أكثر تعقيداً مثل اختطاف الجلسة (Session Hijacking) أو استخراج المعلومات (Information Exfiltration). يتم تقييم هذه التكوينات عادةً على مستوى الخادم ويجب تطبيقها باستمرار عبر جميع أنواع الاستجابات، بما في ذلك نقاط نهاية واجهة برمجة التطبيقات (API) والموارد الثابتة.


| الحقل | القيمة |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **حالة الاختبار** | لم يتم تنفيذه |
| **الخطورة** | منخفضة / متوسطة |

**المراجع:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**الأدوات:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### تدقيق وجود ترويسات الأمان (Security Header Presence Audit)
| الترويسة (Header) | موجودة | مفقودة | التكوين الموصى به |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` or `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Feature-specific, e.g., `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### كيف يتم فرض ترويسات الأمان عبر نطاق التطبيق؟
- [ ] نعم — يتم تطبيق الترويسات عالمياً عبر موازن حمل مركزي أو وكيل عكسي (Reverse Proxy) ولا **يمكن** تجاوزها  
- [ ] نعم — يتم تطبيق الترويسات على استجابات المستندات الرئيسية ولكنها **مفقودة** في استجابات API أو الموارد الثابتة  
- [ ] لا — يتم تطبيق الترويسات بشكل غير متسق أو أنها **مفقودة** عبر نطاق التطبيق بالكامل  

### هل تم تكوين ترويسة Strict-Transport-Security (HSTS) بشكل صحيح؟
- [ ] نعم — تم ضبط `max-age` لمدة طويلة (مثلاً: سنتان) وتم **تفعيل** `includeSubDomains`  
- [ ] نعم — تم ضبط `max-age` ولكن تم **تعطيل** `includeSubDomains` مما يترك النطاقات الفرعية عرضة للخطر  
- [ ] لا — ترويسة HSTS **غير موجودة** أو تم ضبط `max-age` على 0، مما يسمح بهجمات خفض بروتوكول التشفير (Protocol Downgrade)  

### هل تمنع Referrer-Policy تسريب معاملات URL الحساسة؟
- [ ] نعم — تم ضبط السياسة على `no-referrer` أو `same-origin` *(الأكثر أماناً)*  
- [ ] نعم — تم ضبط السياسة على `strict-origin-when-cross-origin`  
- [ ] لا — السياسة **غير محددة** أو تم ضبطها على `unsafe-url` مما قد يؤدي لتسريب الرموز (Tokens) أو معلومات الهوية الشخصية الحساسة (PII) في ترويسات المحيل (Referer headers)  

### هل تمنع ترويسة X-Content-Type-Options استنشاق نوع MIME؟
- [ ] نعم — تعليمة `nosniff` **موجودة** ومنفذة بشكل صحيح  
- [ ] لا — الترويسة **مفقودة** مما قد يسمح للمتصفح بتنفيذ ملفات غير قابلة للتنفيذ (مثل الصور) كبرمجيات نصية (Scripts)  

---