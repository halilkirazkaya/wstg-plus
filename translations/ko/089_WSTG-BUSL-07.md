## WSTG-BUSL-07 — 애플리케이션 오용 방어 테스트 (Test Defenses Against Application Misuse)

애플리케이션 오용(Misuse)에 대한 방어는 의도된 비즈니스 로직(Business Logic)에서 벗어나는 비정상적인 사용자 행위를 식별하고, 로깅(Logging)하며, 이에 대응하는 애플리케이션의 능력을 평가합니다. 전통적인 시그니처 기반 보안(Signature-based Security)과 달리, 이러한 방어 체계는 자동화된 남용을 의미하는 급격한 요청, 크리덴셜 스터핑(Credential Stuffing) 또는 순차적 리소스 열거와 같은 패턴을 탐지하는 데 집중합니다. 공격자의 관점에서 이 테스트는 표준 보안 알림을 트리거하지 않고 데이터를 유출하거나 리소스를 고갈시키도록 설계된 자동화(Automation) 및 "Low and Slow" 공격에 대한 애플리케이션의 복원력을 결정합니다. 이러한 방어 체계를 구현하지 못하면 대규모 데이터 스크레이핑(Data Scraping), 계정 탈취(Account Takeover) 또는 합법적이지만 리소스 집약적인 애플리케이션 기능을 통한 서비스 거부(Denial of Service - DoS)가 발생할 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 중간 (Medium) / 높음 (High) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### 민감한 엔드포인트(Endpoint)에 전송 속도 제한(Rate Limiting) 또는 스로틀링(Throttling) 메커니즘이 적용되어 있습니까?
- [ ] 예 — 엄격한 Rate Limiting이 **적용되어 있으며** 모든 민감한 엔드포인트에 대해 강제됩니다. *(가장 안전)*  
- [ ] 예 — Rate Limiting이 **적용되어 있으나** IP 교체(Rotation) 또는 헤더 조작을 통해 우회가 가능합니다.  
- [ ] 예 — 인증 엔드포인트에만 Rate Limiting이 **적용되어** 있으며, 다른 엔드포인트는 노출되어 있습니다.  
- [ ] 아니요 — 어떤 애플리케이션 기능에도 Rate Limiting 또는 Throttling이 **적용되지 않았습니다.**  

### 애플리케이션이 자동화된 상호작용 패턴을 탐지하고 차단합니까?
- [ ] 예 — 행위 분석 또는 CAPTCHA가 **활성화되어 있으며** 봇(Bot)을 효과적으로 차단합니다.  
- [ ] 예 — 자동화 방지 기능이 **활성화되어 있으나** 헤드리스 브라우저(Headless Browser) 또는 세션 사이클링을 통한 우회가 **가능합니다.**  
- [ ] 아니요 — 애플리케이션이 실제 사용자와 자동화된 스크립트를 **구분할 수 없습니다.**  

### 비정상 행위가 로깅되고 방어적 대응이 뒤따릅니까?
- [ ] 예 — 오용 탐지 시 자동 차단이 트리거되며 보안 팀에 알림이 전송됩니다.  
- [ ] 예 — 감사를 위해 오용이 로깅되지만 자동화된 방어 조치는 **취해지지 않습니다.**  
- [ ] 아니요 — 애플리케이션이 특이한 활동 패턴을 로깅하거나 이에 **대응하지 않습니다.**  

### 클라이언트 측 식별자 또는 헤더를 조작하여 방어 체계를 우회할 수 있습니까?
- [ ] 아니요 — 방어 체계가 서버 측 상태 및 검증된 원격 측정(Telemetry) 데이터에 의존합니다.  
- [ ] 예 — 쿠키를 삭제하거나 `User-Agent` 문자열을 교체하여 통제 항목을 **우회할 수 있습니다.**  
- [ ] 예 — `X-Forwarded-For` 또는 `X-Real-IP`와 같은 헤더를 삽입하여 통제 항목을 **우회할 수 있습니다.**  

---