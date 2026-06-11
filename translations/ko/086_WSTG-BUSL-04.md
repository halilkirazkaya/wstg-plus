## WSTG-BUSL-04 — 프로세스 타이밍 테스트 (Process Timing)

프로세스 타이밍 공격 (Process Timing Attack)은 특정 요청을 처리하는 데 걸리는 시간을 측정하여 내부 상태나 데이터 존재 여부에 대한 민감한 정보를 유추하는 기법입니다. 조기 종료 조건 로직 (Early-exit conditional logic), 데이터베이스 조회 (Database lookup), 또는 비고정 시간 암호화 비교 (Non-constant-time cryptographic comparison)로 인해 발생하는 밀리초 단위의 응답 시간 차이를 분석함으로써, 공격자는 부채널 분석 (Side-channel analysis)을 수행하여 유효한 사용자 이름을 열거하거나 비밀번호 조각을 검증하고 특정 레코드의 존재를 확인할 수 있습니다. 이러한 취약점은 주로 인증 엔드포인트 (Authentication endpoint), 비밀번호 초기화 폼, 그리고 입력값의 유효성에 따라 백엔드 로직이 분기되는 리소스 집약적인 API 필터 (Resource-heavy API filter)에서 발생합니다. 공격자의 관점에서 이러한 타이밍 차이는 애플리케이션이 사용자에게 동일하고 일반적인 오류 메시지를 반환하더라도 백엔드 데이터에 대한 진위를 드러내는 ‘불리언 오라클 (Boolean oracle)’ 역할을 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 (Medium) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**도구:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### 애플리케이션이 유효한 리소스 요청과 유효하지 않은 리소스 요청에 대해 상이한 응답 시간을 보입니까?
- [ ] 아니오 — 입력값의 유효성과 관계없이 응답 시간이 동일함  
- [ ] 예 — 미세한 타이밍 차이가 존재하지만 통계적으로 유의미하지 않음  
- [ ] 예 — 일관된 타이밍 차이가 관찰되며 신뢰할 수 있는 오라클을 제공함  

### 응답 시간을 정규화하기 위해 고정 시간 비교 함수 (Constant-time comparison function) 또는 인위적인 지연이 구현되어 있습니까?
- [ ] 예 — 모든 민감한 비교 작업에 고정 시간 알고리즘이 적용됨 *(가장 안전)*  
- [ ] 예 — 인위적인 지연 또는 지터 (Jitter)가 활성화되어 있으나, 기본 로직은 여전히 비고정 시간 방식임  
- [ ] 아니오 — 타이밍 완화 조치가 적용되지 않아 로직 분기를 직접 측정할 수 있음  

### 공격자가 통계적 분석을 사용하여 네트워크 노이즈로부터 타이밍 신호를 분리할 수 있습니까?
- [ ] 아니오 — 네트워크 지터 및 애플리케이션 노이즈로 인해 타이밍 분석이 불가능함  
- [ ] 예 — 충분한 샘플 크기가 확보되면 노이즈로부터 타이밍 신호를 분리할 수 있음  
- [ ] 예 — 타이밍 델타 (Delta)가 충분히 커서 통계적 정규화가 필요하지 않음  

### 로그인 또는 비밀번호 초기화 기능을 통해 계정 열거 (Account enumeration)가 가능합니까?
- [ ] 아니오 — 타이밍을 통해 계정 존재 여부를 확인할 수 없음  
- [ ] 예 — 타이밍 차이를 통해 원격 공격자가 유효한 사용자 이름 또는 이메일을 열거할 수 있음  

### 리소스 집약적인 작업(예: 파일 처리, 복잡한 검색)이 타이밍을 통해 데이터 존재 여부를 유출합니까?
- [ ] 아니오 — 레코드 발견 여부와 관계없이 리소스 소비가 일정함  
- [ ] 예 — 백엔드 처리 시간을 측정하여 민감한 레코드의 존재 여부를 유추할 수 있음  

---