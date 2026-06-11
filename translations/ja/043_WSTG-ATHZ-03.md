## WSTG-ATHZ-03 — 権限昇格のテスト (Testing for Privilege Escalation)

権限昇格（Privilege Escalation）は、攻撃者が脆弱性を悪用して、より高い権限レベルのユーザーや別のアイデンティティに限定されたリソースまたは機能にアクセスした際に発生します。垂直方向の権限昇格（Vertical escalation）では、一般ユーザーが管理者機能へのアクセスを試みます。一方、水平方向の権限昇格（Horizontal escalation）では、同一の権限レベルを持つ別のユーザーに属するデータへのアクセスが行われます。これらの欠陥は通常、不適切に実装されたアクセス制御リスト（ACLs）、不適切な直接オブジェクト参照（IDOR）、またはセッションやアイデンティティトークン内のパラメータ改ざん（Parameter tampering）によって顕在化します。攻撃者の視点からは、数値IDの操作、クッキー（Cookies）やJWT（JSON Web Tokens）内のロールベースのパラメータ変更、あるいはサーバー側の認可チェックが不足している隠しAPIエンドポイント（API endpoints）への直接呼び出しによって達成されることが一般的です。


| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **テストステータス** | 未実施 (Not Performed) |
| **深刻度** | 高 (High) / 致命的 (Critical) |

**リファレンス:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**ツール:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### アプリケーションは複数の権限レベルまたはマルチテナント (Multi-tenancy) を実装していますか？
- [ ] いいえ — アプリケーションは単一ユーザーまたは単一ロールのシステムです  
- [ ] はい — 複数のロールまたはテナントが定義されており、認可（Authorization）テストが必要です  

### 水平方向の権限昇格（同レベルのアクセス）は可能ですか？
- [ ] いいえ — すべてのリソース識別子およびセッションオーナーに対して認可チェックが適用されています  
- [ ] はい — IDORまたはパラメータ改ざんを通じて、他のユーザーのデータへのアクセスが可能です  
- [ ] はい — データの漏洩は可能ですが、他のユーザーのデータの変更は不可能です  

### 垂直方向の権限昇格（低権限から高権限）は可能ですか？
- [ ] いいえ — 管理用エンドポイントはロールベースのアクセス制御 (RBAC) を厳格に適用しています  
- [ ] はい — 低権限ユーザーが直接URLにアクセスすることで、管理機能にアクセスできます  
- [ ] はい — ロールに関連するヘッダー、クッキー、またはJWTクレームを操作することで、管理機能にアクセスできます  

### マスアサインメント (Mass Assignment) またはパラメータ汚染 (Parameter Pollution) を介してユーザーロールや権限を変更できますか？
- [ ] いいえ — ロールベースのフィールドは厳格に読み取り専用であり、ユーザーによる変更は不可能です  
- [ ] はい — プロフィールの更新や登録時に隠しパラメータ（例：`role=admin`）を含めることで、ユーザー権限を昇格させることができます  

### 認可チェックは、すべてのAPIバージョンおよびHTTPメソッドにわたって一貫して適用されていますか？
- [ ] はい — 制御はすべてのバージョンおよびメソッドにわたって一貫して適用されています  
- [ ] いいえ — 旧版のAPIバージョン（例：`/v1/`）や特定のHTTPメソッド（例：`PUT`, `DELETE`, `PATCH`）において認可がバイパスされます  
- [ ] いいえ — 管理機能はUI上では隠されていますが、APIレベルではすべてのユーザーに対して有効になっています  

---