## WSTG-CRYP-04 — 脆弱な暗号プリミティブ (Weak Cryptographic Primitives) のテスト

脆弱な暗号プリミティブ (Weak Cryptographic Primitives) とは、衝突攻撃 (Collision Attacks)、頻度分析 (Frequency Analysis)、または 総当たり攻撃 (Brute Force) による復号に対して脆弱な、MD5、SHA-1、DES、RC4 などの旧式または安全でないアルゴリズムの使用を指します。これらの脆弱性は通常、アプリケーションのバックエンドにおけるパスワードハッシュ化 (Password Hashing)、保存データの暗号化 (Data Encryption at Rest)、セッション トークン (Session Token) 生成、またはデジタル署名の検証で発生します。攻撃者の視点では、これらのプリミティブを特定することで、傍受したハッシュを解析してプレーンテキストのパスワードを取得したり、認証トークンを偽造したりするなど、データの完全性 (Integrity) と機密性 (Confidentiality) を侵害することが可能になります。現代のアプリケーションは、暗号解読 (Cryptanalysis) に対する堅牢な保護を確実にするために、Argon2、Bcrypt、AES-GCM などの業界標準で衝突耐性があり、高エントロピー (High-Entropy) なプリミティブを利用すべきです。


| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **テストステータス** | 未実施 |
| **深刻度** | 高 (High) |

**参照先:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### 機密データの保存に MD5 や SHA-1 などの非推奨のハッシュアルゴリズムが使用されていますか？
- [ ] いいえ — アプリケーションは Argon2 や Bcrypt などの現代的な衝突耐性のあるアルゴリズムを使用している。
- [ ] はい — 非推奨のアルゴリズムが使用されているが、セキュリティ上の機密性に関わらない整合性チェック**のみ**に使用されている。
- [ ] はい — パスワードや機密トークンに対して、非推奨のアルゴリズムが**使用されている**。 *(High)*

### DES、3DES、RC4 などの脆弱な共通鍵暗号 (Symmetric Encryption) 方式が現在採用されていますか？
- [ ] いいえ — GCM や CBC などの安全なモードで AES（128ビット以上）が使用されている。
- [ ] はい — 後方互換性のためにレガシーな暗号方式が**有効化**されているが、デフォルトではない。
- [ ] はい — 保存中または転送中のデータを保護するための主要な手法として、レガシーな暗号方式が使用されている。

### アプリケーションは「独自実装 (Roll-your-own)」または非標準の暗号ロジックを実装していますか？
- [ ] いいえ — アプリケーションは標準的でピアレビュー済みの暗号ライブラリを厳格に遵守している。
- [ ] はい — カスタムラッパーが存在するが、基礎となるプリミティブは業界標準で**ある**。
- [ ] はい — 独自の暗号アルゴリズムまたはカスタムロジックが機密データに**適用されている**。 *(Critical)*

### ソルト (Salts) および初期化ベクトル (Initialization Vectors (IVs)) はベストプラクティスに従って管理されていますか？
- [ ] はい — ソルトはユーザーごとに一意であり、IV はすべての操作において予測不可能である。
- [ ] はい — ソルトは使用されているが、すべてのレコードにおいて一意では**ない**。
- [ ] いいえ — ソルトが静的、ハードコードされている、または存在せず、IV が複数のセッションで**再利用されている**。

### 公開鍵暗号 (Asymmetric Keys)（RSA、Diffie-Hellman）のビット長は、現代の標準に対して十分ですか？
- [ ] はい — RSA/DH キーが 2048 ビット以上であるか、楕円曲線暗号 (Elliptic Curve (ECC)) が使用されている。
- [ ] いいえ — キーが 2048 ビット未満であるが、バイパスは容易では**ない**。
- [ ] いいえ — キーが 1024 ビット以下であり、素因数分解や計算攻撃に対して**脆弱**である。

---