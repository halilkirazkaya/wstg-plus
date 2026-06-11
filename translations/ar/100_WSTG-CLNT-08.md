## WSTG-CLNT-08 — اختبار الوميض عبر المواقع (Cross-Site Flashing)

يُعد الوميض عبر المواقع (Cross-Site Flashing - XSF) ثغرة أمنية في جانب العميل (client-side) تحدث عندما يقوم ملف Flash (SWF) بمعالجة المدخلات التي يتحكم فيها المستخدم بشكل غير صحيح، مما يسمح للمهاجم بتنفيذ كود أكشن سكربت (ActionScript) خبيث أو الانتقال إلى بيئة جافا سكربت (JavaScript) الخاصة بالمتصفح. من خلال التلاعب بالمعاملات مثل `FlashVars` أو سلاسل استعلام URL الممررة إلى أحواض البيانات (sinks) مثل `ExternalInterface.call` أو `getURL` أو `loadMovie` ، يمكن للمهاجم تنفيذ إجراءات في سياق النطاق المعرض للخطر، بما في ذلك اختطاف الجلسة (Session Hijacking) وتسريب البيانات (Data Exfiltration). توجد هذه الثغرة بشكل أساسي في التطبيقات المؤسسية القديمة التي لا تزال تستضيف ملفات SWF، حيث تسمح عمليات التحقق غير الكافية من المدخلات بإعادة استخدام ملف Flash كمتجه لهجمات برمجة المواقع عبر المواقع (Cross-Site Scripting - XSS) أو لتجاوز سياسة المنشأ نفسه (Same-Origin Policy - SOP) عبر إعدادات النطاقات المتعددة (cross-domain) المتساهلة.


| الحقل | القيمة |
|---|---|
| **معرف WSTG** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **حالة الاختبار** | لم يتم التنفيذ |
| **الخطورة** | متوسطة / عالية |

**المراجع:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**الأدوات:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### هل يتم استضافة ملفات Adobe Flash (.swf) قديمة على التطبيق؟
- [ ] لا — لا يتم استضافة أو الإشارة إلى ملفات Flash ضمن نطاق التطبيق.  
- [ ] نعم — ملفات SWF موجودة ولكنها تقدم محتوى ثابتًا فقط.  
- [ ] نعم — ملفات SWF موجودة وتقبل معاملات ديناميكية عبر `FlashVars` أو سلاسل URL.  

### هل يتم تنقية المدخلات الممررة إلى أحواض ActionScript؟
- [ ] نعم — يتم التحقق من جميع المدخلات بصرامة ولا يمكن التلاعب بأحواض ActionScript.  
- [ ] نعم — يتم تطبيق التحقق ولكن التجاوز **ممكن** عبر تقنيات ترميز محددة.  
- [ ] لا — يتم تمرير المدخلات مباشرة إلى أحواض حساسة مثل `ExternalInterface.call` أو `getURL` *(حرج)*.  

### هل يمكن استخدام ملف Flash لتنفيذ JavaScript تعسفي (XSS)؟
- [ ] لا — تم تعطيل `ExternalInterface` أو تم تعيين معامل `allowScriptAccess` إلى `never`.  
- [ ] نعم — تم تعيين `allowScriptAccess` إلى `sameDomain` ولكن يتم استضافة ملف SWF على النطاق المستهدف.  
- [ ] نعم — تم تعيين `allowScriptAccess` إلى `always` ، مما يسمح بـ XSS من أي نطاق.  

### هل تمنع سياسة `crossdomain.xml` الطلبات غير المصرح بها عبر الأصول (cross-origin)؟
- [ ] نعم — ملف `crossdomain.xml` تقييدي ويسمح فقط بأصول موثوقة ومحددة.  
- [ ] لا — ملف `crossdomain.xml` **مفقود**.  
- [ ] نعم — السياسة متساهلة للغاية (على سبيل المثال، `<allow-access-from domain="*" />`).  

### هل يمكن إجبار ملف SWF على تحميل ملفات خارجية يتحكم فيها المهاجم؟
- [ ] لا — لا يمكن إجبار التطبيق على تحميل ملفات SWF خارجية.  
- [ ] نعم — تقبل دوال `loadMovie` أو `loadMovieNum` عناوين URL خارجية غير خاضعة للتحقق.  

---