## WSTG-CONF-14 — अन्य HTTP सिक्योरिटी हेडर मिसकॉन्फ़िगरेशन (HTTP Security Header Misconfigurations) का परीक्षण करना

HTTP सिक्योरिटी हेडर मिसकॉन्फ़िगरेशन के परीक्षण में उन सुरक्षात्मक हेडर्स (Defensive Headers) की उपस्थिति और सही कार्यान्वयन (Implementation) का सत्यापन शामिल है जो सामान्य वेब-आधारित हमले के वेक्टरों (Attack Vectors) के खिलाफ आवश्यक सुरक्षा प्रदान करते हैं। हालाँकि ये हेडर अंतर्निहित कोड कमजोरियों को ठीक नहीं करते हैं, लेकिन इनकी अनुपस्थिति मैन-इन-द-मिडल (MITM) हमलों, MIME-स्निफिंग (MIME-sniffing) शोषण, और रेफरर्स (Referrers) के माध्यम से अनपेक्षित क्रॉस-ओरिजिन डेटा लीकेज (Cross-Origin Data Leakage) के जोखिम को महत्वपूर्ण रूप से बढ़ा देती है। एक हमलावर के दृष्टिकोण से, गायब सुरक्षा हेडर खराब तरीके से सुरक्षित (Hardened) वातावरण के उच्च-दृश्यता संकेतक हैं, जो अक्सर सेशन हाईजैकिंग (Session Hijacking) या सूचना निष्कर्षण (Information Exfiltration) जैसी अधिक जटिल शोषण श्रृंखलाओं की सुविधा प्रदान करते हैं। इन कॉन्फ़िगरेशन का मूल्यांकन आमतौर पर सर्वर स्तर (Server level) पर किया जाता है और इन्हें API एंडपॉइंट्स (API Endpoints) और स्टेटिक रिसोर्सेज (Static Resources) सहित सभी प्रकार के रिस्पॉन्स पर लगातार लागू किया जाना चाहिए।


| फ़ील्ड | मान |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **परीक्षण की स्थिति** | निष्पादित नहीं किया गया |
| **गंभीरता** | कम / मध्यम |

**संदर्भ:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**उपकरण (Tools):** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### सुरक्षा हेडर उपस्थिति ऑडिट (Security Header Presence Audit)
| हेडर | उपस्थित | अनुपस्थित | अनुशंसित कॉन्फ़िगरेशन |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` or `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Feature-specific, e.g., `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### एप्लिकेशन स्कोप में सुरक्षा हेडर कैसे लागू किए जाते हैं?
- [ ] हाँ — हेडर विश्व स्तर पर एक केंद्रीय लोड बैलेंसर (Load Balancer) या रिवर्स प्रॉक्सी (Reverse Proxy) के माध्यम से लागू किए जाते हैं और उन्हें ओवरराइड **नहीं** किया जा सकता है  
- [ ] हाँ — हेडर मुख्य दस्तावेज़ प्रतिक्रियाओं पर लागू होते हैं लेकिन API प्रतिक्रियाओं या स्टेटिक संपत्तियों पर **गायब** हैं  
- [ ] नहीं — हेडर पूरे एप्लिकेशन डोमेन में असंगत रूप से लागू होते हैं या **गायब** हैं  

### क्या स्ट्रिक्ट-ट्रांसपोर्ट-सिक्योरिटी (HSTS) हेडर सही ढंग से कॉन्फ़िगर किया गया है?
- [ ] हाँ — `max-age` लंबी अवधि (जैसे, 2 वर्ष) के लिए सेट है और `includeSubDomains` **सक्षम** है  
- [ ] हाँ — `max-age` सेट है लेकिन `includeSubDomains` **अक्षम** है, जिससे सबडोमेन असुरक्षित रह जाते हैं  
- [ ] नहीं — HSTS हेडर **उपस्थित नहीं** है या `max-age` 0 पर सेट है, जिससे प्रोटोकॉल डाउनग्रेड हमलों (Protocol Downgrade Attacks) की अनुमति मिलती है  

### क्या रेफरर-पॉलिसी (Referrer-Policy) संवेदनशील URL मापदंडों के लीकेज को रोकती है?
- [ ] हाँ — पॉलिसी `no-referrer` या `same-origin` पर सेट है *(सबसे सुरक्षित)*  
- [ ] हाँ — पॉलिसी `strict-origin-when-cross-origin` पर सेट है  
- [ ] नहीं — पॉलिसी **सेट नहीं** है या `unsafe-url` पर सेट है, जिससे रेफरर हेडर में संभावित रूप से टोकन या संवेदनशील व्यक्तिगत पहचान योग्य जानकारी (PII) लीक हो सकती है  

### क्या X-Content-Type-Options हेडर MIME-स्निफिंग को रोकता है?
- [ ] हाँ — `nosniff` निर्देश **उपस्थित** है और सही ढंग से लागू किया गया है  
- [ ] नहीं — हेडर **गायब** है, जिससे संभावित रूप से ब्राउज़र को गैर-निष्पादन योग्य फ़ाइलों (जैसे, चित्र) को स्क्रिप्ट के रूप में निष्पादित करने की अनुमति मिल सकती है  

---