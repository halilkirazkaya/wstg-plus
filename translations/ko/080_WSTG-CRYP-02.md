## WSTG-CRYP-02 — 패딩 오라클(Padding Oracle) 테스트

패딩 오라클(Padding Oracle) 취약점은 애플리케이션의 복호화 과정에서 복호화된 암호문(Ciphertext)의 패딩(Padding) 유효성 여부에 대한 정보가 유출될 때 발생합니다. 공격자는 고유한 HTTP 상태 코드, 특정 오류 메시지 또는 응답 시간 차이와 같은 이러한 부가 채널(Side-channel) 응답을 관찰함으로써, 암호화 키를 모르더라도 데이터를 복호화하고 잠재적으로 임의의 평문(Plaintext)을 재암호화할 수 있습니다. 이 취약점은 일반적으로 PKCS#7 패딩이 적용된 CBC(Cipher Block Chaining) 모드의 블록 암호(Block Cipher)를 사용하는 암호화된 세션 쿠키, 인증 토큰 또는 URL 파라미터에서 나타납니다. 취약점 악용 시 기밀성이 완전히 상실될 수 있으며, 위조된 암호화 블록 생성을 통해 무결성 침해로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음(High) / 치명적(Critical)* |

> *세션 관리, 신원 토큰 또는 개인정보(PII) 암호화와 관련된 취약점인 경우 위험도는 '치명적'으로 간주됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### 민감한 데이터에 CBC 모드의 블록 암호가 사용됩니까?
- [ ] 아니요 — 애플리케이션이 인증된 암호화(Authenticated Encryption, AES-GCM/ChaCha20-Poly1305)를 사용함  
- [ ] 예 — 애플리케이션이 블록 암호를 사용하지만 패딩이 필요하지 않음 (예: 스트림 암호 또는 CTR 모드)  
- [ ] 예 — 애플리케이션이 CBC 모드를 사용하며 패딩이 적용됨  

### 서버가 부가 채널을 통해 패딩 유효성을 노출합니까?
- [ ] 아니요 — 서버가 일관된 오류 메시지와 응답 시간을 반환함  
- [ ] 예 — 서버가 패딩 오류에 대해 서로 다른 HTTP 상태 코드(예: 200 vs 500)를 반환함  
- [ ] 예 — 서버가 특정 오류 메시지(예: "Invalid Padding", "Decryption failed")를 반환함  
- [ ] 예 — 복호화 실패 시 서버에서 측정 가능한 시간 차이가 발생함  

### 암호문의 자동 복호화가 가능합니까?
- [ ] 아니요 — 부가 채널이 악용하기에 충분히 안정적이지 않음  
- [ ] 예 — 암호문의 일부(마지막 몇 블록)를 복호화할 수 있음  
- [ ] 예 — `PadBuster`와 같은 도구를 사용하여 전체 평문 복구가 가능함  

### 공격자가 비트 플리핑(Bit-flipping)을 수행하거나 유효한 암호문을 위조할 수 있습니까?
- [ ] 아니요 — 복호화 전 무결성 검사(HMAC)가 수행됨  
- [ ] 예 — 무결성 검사가 없으나, 페이로드(Payload) 구성이 불가능함  
- [ ] 예 — 임의의 암호문 구성이 가능하여 권한 상승(Privilege Escalation) 또는 데이터 주입(Data Injection)이 가능함  

---