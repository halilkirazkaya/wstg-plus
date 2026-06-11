## WSTG-CONF-14 — অন্যান্য HTTP সিকিউরিটি হেডার মিসকনফিগারেশন পরীক্ষা (Test Other HTTP Security Header Misconfigurations)

HTTP সিকিউরিটি হেডার (Security Header) মিসকনফিগারেশন পরীক্ষার অন্তর্ভুক্ত হলো এমন সব ডিফেন্সিভ হেডারগুলোর উপস্থিতি এবং সঠিক বাস্তবায়ন যাচাই করা, যা সাধারণ ওয়েব-ভিত্তিক অ্যাটাক ভেক্টরগুলোর (Attack Vectors) বিরুদ্ধে প্রয়োজনীয় সুরক্ষা প্রদান করে। যদিও এই হেডারগুলো মূল কোডের দুর্বলতাগুলো সংশোধন করে না, তবে এগুলোর অনুপস্থিতি ম্যান-ইন-দ্য-মিডল (Man-in-the-Middle - MITM) অ্যাটাক, MIME-sniffing এক্সপ্লয়টেশন (Exploitation), এবং রেফারারের (Referrers) মাধ্যমে অনিচ্ছাকৃত ক্রস-অরিজিন ডেটা লিকেজের (Cross-origin data leakage) ঝুঁকি উল্লেখযোগ্যভাবে বাড়িয়ে দেয়। একজন আক্রমণকারীর (Attacker) দৃষ্টিকোণ থেকে, অনুপস্থিত সিকিউরিটি হেডারগুলো একটি দুর্বলভাবে সুরক্ষিত এনভায়রনমেন্টের (Environment) স্পষ্ট নির্দেশক, যা প্রায়শই সেশন হাইজ্যাকিং (Session Hijacking) বা ইনফরমেশন এক্সফিল্ট্রেশনের (Information Exfiltration) মতো আরও জটিল এক্সপ্লয়টেশন চেইন তৈরিতে সহায়তা করে। এই কনফিগারেশনগুলো সাধারণত সার্ভার স্তরে মূল্যায়ন করা হয় এবং API এন্ডপয়েন্ট (Endpoint) ও স্ট্যাটিক রিসোর্সসহ (Static Resources) সমস্ত রেসপন্স টাইপের ক্ষেত্রে ধারাবাহিকভাবে প্রয়োগ করা উচিত।


| ক্ষেত্র | মান |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **পরীক্ষার অবস্থা** | সম্পন্ন করা হয়নি (Not Performed) |
| **গুরুত্বের মাত্রা (Severity)** | নিম্ন / মাঝারি (Low / Medium) |

**তথ্যসূত্র:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**টুলস:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### সিকিউরিটি হেডার উপস্থিতি অডিট (Security Header Presence Audit)
| হেডার | উপস্থিত | অনুপস্থিত | প্রস্তাবিত কনফিগারেশন (Recommended Configuration) |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` অথবা `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (ফিচার-নির্দিষ্ট, যেমন: `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### অ্যাপ্লিকেশন স্কোপ জুড়ে সিকিউরিটি হেডারগুলো কীভাবে প্রয়োগ করা হয়?
- [ ] হ্যাঁ — হেডারগুলো একটি সেন্ট্রাল লোড ব্যালেন্সার (Load Balancer) বা রিভার্স প্রক্সির (Reverse Proxy) মাধ্যমে গ্লোবালি প্রয়োগ করা হয়েছে এবং এগুলো ওভাররাইড (Override) করা সম্ভব নয়।
- [ ] হ্যাঁ — হেডারগুলো মূল ডকুমেন্টের রেসপন্সে প্রয়োগ করা হয়েছে কিন্তু API রেসপন্স বা স্ট্যাটিক অ্যাসেটগুলোতে (Static Assets) অনুপস্থিত।
- [ ] না — হেডারগুলো ধারাবাহিকভাবে প্রয়োগ করা হয়নি অথবা সম্পূর্ণ অ্যাপ্লিকেশন ডোমেন জুড়ে অনুপস্থিত।

### Strict-Transport-Security (HSTS) হেডারটি কি সঠিকভাবে কনফিগার করা হয়েছে?
- [ ] হ্যাঁ — `max-age` দীর্ঘ সময়ের জন্য (যেমন ২ বছর) সেট করা হয়েছে এবং `includeSubDomains` সক্রিয় (Enabled) করা আছে।
- [ ] হ্যাঁ — `max-age` সেট করা হয়েছে কিন্তু `includeSubDomains` নিষ্ক্রিয় (Disabled), যা সাবডোমেনগুলোকে অরক্ষিত রেখে দেয়।
- [ ] না — HSTS হেডার উপস্থিত নেই অথবা `max-age` ০ তে সেট করা আছে, যা প্রোটোকল ডাউনগ্রেড অ্যাটাকের (Protocol Downgrade Attacks) সুযোগ দেয়।

### Referrer-Policy কি সংবেদনশীল URL প্যারামিটার ফাঁস হওয়া প্রতিরোধ করে?
- [ ] হ্যাঁ — পলিসি `no-referrer` অথবা `same-origin` এ সেট করা আছে *(সবচেয়ে নিরাপদ)*।
- [ ] হ্যাঁ — পলিসি `strict-origin-when-cross-origin` এ সেট করা আছে।
- [ ] না — পলিসি সেট করা নেই অথবা `unsafe-url` এ সেট করা আছে, যার ফলে রেফারার হেডারে টোকেন বা সংবেদনশীল PII (Personally Identifiable Information) ফাঁস হওয়ার সম্ভাবনা থাকে।

### X-Content-Type-Options হেডারটি কি MIME-sniffing প্রতিরোধ করছে?
- [ ] হ্যাঁ — `nosniff` নির্দেশিকা উপস্থিত এবং সঠিকভাবে বাস্তবায়িত।
- [ ] না — হেডারটি অনুপস্থিত, যা ব্রাউজারকে অ-এক্সিকিউটেবল ফাইল (যেমন ইমেজ) স্ক্রিপ্ট হিসেবে চালানোর সুযোগ দিতে পারে।

---