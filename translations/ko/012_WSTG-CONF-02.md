## WSTG-CONF-02 — 애플리케이션 플랫폼 설정 점검 (Application Platform Configuration Testing)

애플리케이션 플랫폼 설정 점검은 하위 웹 서버, 애플리케이션 서버 및 프레임워크(Framework) 설정을 감사하여 일반적인 익스플로잇(Exploit)에 대해 보안이 강화(Hardened)되었는지 확인하는 과정을 포함합니다. 공격자는 무단 접근 권한을 획득하거나 코드를 실행하기 위해 기본 계정 정보(Default Credentials), 패치되지 않은 플랫폼 취약점, 노출된 관리자 인터페이스(Administrative Interfaces)를 적극적으로 탐색합니다. 설정 오류(Misconfigurations)는 종종 민감한 환경 변수(Environment Variables), 내부 시스템 경로(Internal System Paths) 노출 또는 공격 표면(Attack Surface)을 확장하는 불필요한 샘플 애플리케이션의 존재로 이어집니다. 전문적인 관점에서 볼 때, 잘못 설정된 플랫폼은 대상 환경에 대한 초기 접근(Initial Access)을 달성할 가능성이 높은 진입점 역할을 합니다.


| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) / 높음(High)* |

> *관리자 인터페이스가 기본 계정 정보로 접근 가능하거나 샘플 스크립트로 인해 원격 코드 실행(Remote Code Execution, RCE)이 가능한 경우 위험도는 '높음(High)'으로 분류됩니다.

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### 플랫폼에 기본 계정 정보 또는 계정이 활성화되어 있습니까?
- [ ] 아니오 — 모든 기본 계정이 **비활성화**되었거나 비밀번호가 변경됨  
- [ ] 예 — 기본 계정이 존재하지만 외부 네트워크에서는 **접근할 수 없음**  
- [ ] 예 — 기본 계정이 **활성화**되어 있으며 인터넷을 통해 접근 가능함 *(위험)*  

### 플랫폼이 헤더 또는 에러 페이지를 통해 민감한 버전 정보를 노출하고 있습니까?
- [ ] 아니오 — 버전 문자열이 **숨겨져** 있거나 일반적인 내용임  
- [ ] 예 — 상세한 버전 정보가 HTTP 헤더(예: `Server`, `X-Powered-By`)에 **노출됨**  
- [ ] 예 — 상세 에러 메시지에 전체 스택 트레이스(Stack Traces) 및 내부 시스템 경로가 **노출됨**  

### 관리 또는 경영 인터페이스가 적절히 제한되어 있습니까?
- [ ] 아니오 — 관리자 인터페이스가 **노출되지 않음**  
- [ ] 예 — 인터페이스가 존재하지만 IP 허용 목록(IP Allowlisting) 또는 다중 요소 인증(Multi-Factor Authentication, MFA)에 의해 **제한됨**  
- [ ] 예 — 인터페이스(예: `/admin`, `/manager`, `/console`)가 단일 요소 인증만으로 **공개적으로 접근 가능함**  

### 운영 서버에 샘플 파일, 스크립트 또는 문서가 존재합니까?
- [ ] 아니오 — 불필요한 파일이 모두 **제거됨**  
- [ ] 예 — 샘플 파일 또는 문서가 **존재**하지만 민감한 정보를 유출하지 않음  
- [ ] 예 — 샘플 스크립트(예: `info.php`, `examples/`)가 **존재**하며 직접적인 공격 벡터를 제공함  

### 서버가 취약한 HTTP 메소드(HTTP Methods)를 지원하도록 설정되어 있습니까?
- [ ] 아니오 — `GET` 및 `POST`만 **활성화됨**  
- [ ] 예 — `PUT`, `DELETE` 또는 `TRACE`와 같이 잠재적으로 위험한 메소드가 **활성화**되어 있으나 제한됨  
- [ ] 예 — 위험한 메소드가 **활성화**되어 있으며 무단 파일 수정 또는 계정 정보 탈취가 가능함  

---