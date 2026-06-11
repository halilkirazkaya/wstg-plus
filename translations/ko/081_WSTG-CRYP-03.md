## WSTG-CRYP-03 — 암호화되지 않은 채널을 통해 전송되는 민감 정보 테스트 (Testing for Sensitive Information Sent Via Unencrypted Channels)

암호화되지 않은 일반 HTTP와 같은 채널을 통해 민감한 데이터를 전송하면, 중간자 공격 (Man-in-the-Middle, MITM)을 통해 정보가 가로채기되거나 수정될 위험이 있습니다. 이 취약점은 일반적으로 웹 애플리케이션이 모든 엔드포인트 (Endpoint)에서 전송 계층 보안 (Transport Layer Security, TLS)을 강제하지 않을 때 발생하며, 특히 레거시 컴포넌트, 로그인 폼, 또는 HTTP에서 HTTPS로 전환되는 초기 단계에서 자주 나타납니다. 공격자 관점에서 이는 공용 Wi-Fi와 같이 침해되었거나 신뢰할 수 없는 네트워크 세그먼트에서 인증 자격 증명, 세션 토큰 (Session Tokens), 개인 식별 정보 (Personally Identifiable Information, PII)를 수동적으로 스니핑 (Passive Sniffing) 할 수 있게 합니다. 모든 민감한 데이터가 견고한 TLS 터널 내에 캡슐화되도록 보장하는 것은 사용자 상호작용의 기밀성 및 무결성을 유지하고 세션 하이재킹 (Session Hijacking)을 방지하는 데 필수적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 높음 (High) / 중간 (Medium) |

> *인증 자격 증명, 세션 식별자 또는 PII가 평문 (Cleartext)으로 전송되는 경우 위험도는 '높음'이 됩니다.

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### 애플리케이션이 민감한 엔드포인트에 대해 암호화되지 않은 HTTP 통신을 허용합니까?
- [ ] 아니요 — 모든 애플리케이션 트래픽이 HTTPS로 엄격하게 강제됩니다 *(가장 안전)*  
- [ ] 예 — 민감하지 않은 페이지는 HTTP로 접근 가능하지만, 민감한 엔드포인트는 **접근할 수 없습니다**  
- [ ] 예 — 민감한 엔드포인트(로그인, 결제, 프로필 등)가 암호화되지 않은 HTTP로 **접근 가능합니다** *(높은 위험)*  

### HTTP에서 HTTPS로의 전역 리다이렉션 (Redirection)이 존재합니까?
- [ ] 예 — 애플리케이션이 모든 HTTP 요청에 대해 HTTPS로 301/302 리다이렉션을 수행합니다  
- [ ] 예 — 특정 민감한 엔드포인트에 대해서만 리다이렉션이 수행됩니다  
- [ ] 아니요 — HTTP에서 HTTPS로의 리다이렉션이 **적용되지 않았습니다**  

### HTTP 엄격한 전송 보안 (HTTP Strict Transport Security, HSTS)이 구현되어 있습니까?
- [ ] 예 — HSTS가 긴 `max-age`와 함께 **활성화**되어 있으며, `includeSubDomains` 및 `preload` 지시자를 포함합니다  
- [ ] 예 — HSTS가 **활성화**되어 있으나 `includeSubDomains` 또는 `preload` 지시자가 누락되었습니다  
- [ ] 아니요 — 애플리케이션 서버에 HSTS가 **활성화되지 않았습니다**  

### 민감한 쿠키 (Cookies)가 평문 전송으로부터 보호되고 있습니까?

| 쿠키 이름 | Secure 플래그 존재 | Secure 플래그 누락 |
|-------------|:-------------------:|:------------------:|
| `SessionID` |                     |                    |
| `AuthToken` |                     |                    |
| `CSRF-Token`|                     |                    |

### 애플리케이션이 암호화되지 않은 채널을 통해 서드파티 API로 민감한 데이터를 전송합니까?
- [ ] 아니요 — 모든 외부 API 호출이 암호화된(HTTPS) 터널을 통해 수행됩니다  
- [ ] 예 — 일부 민감하지 않은 텔레메트리 데이터가 HTTP를 통해 전송됩니다  
- [ ] 예 — API 키, PII 또는 인증 자격 증명이 암호화되지 않은 HTTP를 통해 서드파티로 **전송됩니다**  

---