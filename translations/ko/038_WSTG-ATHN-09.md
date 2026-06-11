## WSTG-ATHN-09 — 취약한 비밀번호 변경 또는 초기화 기능 테스트

비밀번호 변경 및 초기화 메커니즘(Password change and reset mechanisms)은 인증 관리의 핵심 구성 요소로, 부적절하게 구현될 경우 공격자가 사용자 계정을 탈취할 수 있게 합니다. 이러한 취약점은 일반적으로 예측 가능한 초기화 토큰, 브루트 포스(Brute Force) 공격 시 계정 잠금 부재, 안전하지 않은 토큰 전달, 또는 현재 비밀번호를 확인하지 않고 비밀번호를 변경할 수 있는 기능 등과 관련이 있습니다. 공격자는 복구 링크를 가로채거나 추측하고, 호스트 헤더 인젝션(Host Header Injection)을 수행하여 초기화 이메일을 리다이렉트하거나, 세션 고정(Session Fixation)을 활용해 비밀번호 변경 후에도 액세스 권한을 유지하는 방식으로 이러한 결함을 악용합니다. 이러한 프로세스가 강력한 검증을 요구하고 암호학적 무작위성(Cryptographic randomness)을 활용하도록 보장하는 것은 계정 무결성을 유지하고 무단 액세스를 방지하는 데 필수적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 높음(High) / 치명적(Critical) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**도구:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### 표준 변경 중에 새 비밀번호를 설정하기 위해 현재 비밀번호가 요구됩니까?
- [ ] 예 — 현재 비밀번호가 **요구되며** 검증됨  
- [ ] 예 — 현재 비밀번호가 **요구되나** 파라미터 오염(Parameter Pollution) 또는 조작을 통해 우회 가능함  
- [ ] 아니요 — 현재 비밀번호가 **요청되지 않아** 세션 하이재킹(Session Hijacking)을 통한 계정 탈취가 가능함 *(High)*  

### 비밀번호 초기화 토큰이 암호학적으로 안전하고 예측 불가능합니까?
- [ ] 예 — 토큰이 높은 엔트로피(Entropy), 무작위성을 가지며 **예측 불가능함**  
- [ ] 예 — 토큰이 생성되지만 낮은 엔트로피를 보이거나 예측 가능한 패턴(예: 타임스탬프 기반)이 있음  
- [ ] 아니요 — 토큰이 **예측 가능하거나** Base64 인코딩된 이메일/ID와 같은 정적 사용자 데이터를 기반으로 함 *(Critical)*  

### 비밀번호 초기화 링크가 호스트 헤더 인젝션을 통해 리다이렉트될 수 있습니까?
- [ ] 아니요 — 애플리케이션이 링크 생성을 위해 `Host` 헤더를 무시하거나 검증함  
- [ ] 예 — 애플리케이션이 링크를 구성하기 위해 `Host` 또는 `X-Forwarded-Host` 헤더를 사용하지만, 검증이 **존재함**  
- [ ] 예 — 헤더 조작을 통해 공격자가 제어하는 도메인으로 링크를 **리다이렉트할 수 있음** *(High)*  

### 초기화 토큰이 제한된 수명을 가지며 즉시 무효화됩니까?
- [ ] 예 — 토큰이 빠르게 만료되며 사용 또는 비밀번호 변경 직후 **즉시** 무효화됨  
- [ ] 예 — 토큰이 긴 기간(예: 24시간 이상) 후에 만료되거나 **여러 번 사용**을 허용함  
- [ ] 아니요 — 토큰이 **만료되지 않거나** 비밀번호가 성공적으로 변경된 후에도 유효함  

### 비밀번호 초기화 및 변경 엔드포인트에 속도 제한(Rate Limiting) 또는 잠금 설정이 있습니까?
- [ ] 예 — 열거(Enumeration) 및 브루트 포스를 방지하기 위해 엄격한 속도 제한 및 CAPTCHA가 **적용됨**  
- [ ] 예 — 제한적인 제어가 존재하지만 IP 로테이션 또는 헤더 스푸핑(Header Spoofing)을 통해 우회가 **가능함**  
- [ ] 아니요 — 속도 제한이 **적용되지 않아** 대량의 계정 열거 또는 토큰 브루트 포싱이 가능함  

---