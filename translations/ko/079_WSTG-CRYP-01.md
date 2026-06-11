## WSTG-CRYP-01 — 취약한 전송 계층 보안 테스트 (Testing for Weak Transport Layer Security)

취약한 전송 계층 보안 테스트는 데이터 전송 중의 기밀성과 무결성을 보장하기 위해 SSL/TLS의 구현 및 설정을 분석하는 과정을 포함합니다. 공격자는 중간자 공격 (Man-in-the-Middle, MitM)을 수행하기 위해 오래된 프로토콜(SSLv2, TLS 1.0), 취약한 암호 스위트 (Cipher Suite)(NULL, RC4 또는 익명), 유효하지 않거나 취약하게 서명된 인증서와 같은 설정 오류를 표적으로 삼습니다. 이 테스트는 일반적으로 암호화된 통신이 수립되는 애플리케이션의 웹 서버 또는 로드 밸런서 (Load Balancer) 엔드포인트를 대상으로 합니다. 성공적인 익스플로잇 (Exploit)을 통해 공격자는 민감한 데이터를 가로채거나, 사용자 세션을 탈취하거나, 프로토콜 다운그레이드 또는 트래픽 복호화를 통해 연결을 안전하지 않은 상태로 강제 전환할 수 있습니다.


| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 높음(High) / 중간(Medium)* |

> *민감한 데이터(개인 식별 정보 (Personally Identifiable Information, PII), 자격 증명 (Credentials), 세션 토큰 (Session Token))가 전송되는 경우 위험도는 '높음'이며, 일반적인 정보 트래픽의 경우 '중간'입니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### 오래된 또는 안전하지 않은 TLS/SSL 프로토콜이 지원됩니까?
- [ ] 아니요 — TLS 1.2 및 TLS 1.3만 **활성화됨**  
- [ ] 예 — TLS 1.0 또는 TLS 1.1이 **활성화됨**  
- [ ] 예 — SSLv2 또는 SSLv3가 **활성화됨** *(심각)*  

### 서버가 취약하거나 안전하지 않은 암호 스위트를 허용합니까?
- [ ] 아니요 — 강력하고 현대적인 암호 방식(예: AES-GCM, ChaCha20)만 **지원됨**  
- [ ] 예 — 취약한 암호화 방식(예: 3DES, RC4) 또는 낮은 비트 길이의 암호가 **지원됨**  
- [ ] 예 — NULL, 익명(ADH) 또는 수출용(Export) 암호가 **지원됨** *(심각)*  

### 서버 인증서가 유효하며 신뢰할 수 있습니까?
- [ ] 예 — 인증서가 유효하고, 도메인과 일치하며, **신뢰할 수 있는 인증 기관 (Certificate Authority, CA)**에 의해 서명됨  
- [ ] 아니요 — 인증서가 **만료됨** 또는 **취약한 서명 알고리즘**(예: SHA-1)을 사용함  
- [ ] 아니요 — 인증서가 **자기 서명됨 (Self-signed)** 또는 도메인 이름 **불일치 (Mismatch)**가 발생함  

### 서버가 안전한 재협상(Secure Renegotiation)을 지원하고 다운그레이드 공격을 방지합니까?
- [ ] 예 — 안전한 재협상이 **지원됨** 및 TLS Fallback SCSV가 **활성화됨**  
- [ ] 아니요 — 안전한 재협상이 **지원되지 않음**, 잠재적인 MitM 인젝션 허용  
- [ ] 아니요 — 서버가 **POODLE** 또는 **ROBOT** 공격에 취약함  

### HTTP 엄격한 전송 보안 (HTTP Strict Transport Security, HSTS)이 적절히 구현되었습니까?

| 헤더 | 존재함 | 누락됨 |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] 예 — HSTS 헤더가 충분히 긴 `max-age`와 함께 존재하며 `includeSubDomains`가 **활성화됨**  
- [ ] 예 — HSTS 헤더가 존재하지만 `includeSubDomains`가 누락되었거나 **짧은 max-age**를 가짐  
- [ ] 아니요 — HSTS 헤더가 **존재하지 않음**  

---