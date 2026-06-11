## WSTG-CONF-05 — इन्फ्रास्ट्रक्चर और एप्लीकेशन एडमिन इंटरफेस की पहचान करना (Enumerate Infrastructure and Application Admin Interfaces)

एडमिनिस्ट्रेटिव इंटरफेस (Administrative interfaces) किसी एप्लीकेशन और उसके अंतर्निहित इन्फ्रास्ट्रक्चर (Infrastructure) के मैनेजमेंट कंट्रोल प्लेन (Management Control Plane) का प्रतिनिधित्व करते हैं, जो अक्सर सिस्टम कॉन्फ़िगरेशन (System Configurations), उपयोगकर्ता डेटा (User Data), और सर्वर-साइड ऑपरेशंस (Server-side Operations) तक विशेषाधिकार प्राप्त एक्सेस (Privileged Access) प्रदान करते हैं। हमलावर डायरेक्टरी ब्रूट-फोर्सिंग (Directory Brute-forcing), `robots.txt` के विश्लेषण, या अनुमानित URL पैटर्न (Predictable URL patterns) को देखकर इन इंटरफेस की पहचान करने को प्राथमिकता देते हैं ताकि वे ऐसे प्रवेश बिंदु (entry points) ढूंढ सकें जिनमें मजबूत प्रमाणीकरण (Authentication) की कमी हो या जो डिफ़ॉल्ट क्रेडेंशियल्स (Default Credentials) पर निर्भर हों। एक्सपोज्ड एडमिन पैनल की खोज से पूरे सिस्टम के साथ समझौता होने, अनधिकृत डेटा संशोधन (Unauthorized data modification), या सेवा में व्यवधान का जोखिम काफी बढ़ जाता है। ये इंटरफेस अक्सर गैर-मानक पोर्ट्स (Non-standard ports) या सबडोमेन (Subdomains) पर पाए जाते हैं, जो उन्हें ऑटोमेटेड स्कैनर्स (Automated Scanners) और मैनुअल रीकॉनेसेंस (Manual Reconnaissance) के लिए मुख्य लक्ष्य बनाते हैं।


| फ़ील्ड (Field) | मान (Value) |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **टेस्ट स्टेटस (Test Status)** | निष्पादित नहीं (Not Performed) |
| **गंभीरता (Severity)** | मध्यम / उच्च* (Medium / High*) |

> *यदि इंटरफेस मल्टी-फैक्टर ऑथेंटिकेशन (Multi-factor authentication - MFA) के बिना सिस्टम-व्यापी कॉन्फ़िगरेशन परिवर्तन, उपयोगकर्ता प्रबंधन, या डेटा एक्सफ़िल्ट्रेशन की अनुमति देता है, तो गंभीरता उच्च हो जाती है।

**संदर्भ (References):**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**टूल्स (Tools):** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### क्या एडमिनिस्ट्रेटिव इंटरफेस डायरेक्टरी फ़ज़िंग (Directory Fuzzing) या रीकॉनेसेंस के माध्यम से खोजे जा सकते हैं?
- [ ] नहीं — कोई एडमिनिस्ट्रेटिव इंटरफेस एक्सपोज्ड या खोजने योग्य नहीं है  
- [ ] हाँ — इंटरफेस खोजने योग्य हैं लेकिन एक्सेस आंतरिक IP श्रेणियों (Internal IP ranges) तक प्रतिबंधित है  
- [ ] हाँ — इंटरफेस खोजने योग्य हैं और सार्वजनिक रूप से सुलभ हैं  

### क्या खोजे गए एडमिनिस्ट्रेटिव पोर्टल पर प्रमाणीकरण (Authentication) अनिवार्य है?
- [ ] हाँ — मल्टी-फैक्टर ऑथेंटिकेशन (MFA) लागू है  
- [ ] हाँ — सिंगल-फैक्टर ऑथेंटिकेशन लागू है  
- [ ] नहीं — इंटरफेस तक पहुँचने के लिए किसी प्रमाणीकरण की आवश्यकता नहीं है *(क्रिटिकल/Critical)*  

### क्या खोज को रोकने के लिए एडमिनिस्ट्रेटिव पाथ (Paths) छिपे हुए या गैर-मानक हैं?
- [ ] हाँ — पाथ गैर-मानक, रैंडमाइज्ड, या गैर-अनुमानित नामकरण का उपयोग करते हैं  
- [ ] नहीं — पाथ सामान्य नामकरण परंपराओं (जैसे, `/admin`, `/manager`, `/console`) का उपयोग करते हैं और आसानी से अनुमान लगाए जा सकते हैं  

### क्या इंटरफेस संवेदनशील सिस्टम या एप्लीकेशन कार्यक्षमता तक पहुंच प्रदान करता है?
- [ ] नहीं — इंटरफेस एक "रीड-ओनली" स्टेटस डैशबोर्ड है जिसका प्रभाव न्यूनतम है  
- [ ] हाँ — इंटरफेस एप्लीकेशन कॉन्फ़िगरेशन या उपयोगकर्ता डेटा को संशोधित कर सकता है  
- [ ] हाँ — इंटरफेस सिस्टम-स्तरीय कमांड (System-level commands) निष्पादित कर सकता है या डेटाबेस प्रबंधन कर सकता है  

---