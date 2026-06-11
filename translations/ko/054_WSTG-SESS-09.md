## WSTG-SESS-09 — 세션 하이재킹(Session Hijacking) 테스트

세션 하이재킹(Session Hijacking)은 공격자가 유효한 세션 식별자(Session Identifier, SID)를 탈취, 예측 또는 고정하여 사용자의 활성 세션에 대한 무단 액세스 권한을 획득할 때 발생합니다. 이 취약점은 일반적으로 불충분한 전송 계층 보안, 쿠키 보안 플래그 부재, 또는 공격자가 인증을 완전히 우회할 수 있도록 하는 예측 가능한 SID 생성 알고리즘을 통해 나타납니다. 공격자의 관점에서 성공적인 익스플로잇(Exploit)은 자격 증명 없이 피해자를 완전히 사칭할 수 있게 하며, 이는 주로 네트워크 스니핑(Network Sniffing), 교차 사이트 스크립팅(Cross-Site Scripting, XSS) 또는 세션 고정(Session Fixation) 공격을 통해 이루어집니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 / 치명적 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**도구:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### 전송 중에 세션 식별자가 보호됩니까?
- [ ] 예 — `Secure` 플래그가 **활성화**되어 있으며 HSTS가 엄격하게 적용됨  
- [ ] 예 — `Secure` 플래그가 **활성화**되어 있으나 HSTS가 **비활성화**됨  
- [ ] 아니요 — `Secure` 플래그가 **적용되지 않아**, 암호화되지 않은 채널을 통한 SID 가로채기가 가능함 *(높음)*  

### 세션 식별자가 클라이언트 측 스크립트 액세스로부터 보호됩니까?
- [ ] 예 — 모든 세션 관련 쿠키에 `HttpOnly` 플래그가 **적용됨**  
- [ ] 아니요 — `HttpOnly` 플래그가 **적용되지 않아**, XSS를 통한 SID 유출이 가능함 *(치명적)*  

### 애플리케이션이 세션을 클라이언트 속성에 바인딩(Binding)합니까?
- [ ] 예 — 세션이 클라이언트 속성(IP/User-Agent)에 바인딩되어 있으며 재사용이 **불가능함**  
- [ ] 예 — 세션 바인딩이 존재하지만 헤더 스푸핑(Header Spoofing)을 통해 우회가 **가능함**  
- [ ] 아니요 — 세션 바인딩이 존재하지 않음; SID가 어떤 소스에서 사용되더라도 **유효함**  

### 세션 식별자가 예측을 방지할 수 있을 만큼 충분히 무작위입니까?
- [ ] 예 — SID가 암호학적으로 안전한 의사 난수 생성기(Cryptographically Secure Pseudo-Random Number Generator, CSPRNG)를 사용함  
- [ ] 아니요 — SID 길이가 불충분하거나 **예측 가능한** 시퀀스/패턴을 따름  

### 공격 노출 시간을 제한하기 위해 동시 세션(Concurrent Sessions)이 관리됩니까?
- [ ] 아니요 — 애플리케이션이 사용자당 단일 활성 세션을 엄격하게 강제함  
- [ ] 예 — 다중 동시 세션이 **활성화**되어 있으나 모니터링됨  
- [ ] 예 — 서로 다른 기기/위치에서 무제한 동시 세션이 **가능함**  

---