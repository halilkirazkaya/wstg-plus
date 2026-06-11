## WSTG-ATHN-11 — 다요소 인증(Multi-Factor Authentication, MFA) 테스트

다요소 인증(MFA) 테스트는 기본 인증 정보가 유출된 경우에도 무단 액세스를 방지하기 위해 설계된 보조 보안 계층의 견고성을 평가합니다. 공격자는 주로 레거시 API(API) 버전, 모바일 백엔드 또는 비밀번호 재설정 워크플로와 같이 MFA가 일관되게 적용되지 않는 엔드포인트(Endpoint)를 식별하여 MFA를 우회하려고 시도합니다. 취약점 공격에는 서버 응답 조작, 수명이 짧은 일회용 비밀번호(OTP)에 대한 브루트 포스(Brute Force), 또는 사용자가 두 번째 요소를 제공하지 않고도 인증된 상태로 전환할 수 있도록 허용하는 레이스 컨디션(Race Condition) 및 세션 관리 결함을 악용하는 행위가 포함됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 높음 (High) / 치명적 (Critical) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### 모든 인증 포털에서 MFA가 일관되게 강제 적용됩니까?
- [ ] 예 — 모든 웹, 모바일 및 API 기반 로그인 시도에 MFA가 요구됩니다  
- [ ] 예 — 하지만 특정 엔드포인트(예: 레거시 `/api/v1/login` 또는 모바일 전용 포털)에서는 MFA가 **강제되지 않습니다**  
- [ ] 아니요 — 애플리케이션에 MFA가 **구현되지 않았습니다** *(치명적)*  

### URL 직접 탐색 또는 응답 조작을 통해 MFA 검증 단계를 우회할 수 있습니까?
- [ ] 아니요 — 직접 탐색 또는 파라미터 변조로 챌린지(Challenge)를 **우회할 수 없습니다**  
- [ ] 예 — MFA 챌린지를 완료하지 않고 내부 대시보드 URL로 직접 탐색이 **가능합니다**  
- [ ] 예 — 액세스 권한을 얻기 위해 서버 응답을 조작(예: `401 Unauthorized`를 `200 OK`로 변경)하는 것이 **가능합니다**  

### MFA 코드 검증 프로세스가 브루트 포스 공격으로부터 보호됩니까?
- [ ] 예 — 여러 번의 OTP 시도 실패 후 엄격한 속도 제한(Rate Limiting) 또는 계정 잠금이 **적용됩니다**  
- [ ] 예 — 속도 제한이 존재하지만 IP 로테이션 또는 헤더 조작을 통해 우회가 **가능합니다**  
- [ ] 아니요 — 속도 제한이 강제되지 않으며, 코드에 대한 자동화된 브루트 포스가 **가능합니다**  

### 애플리케이션이 MFA 전환 중에 보안 세션 상태를 유지합니까?
- [ ] 예 — 높은 권한의 세션 토큰은 MFA가 성공적으로 완료된 **후에만** 발행됩니다  
- [ ] 아니요 — MFA가 완료되기 **전에** 기능이 온전한 세션 쿠키가 발행되어 일부 기능에 대한 액세스를 허용합니다  
- [ ] 아니요 — 애플리케이션이 탈취 가능한 정적 식별자 또는 예측 가능한 세션 전환을 사용합니다  

### 대체 MFA 수단(SMS, 이메일, 백업 코드)이 취약점 공격에 노출되어 있습니까?
- [ ] 아니요 — 모든 수단이 안전하고 예측 불가능하며 시간 제한이 있는 값을 사용합니다  
- [ ] 예 — 백업 코드가 예측 가능하거나 열거(Enumerate) **가능합니다**  
- [ ] 예 — "코드 재전송" 기능을 악용하여 SMS/이메일 플러딩(Flooding)을 수행하거나 연락처 세부 정보의 일부를 노출할 수 있습니다  

---