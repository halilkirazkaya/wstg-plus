## WSTG-CRYP-04 — 취약한 암호화 프리미티브(Weak Cryptographic Primitives) 점검

취약한 암호화 프리미티브는 MD5, SHA-1, DES 또는 RC4와 같이 충돌 공격(Collision attacks), 빈도 분석(Frequency analysis) 또는 무차별 대입 암호 해독(Brute-force decryption)에 취약한 오래되거나 안전하지 않은 알고리즘을 사용하는 것을 의미합니다. 이러한 약점은 일반적으로 애플리케이션 백엔드 내의 비밀번호 해싱(Password hashing), 저장 데이터 암호화(Data encryption at rest), 세션 토큰 생성(Session token generation) 또는 디지털 서명 검증(Digital signature verification)에서 발생합니다. 공격자의 관점에서 이러한 프리미티브를 식별하면 가로챈 해시를 크래킹(Cracking)하여 평문 비밀번호를 획득하거나 인증 토큰을 위조하는 등 데이터 무결성 및 기밀성(Data integrity and confidentiality)을 침해할 수 있습니다. 현대적인 애플리케이션은 암호 해독(Cryptanalysis)에 대한 강력한 보호를 보장하기 위해 Argon2, Bcrypt 또는 AES-GCM과 같이 업계 표준이며 충돌 저항성이 있고 엔트로피가 높은 프리미티브를 사용해야 합니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 높음(High) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### MD5 또는 SHA-1과 같이 권장되지 않는 해싱 알고리즘(Hashing algorithms)이 민감 데이터 저장에 사용됩니까?
- [ ] 아니요 — 애플리케이션은 Argon2 또는 Bcrypt와 같은 현대적인 충돌 방지 알고리즘을 사용함  
- [ ] 예 — 권장되지 않는 알고리즘이 사용되지만, 보안에 민감하지 않은 무결성 검사용으로**만** 사용됨  
- [ ] 예 — 비밀번호 또는 민감한 토큰에 권장되지 않는 알고리즘이 **사용됨** *(High)*  

### DES, 3DES 또는 RC4와 같이 취약한 대칭 암호화 암호(Symmetric encryption ciphers)가 현재 사용되고 있습니까?
- [ ] 아니요 — GCM 또는 CBC와 같은 안전한 모드의 AES(128비트 이상)가 사용됨  
- [ ] 예 — 하위 호환성(Backward compatibility)을 위해 레거시 암호가 **활성화**되어 있지만 기본값은 아님  
- [ ] 예 — 레거시 암호가 저장 데이터 또는 전송 중인 데이터를 보호하는 주요 방법임  

### 애플리케이션이 자체 제작("Roll-your-own") 또는 비표준 암호화 로직을 구현합니까?
- [ ] 아니요 — 애플리케이션은 표준적이고 피어 리뷰를 거친 암호화 라이브러리를 엄격히 준수함  
- [ ] 예 — 사용자 정의 래퍼가 존재하지만 기본 프리미티브는 업계 표준을 **따름**  
- [ ] 예 — 독점적인 암호화 알고리즘 또는 사용자 정의 로직이 민감한 데이터에 **적용됨** *(Critical)*  

### 암호화 솔트(Salts) 및 초기화 벡터(Initialization Vectors, IVs)가 모범 사례에 따라 관리됩니까?
- [ ] 예 — 솔트는 사용자당 고유하며 IV는 모든 작업에 대해 예측 불가능함  
- [ ] 예 — 솔트가 사용되지만 모든 레코드에서 고유하지는 **않음**  
- [ ] 아니요 — 솔트가 정적이거나 하드코딩되어 있거나 없으며, 여러 세션에서 IV가 **재사용됨**  

### 비대칭 키(Asymmetric keys, RSA, Diffie-Hellman)의 비트 길이가 현대 표준에 충분합니까?
- [ ] 예 — RSA/DH 키가 2048비트 이상이거나 타원 곡선 암호(ECC)가 사용됨  
- [ ] 아니요 — 키가 2048비트 미만이지만 우회(Bypass)가 단순하지는 않음  
- [ ] 아니요 — 키가 1024비트 이하로, 인수분해 또는 연산 공격(Factorization or computational attacks)에 **취약함**  

---