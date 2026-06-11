## WSTG-CONF-06 — اختبار طرق HTTP (HTTP Methods)

يتضمن اختبار طرق HTTP (HTTP Methods) تحديد الأفعال (Verbs) التي يدعمها خادم الويب والتطبيق لضمان كشف الوظائف الضرورية فقط. بالإضافة إلى الطرق القياسية مثل `GET` و `POST`، قد تقوم الخوادم عن غير قصد بتمكين طرق خطيرة مثل `PUT` أو `DELETE` أو `TRACE`؛ مما قد يسمح للمهاجمين برفع ملفات ضارة، أو حذف محتوى موجود، أو تسريب ملفات تعريف ارتباط الجلسة (Session Cookies) عبر هجوم تتبع المواقع المشترك (Cross-Site Tracing - XST). يقوم مختبرو الاختراق بفحص ترويسات الاستجابة (Response Headers) مثل `Allow` أو `Public` ومحاولة تجاوز القيود باستخدام تقنيات تجاوز الطرق (Method Overriding) أو التلاعب بالأفعال (Verb Tampering) للوصول إلى وظائف غير مصرح بها. يعد هذا الاختبار أمراً حيوياً لتحصين سطح الهجوم (Attack Surface) ومنع أخطاء الإعداد (Misconfigurations) التي قد تؤدي إلى اختراق الخادم أو التلاعب غير المصرح به بالبيانات.


| الحقل | القيمة |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **حالة الاختبار** | لم يتم التنفيذ |
| **الخطورة** | منخفضة / متوسطة* |

> *تصبح الخطورة عالية (High) إذا كانت `PUT` أو `DELETE` تسمح بالتلاعب غير المصرح به بالملفات، أو إذا كانت `TRACE` تسهل تسريب رموز الجلسة (Session Tokens).

**المراجع:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**الأدوات:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### هل تم تعطيل طرق HTTP الخطيرة مثل `PUT` أو `DELETE` في كافة أرجاء التطبيق؟
- [ ] نعم — الطرق الآمنة فقط (GET, POST, HEAD) **هي الممكنة**  
- [ ] لا — طرق `PUT` أو `DELETE` **ممكنة** ولكنها تتطلب مصادقة (Authentication) صالحة  
- [ ] لا — طرق `PUT` أو `DELETE` **ممكنة** ويمكن الوصول إليها بدون مصادقة *(حرجة - Critical)*  

### هل تم تعطيل طريقة `TRACE` لمنع تتبع المواقع المشترك (XST)؟
- [ ] نعم — طريقة `TRACE` **معطلة** أو تعيد كود 405 Method Not Allowed  
- [ ] لا — طريقة `TRACE` **ممكنة** ولكنها لا تعكس الترويسات الحساسة  
- [ ] لا — طريقة `TRACE` **ممكنة** وتعكس ترويسات `Cookie` أو `Authorization` *(متوسطة)*  

### هل يدعم الخادم تجاوز طرق HTTP (مثل `X-HTTP-Method-Override`)؟
- [ ] لا — الخادم **لا** يعالج ترويسات تجاوز الطرق  
- [ ] نعم — الخادم يعالج ترويسات التجاوز ولكن يتم **فرض ضوابط الوصول**  
- [ ] نعم — الخادم يعالج ترويسات التجاوز و**من الممكن** تجاوز ضوابط الوصول (Access Control Bypass)  

### هل يستجيب الخادم بشكل آمن لطرق HTTP العشوائية أو المشوهة؟
- [ ] نعم — يعيد الخادم كود 405 Method Not Allowed أو 501 Not Implemented  
- [ ] لا — يعيد الخادم كود 200 OK أو 500 Error للطرق العشوائية مثل `JEFF` أو `TEST`  
- [ ] لا — استخدام طرق عشوائية يسمح بتجاوز فلاتر المصادقة أو التفويض  

### هل تم تقييد طريقة `OPTIONS` أو تكوينها لتقليل كشف المعلومات (Information Disclosure)؟
- [ ] نعم — طريقة `OPTIONS` معطلة أو تعيد ترويسة `Allow` محدودة  
- [ ] لا — تكشف طريقة `OPTIONS` عن نطاق واسع من الطرق الممكنة، بما في ذلك DAV أو أفعال التصحيح (Debugging Verbs)  

---