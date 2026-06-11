## WSTG-CONF-01 — 네트워크 인프라 설정 점검 (Network Infrastructure Configuration Testing)

네트워크 인프라 설정 점검(Network Infrastructure Configuration Testing)은 웹 애플리케이션을 지원하는 기본 서버 및 네트워크 구성 요소의 취약점을 식별하는 과정입니다. 공격자는 잘못 설정된 서비스, 구형 프로토콜 및 불필요하게 열린 포트를 표적으로 삼아 발판을 마련하거나, 측면 이동(Lateral Movement)을 수행하거나, 정찰(Reconnaissance)을 진행합니다. 주요 발견 사항으로는 취약한 TLS(Transport Layer Security) 버전, 상세한 서비스 배너(Service Banner), 내부 네트워크로 제한되어야 할 노출된 관리 인터페이스 등이 있습니다. 공격자는 이러한 인프라 계층의 결함을 악용하여 전송 중인 데이터의 기밀성을 침해하거나 호스트 운영체제(OS)에 대한 비인가 접근 권한을 획득할 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 낮음 / 중간 |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**도구:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### 애플리케이션 호스트에 불필요하게 열린 포트나 노출된 서비스가 있습니까?
- [ ] 아니요 — 필수 포트(예: 443)만 **접근 가능함**  
- [ ] 예 — 추가 서비스가 노출되어 있으나 **보안** 설정을 따름  
- [ ] 예 — 비필수 서비스(예: FTP, Telnet, SMB)가 공용 네트워크에 **노출됨**  

### 서비스 배너 또는 헤더에서 민감한 버전 정보가 유출됩니까?
- [ ] 아니요 — 배너가 제거되었거나 일반적인 내용임  
- [ ] 예 — 배너가 서버 소프트웨어(예: Apache/nginx)를 노출하지만 버전 번호는 **포함하지 않음**  
- [ ] 예 — 상세한 배너가 특정 소프트웨어 버전을 노출하여 **익스플로잇(Exploit)**을 용이하게 함  

### TLS 설정이 최신 보안 권장 사항(Best Practices)을 따르고 있습니까?
- [ ] 예 — 강력한 암호화 제품군(Cipher Suite)과 함께 TLS 1.2/1.3만 **활성화됨**  
- [ ] 예 — 구형 프로토콜(TLS 1.0/1.1)이 **활성화되어 있음**  
- [ ] 아니요 — 취약한 암호 또는 보안되지 않은 프로토콜(SSLv2/v3)이 **활성화됨**  

### 관리자 또는 매니지먼트 인터페이스가 허가된 네트워크로 제한되어 있습니까?
- [ ] 예 — 인터페이스(예: SSH, RDP, 제어판)가 공용 인터넷에서 **접근 불가능함**  
- [ ] 아니요 — 인터페이스가 외부에서 **접근 가능**하지만 다중 요소 인증(MFA)이 요구됨  
- [ ] 아니요 — 인터페이스가 단일 요소 인증 또는 기본 자격 증명(Default Credentials)을 사용하여 외부에서 **접근 가능함**  

---