## WSTG-SESS-10 — JSON Web Token (JWT) のテスト

JSON Web Token (JWT) は、ステートレスなセッション管理やアイデンティティの伝搬に一般的に使用されますが、そのセキュリティは署名アルゴリズムの正しい実装と、クレーム（Claims）の厳格な検証に完全に依存しています。攻撃者は、"none" アルゴリズムの不備を悪用したり、脆弱な HS256 シークレットキーを総当たり攻撃（Brute Force）したり、非対称公開鍵を対称 HMAC シークレットとして使用させるアルゴリズム切り替え攻撃（Algorithm-switching attacks）を実行したりすることで、認証のバイパスを頻繁に試みます。これらの脆弱性は通常、セッションクッキーや認可（Authorization）ヘッダーに現れ、トークンの暗号学的整合性を損なうことなく `role` や `user_id` などのクレームを改ざんすることで、権限昇格（Privilege Escalation）や任意のユーザーへのなりすましを可能にする可能性があります。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **テストステータス** | 未実施 |
| **深刻度** | 高 / 緊急 |

**参考文献:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**ツール:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### "none" アルゴリズムがサーバーによって受け入れられますか？
- [ ] いいえ — サーバーはヘッダーに `alg: none` を含むトークンを**拒否**します  
- [ ] はい — サーバーは `alg: none` と空の署名を持つトークンを**受け入れます** *(緊急)*  

### JWT 署名はどのように検証され、総当たり攻撃（Brute Force）から保護されていますか？
- [ ] はい — 強固な非対称鍵 (RS256/ES256) または高エントロピーの HMAC シークレットが使用されています  
- [ ] はい — HS256 が使用されていますが、エントロピーが低いかデフォルト値であるため、シークレットが**総当たり攻撃可能**です  
- [ ] いいえ — 署名の検証が**適用されていない**か、アルゴリズムの切り替え (RS256 から HS256) によって**バイパス可能**です  

### 標準的なクレーム (exp, nbf, iat) はバックエンドで厳格に適用されていますか？
- [ ] はい — `exp` (有効期限) が存在し、バイパス**できません**  
- [ ] はい — `exp` は存在しますが、サーバーは検証時にそれを**強制しません**  
- [ ] いいえ — 有効期限クレームが**欠落している**か無視されており、無制限のセッションリプレイ（Session Replay）が可能です  

### 実装において、ヘッダーベースのキーインジェクション (kid, jku, x5u) は防止されていますか？
- [ ] はい — ヘッダーはサニタイズ（Sanitized）されており、信頼された内部のキー/ソースのみが**許可**されています  
- [ ] いいえ — `kid` (Key ID) ヘッダーに、ディレクトリトラバーサル（Directory Traversal）や SQL Injection（SQL インジェクション）の脆弱性があり、既知のファイルを指すことができます  
- [ ] いいえ — `jku` または `x5u` ヘッダーを攻撃者が制御する URL に向けることで、悪意のあるキーをロード**できます**  

---