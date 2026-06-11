## WSTG-ATHN-10 — 대체 채널의 취약한 인증 테스트 (Testing for Weaker Authentication in Alternative Channel)

대체 채널의 취약한 인증 테스트(Testing for weaker authentication in alternative channels)는 계정 액세스를 위한 보조 경로—모바일 API(Mobile API), 비밀번호 복구 워크플로(Password recovery workflows), 헬프 데스크 인터페이스(Help desk interfaces) 또는 레거시 서브도메인(Legacy subdomains) 등—를 식별하고 분석하는 작업을 포함합니다. 이러한 경로는 기본 웹 포털만큼의 보안 엄격성을 갖추지 못할 수 있습니다. 이러한 대체 채널은 종종 강력한 다요소 인증(Multi-factor authentication, MFA), 엄격한 전송 속도 제한(Rate Limiting) 또는 복잡한 비밀번호 요구 사항이 결여되어 전체 인증 시스템을 약화시키는 "가장 취약한 고리"가 됩니다. 공격자는 특히 이러한 간과된 진입점을 표적으로 삼아 크리덴셜 스터핑(Credential Stuffing)을 수행하거나, 서로 다른 인터페이스가 신원 확인을 처리하는 방식의 차이를 악용하여 MFA를 우회(Bypass)합니다. 모든 인증 표면에서 보안 동등성(Security parity)을 보장하는 것은 무단 액세스를 방지하고 사용자 세션 및 데이터의 무결성을 유지하는 데 필수적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **테스트 상태** | 미실시 (Not Performed) |
| **위험도** | 높음 (High) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### 대체 인증 채널이 존재하며 식별되었습니까?
- [ ] 아니오 — 단일 인증 채널만 존재함  
- [ ] 예 — 다중 채널이 식별됨 (예: 모바일 앱 API, 데스크톱 클라이언트, 레거시 포털, SOAP 서비스)  

### 대체 채널이 기본 채널과 동일한 수준의 보안을 강제합니까?
- [ ] 예 — 모든 채널이 **동일한** 비밀번호 복잡성 및 MFA 요구 사항을 강제함  
- [ ] 예 — 비밀번호 복잡성은 강제하지만, MFA가 **요구되지 않거나** 우회할 수 있음  
- [ ] 아니오 — 대체 채널이 **현저히 취약한** 인증 요구 사항을 가지고 있음  

### 전송 속도 제한 및 계정 잠금이 모든 채널에서 일관되게 적용됩니까?
- [ ] 예 — 전송 속도 제한 및 계정 잠금(Account lockout)이 모든 엔드포인트(Endpoint)에서 **일관되게 강제됨**  
- [ ] 예 — 통제 수단이 존재하지만 특정 채널(예: 모바일 API)에서 **우회가 가능함**  
- [ ] 아니오 — 보조 인증 경로에 전송 속도 제한 또는 계정 잠금이 **적용되지 않음**  

### 대체 채널로 전환하여 MFA를 우회할 수 있습니까?
- [ ] 아니오 — MFA가 필수이며 진입점에 관계없이 **우회할 수 없음**  
- [ ] 예 — 웹 포털에서는 MFA가 요구되지만 모바일 또는 레거시 API에서는 **강제되지 않음**  

### 비밀번호 복구 또는 "비밀번호 찾기" 흐름이 표준 로그인보다 취약합니까?
- [ ] 아니오 — 복구 흐름이 강력한 대역 외(Out-of-band) 인증을 사용하며 정보를 유출하지 않음  
- [ ] 예 — 복구 흐름이 쉽게 추측하거나 조사할 수 있는 취약한 보안 질문을 사용함  
- [ ] 예 — 복구 흐름이 열거(Enumeration) 공격에 취약하거나 표준 로그인과 **동일한 수준의** 증명을 요구하지 않음  

---