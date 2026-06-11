## WSTG-CONF-12 — सामग्री सुरक्षा नीति (Content Security Policy - CSP)

सामग्री सुरक्षा नीति (Content Security Policy - CSP) एक महत्वपूर्ण रक्षा-इन-डेप्थ (Defense-in-depth) तंत्र है जिसे क्रॉस-साइट स्क्रिप्टिंग (Cross-Site Scripting - XSS), क्लिकजैकिंग (Clickjacking), और डेटा इंजेक्शन हमलों (Data Injection Attacks) जैसे जोखिमों को कम करने के लिए HTTP प्रतिक्रिया हेडर के माध्यम से लागू किया जाता है। कौन से गतिशील संसाधन (Dynamic Resources) लोड होने की अनुमति है, यह परिभाषित करके, एक अच्छी तरह से कॉन्फ़िगर की गई CSP हमलावर की अनधिकृत स्क्रिप्ट (Unauthorized Scripts) चलाने या बाहरी डोमेन (External Domains) पर संवेदनशील डेटा चुराने (Exfiltrate) की क्षमता को प्रतिबंधित करती है। पेनटेस्टर्स (Pentesters) अत्यधिक अनुमति देने वाले निर्देशों, `'unsafe-inline'` जैसे असुरक्षित कीवर्ड के उपयोग, और असुरक्षित CDNs पर निर्भरता के लिए नीति का मूल्यांकन करते हैं जो बाईपास (Bypass) की सुविधा प्रदान कर सकते हैं। एक कमजोर या अनुपस्थित CSP सीधे तौर पर कोई भेद्यता (Vulnerability) पैदा नहीं करती है, लेकिन सुरक्षा की माध्यमिक परत प्रदान करने में विफल रहकर सफल इंजेक्शन हमलों के प्रभाव को काफी बढ़ा देती है।


| क्षेत्र | मान |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **परीक्षण स्थिति** | निष्पादित नहीं किया गया |
| **गंभीरता** | कम / मध्यम |

**संदर्भ:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**उपकरण:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### क्या एप्लिकेशन की प्रतिक्रियाओं में सामग्री सुरक्षा नीति (CSP) हेडर मौजूद है?
- [ ] हाँ — `Content-Security-Policy` हेडर **मौजूद** है और **लागू (Enforced)** है  
- [ ] हाँ — परीक्षण के लिए `Content-Security-Policy-Report-Only` **मौजूद** है  
- [ ] नहीं — प्रतिक्रियाओं में कोई CSP हेडर **मौजूद नहीं** है  

### क्या `script-src` या `default-src` निर्देश उचित रूप से प्रतिबंधित हैं?
- [ ] हाँ — निर्देश सख्त व्हाइटलिस्टिंग (Whitelisting), नॉनसेस (Nonces), या हैश (Hashes) का उपयोग करते हैं और बाईपास **संभव नहीं** है  
- [ ] हाँ — निर्देश **उचित रूप से कॉन्फ़िगर** किए गए हैं लेकिन व्हाइटलिस्ट में ज्ञात बाईपास योग्य CDNs शामिल हैं  
- [ ] नहीं — निर्देश वाइल्डकार्ड (`*`) या `data:` स्कीम्स का उपयोग करते हैं, जिससे बाईपास **संभव** हो जाता है  

### क्या नीति असुरक्षित इनलाइन स्क्रिप्ट या eval के निष्पादन की अनुमति देती है?
- [ ] नहीं — `'unsafe-inline'` और `'unsafe-eval'` **मौजूद नहीं** हैं  
- [ ] हाँ — `'unsafe-inline'` **मौजूद** है लेकिन एक `nonce` या `hash` द्वारा सुरक्षित है  
- [ ] हाँ — `'unsafe-inline'` या `'unsafe-eval'` बिना किसी अतिरिक्त सुरक्षा के **सक्षम (Enabled)** हैं *(Critical)*  

### क्या एप्लिकेशन `frame-ancestors` निर्देश के माध्यम से क्लिकजैकिंग से सुरक्षित है?
- [ ] हाँ — `frame-ancestors` को `'none'` या `'self'` पर सेट किया गया है  
- [ ] हाँ — `frame-ancestors` **सक्षम** है लेकिन विशिष्ट विश्वसनीय तृतीय-पक्ष डोमेन की अनुमति देता है  
- [ ] नहीं — `frame-ancestors` **अनुपस्थित** है, जो पुराने `X-Frame-Options` या बिना किसी सुरक्षा पर निर्भर है  

### क्या CSP उल्लंघनों की रिपोर्ट करने के लिए कोई तंत्र है?
- [ ] हाँ — `report-uri` या `report-to` **कॉन्फ़िगर** और सक्रिय है  
- [ ] नहीं — उल्लंघन रिपोर्टिंग **अक्षम** है या **कॉन्फ़िगर नहीं** की गई है  

---