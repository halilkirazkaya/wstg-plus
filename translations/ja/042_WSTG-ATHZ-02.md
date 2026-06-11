## WSTG-ATHZ-02 — 認可スキーマのバイパス (Bypassing Authorization Schema) に関するテスト

認可スキーマのバイパスは、アプリケーションがアクセス制御を適切に適用できず、攻撃者が意図された権限以外のリソースや機能にアクセスできる場合に発生します。この脆弱性は通常、攻撃者が自分と同じ権限レベルの別のユーザーに属するデータにアクセスする「横方向の権限昇格 (Horizontal Privilege Escalation)」や、低権限ユーザーが管理者権限を取得する「縦方向の権限昇格 (Vertical Privilege Escalation)」として現れます。攻撃者は、リソース ID (Resource IDs) などのリクエストパラメータの操作、セッショントークン (Session tokens) の変更、または `X-Original-URL` のような HTTP ヘッダー (HTTP headers) を利用してパスベースの制限を回避することで、これらの不備を悪用します。堅牢な認可を確保することは、データの機密性を維持し、システム全体を侵害する可能性のある不正な管理アクションを防止するために不可欠です。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **テストステータス** | 未実施 |
| **深刻度** | 高 / 緊急 |

**リファレンス:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**ツール:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### 同じロールのユーザー間において、横方向の権限昇格は防止されていますか？
- [ ] はい — すべてのリソースリクエストに認可チェックが適用されており、バイパスは不可能です。
- [ ] はい — 認可チェックは適用されていますが、安全でない直接オブジェクト参照 (IDOR) やパラメータ改ざんによってバイパス可能です。
- [ ] いいえ — ユーザーはリソース ID を変更するだけで、他のユーザーに属するデータにアクセスできます *(高)*。

### 低権限ユーザーに対する縦方向の権限昇格は防止されていますか？
- [ ] いいえ — アプリケーションに権限レベルが 1 つしかありません。
- [ ] はい — 低権限ユーザーは管理機能にアクセスできません。
- [ ] はい — 管理機能は UI 上では非表示ですが、直接 URL を指定することでアクセス可能です。
- [ ] いいえ — 低権限ユーザーはロールや権限を操作することで、管理アクションを実行可能です *(緊急)*。

### HTTP メソッドの改ざん (HTTP verb tampering) を使用して、認可をバイパスできますか？
- [ ] いいえ — 使用される HTTP メソッドに関係なく、アクセス制御が強制されています。
- [ ] はい — メソッドの変更 (例: `GET` から `POST` または `PUT` へ) により、認可チェックがバイパスされます。

### 管理用または制限されたエンドポイントに、認証なしでアクセス可能ですか？
- [ ] いいえ — すべての機密性の高いエンドポイントには、有効なセッションと適切な権限が必要です。
- [ ] はい — 攻撃者が直接 URL を知っている場合、一部の管理用エンドポイントにアクセス可能です。
- [ ] はい — 機密性の高い機能への未認証アクセスが可能です *(緊急)*。

### HTTP ヘッダーによって、パスベースの認可を回避することは可能ですか？
- [ ] いいえ — アプリケーションはヘッダーベースのバイパスの影響を受けません。
- [ ] はい — `X-Original-URL`、`X-Rewrite-URL`、または `X-Forwarded-For` などのヘッダーを使用して、アクセス制御をバイパス可能です。

---