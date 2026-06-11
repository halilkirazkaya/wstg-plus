## WSTG-CRYP-04 — 弱加密原语测试 (Testing for Weak Cryptographic Primitives)

弱加密算法原语 (Weak Cryptographic Primitives) 涉及使用过时或不安全的算法，例如 MD5、SHA-1、DES 或 RC4，这些算法容易受到碰撞攻击 (Collision Attacks)、频率分析或暴力破解 (Brute Force) 解密。这些弱点通常出现在应用程序后端的密码哈希 (Hashing)、静态数据加密、会话令牌 (Session Token) 生成或数字签名验证中。从攻击者的角度来看，识别出这些原语可以破坏数据的完整性和机密性，例如通过破解拦截到的哈希以获取明文密码或伪造身份验证令牌。现代应用程序应转而使用符合行业标准、抗碰撞且高熵的原语，如 Argon2、Bcrypt 或 AES-GCM，以确保针对密码分析 (Cryptanalysis) 的强大防护。

| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **测试状态** | 未执行 |
| **严重程度** | 高 |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**工具：** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### 敏感数据存储是否使用了诸如 MD5 或 SHA-1 等已弃用的哈希算法？
- [ ] 否 — 应用程序使用 Argon2 或 Bcrypt 等现代抗碰撞算法  
- [ ] 是 — 使用了已弃用的算法，但**仅**用于非安全敏感的完整性检查  
- [ ] 是 — 密码或敏感令牌**使用了**已弃用的算法 *(高)*  

### 当前是否采用了 DES、3DES 或 RC4 等弱对称加密密码？
- [ ] 否 — 使用了 GCM 或 CBC 等安全模式下的 AES (128位或更高)  
- [ ] 是 — 为了向后兼容**启用了**遗留密码，但它们**不是**默认设置  
- [ ] 是 — 遗留密码是保护静态或传输中数据的主要方法  

### 应用程序是否实现了“自行开发” (Roll-your-own) 或非标准的加密逻辑？
- [ ] 否 — 应用程序严格遵守标准的、经过同行评审的加密库  
- [ ] 是 — 存在自定义封装，但底层原语**属于**行业标准  
- [ ] 是 — 对敏感数据**应用了**私有加密算法或自定义逻辑 *(严重)*  

### 加密盐 (Salts) 和初始化向量 (IVs) 是否按照最佳实践进行管理？
- [ ] 是 — 盐对每个用户都是唯一的，且每次操作的 IV 均不可预测  
- [ ] 是 — 使用了盐，但在所有记录中并**不唯一**  
- [ ] 否 — 盐是静态的、硬编码的或缺失的，且 IV 在多个会话中被**复用**  

### 非对称密钥 (RSA, Diffie-Hellman) 的位长度是否符合现代标准？
- [ ] 是 — RSA/DH 密钥为 2048 位或更高，或者使用了椭圆曲线加密 (ECC)  
- [ ] 否 — 密钥小于 2048 位，但绕过并非易事  
- [ ] 否 — 密钥为 1024 位或更小，使其**容易受到**因子分解或计算攻击的影响  

---