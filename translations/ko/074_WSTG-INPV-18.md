## WSTG-INPV-18 — 서버 측 템플릿 주입(Server-Side Template Injection, SSTI) 점검

서버 측 템플릿 주입(Server-Side Template Injection, SSTI)은 애플리케이션이 적절한 검증 및 정제(Sanitization) 또는 이스케이핑(Escaping) 없이 사용자 제어 입력을 서버 측 템플릿 엔진에 포함할 때 발생합니다. 이를 통해 공격자는 악성 템플릿 지시어를 주입할 수 있으며, 이는 서버에서 실행되어 원격 코드 실행(Remote Code Execution, RCE)을 통한 완전한 시스템 침해로 이어질 수 있습니다. SSTI는 일반적으로 Jinja2, Twig 또는 FreeMarker와 같은 엔진을 사용하는 동적 이메일 생성, 프로필 페이지 및 알림 시스템과 같은 기능에서 나타납니다. 공격자 관점에서 이 취약점은 템플릿 엔진의 고유 객체 및 메서드를 활용하여 민감한 환경 변수를 유출하거나, 내부 파일을 읽거나, 시스템 명령을 실행하는 데 자주 악용됩니다.


| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 심각 (Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**도구:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### 애플리케이션이 사용자 입력을 처리하기 위해 서버 측 템플릿 엔진을 사용합니까?
- [ ] 아니요 — 애플리케이션이 동적 콘텐츠를 위해 서버 측 템플릿을 사용하지 **않음**  
- [ ] 예 — 템플릿이 사용되지만 사용자 입력이 템플릿 지시어에 포함되지 **않음**  
- [ ] 예 — 사용자 입력이 적절한 검증 및 정제 없이 템플릿에 **포함됨**  

### 입력 필드에 주입된 기본 산술 식이 평가(Evaluate)됩니까?
- [ ] 아니요 — `{{7*7}}` 또는 `${7*7}`과 같은 페이로드(Payload)가 리터럴 문자열로 그대로 출력됨  
- [ ] 예 — 식이 평가되어 결과(예: `49`)가 반환되며, 이는 템플릿 엔진이 **취약함**을 확인하는 지표임  

### 템플릿 엔진의 내장 객체 또는 메서드에 접근할 수 있습니까?
- [ ] 아니요 — 위험한 객체, 메서드 또는 설정에 대한 접근이 **제한**되거나 샌드박스(Sandbox) 처리되어 있음  
- [ ] 예 — 내장 객체(예: `self`, `config`, `__class__`)에 접근하여 민감한 데이터를 유출할 수 **있음**  
- [ ] 예 — 시스템 호출 또는 OS 라이브러리를 통한 원격 코드 실행(RCE)이 **가능함**  

### 위험한 지시어의 실행을 방지하는 샌드박스 또는 화이트리스트(Allowlist) 메커니즘이 있습니까?
- [ ] 예 — 엄격한 화이트리스트 또는 보안 샌드박스가 적용되어 있으며 우회(Bypass)가 **불가능함**  
- [ ] 예 — 샌드박스가 존재하지만 특정 엔진 종속적인 페이로드를 통해 우회가 **가능함**  
- [ ] 아니요 — 템플릿 엔진에 샌드박스 또는 화이트리스트가 **적용되지 않음**  

---