## WSTG-ATHN-01 — 암호화된 채널을 통한 자격 증명(Credentials) 전송 테스트

암호화된 채널을 통한 자격 증명 전송 여부를 테스트함으로써 사용자 이름, 비밀번호 및 세션 토큰(Session Tokens)과 같은 민감한 인증 데이터가 전송 중에 가로채기(Interception)로부터 보호되는지 확인합니다. 공용 Wi-Fi에서의 중간자 공격(Man-in-the-Middle, MitM) 또는 침해된 내부 네트워크와 같이 네트워크에 위치한 공격자는 TLS/SSL이 적절하게 강제되지 않을 경우 패킷 스니핑(Packet Sniffing) 도구를 사용하여 평문(Cleartext) 자격 증명을 캡처할 수 있습니다. 이 취약점은 일반적으로 애플리케이션이 HTTPS를 의무화하지 않거나 HTTP에서 부적절하게 리다이렉트(Redirect)하는 로그인 엔드포인트, 비밀번호 재설정 양식 및 API 인증 헤더에서 발생합니다. 성공적인 공격은 공격자에게 사용자 계정에 대한 전체 액세스 권한을 부여하여 사용자 데이터의 완전한 침해 및 애플리케이션 내에서의 잠재적인 측면 이동(Lateral Movement)으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **테스트 상태** | Not Performed |
| **위험도** | High |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**도구:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### 전체 인증 프로세스에 HTTPS가 강제됩니까?
- [ ] 예 — HSTS가 활성화(Enabled)되어 있으며 모든 트래픽이 HTTPS를 통해 엄격하게 강제됩니다.
- [ ] 예 — HTTPS로의 리다이렉트가 발생하지만, HSTS는 활성화되지 않았습니다.
- [ ] 아니요 — 자격 증명이 암호화되지 않은 HTTP를 통해 제출될 수 있습니다.

### 로그인 페이지와 양식이 암호화된 채널을 통해 제공됩니까?
- [ ] 예 — 로그인 양식을 포함하는 페이지가 HTTPS를 통해 전달됩니다.
- [ ] 아니요 — 로그인 페이지가 HTTP를 통해 제공되어 폼 액션 하이재킹(Form-action Hijacking) 또는 자격 증명 스니핑이 가능합니다.

### 모든 민감한 세션 쿠키에 `Secure` 플래그가 적용되어 있습니까?
- [ ] 예 — 모든 인증 및 세션 쿠키에 `Secure` 플래그가 적용되어 있습니다.
- [ ] 예 — 일부 쿠키에만 `Secure` 플래그가 적용되어 있습니다.
- [ ] 아니요 — `Secure` 플래그가 적용되지 않아 암호화되지 않은 요청을 통해 쿠키가 유출될 수 있습니다.

### 서버가 취약한 TLS 버전이나 안전하지 않은 암호화 스위트(Cipher Suites)를 지원합니까?
- [ ] 아니요 — 현대적인 TLS(1.2+) 및 강력한 암호화 알고리즘만 활성화되어 있습니다.
- [ ] 예 — 레거시 버전(TLS 1.0/1.1) 또는 취약한 암호화 알고리즘(RC4, DES, CBC)이 지원됩니다.

### 애플리케이션이 암호화되지 않은 채널을 통해 서드파티(Third-party) 도메인으로 자격 증명이 전송되는 것을 방지합니까?
- [ ] 예 — 외부 도메인으로 민감한 데이터가 전송되지 않거나 모든 외부 호출이 HTTPS를 통해 강제됩니다.
- [ ] 아니요 — 인증 헤더 또는 자격 증명이 HTTP를 통해 서드파티 엔드포인트(예: 분석 도구 또는 SSO)로 전송됩니다.

---