## WSTG-INPV-05 — SQLインジェクション (SQL Injection)

SQLインジェクション (SQL Injection) は、ユーザーが提供した入力が適切なパラメータ化クエリ (Parameterized Queries) やサニタイジング (Sanitization) を行わずにSQLクエリに組み込まれた際に発生し、攻撃者がクエリロジックを操作することを可能にします。攻撃に成功すると、不正なデータの抽出 (Data Exfiltration)、認証バイパス (Authentication Bypass)、データの改ざん、そして場合によっては `xp_cmdshell` や `LOAD_FILE()` などのデータベース機能を通じたリモートコード実行 (Remote Code Execution) につながる可能性があります。この脆弱性は、一般的にログインフォーム、検索機能、REST APIパラメータ、HTTPヘッダー、およびユーザー制御の入力から動的SQLクエリを構築するあらゆるエンドポイントに影響を与えます。攻撃者の視点からは、インジェクションポイントの特定は、データベースの完全な侵害や、インフラ内での潜在的なラテラルムーブメント (Lateral Movement) への主要な入り口となります。


| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **テストステータス** | 未実施 |
| **深刻度** | クリティカル (Critical) |

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**ツール:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### ユーザーが制御可能なパラメータは、安全なデータベースアクセス手法を使用して処理されていますか？
- [ ] はい — アプリケーションはORMまたはパラメータ化クエリのみを使用しており、バイパスは**不可能**である  
- [ ] はい — パラメータ化クエリが使用されているが、エッジケース（例：`ORDER BY` 句など）によりバイパスが**可能**である  
- [ ] いいえ — パラメータ化を**行わず**、生の文字列結合を使用してSQLクエリが構築されている  

### 認証メカニズムはSQLインジェクションによって回避可能ですか？
- [ ] いいえ — ログインパラメータにインジェクションの脆弱性は**存在しない**  
- [ ] はい — 恒真式 (Tautology) ベースのペイロード（例：`' OR 1=1 --`）を使用して、認証バイパスが**可能**である  

### インバンド (In-band) またはアウトオブバンド (Out-of-band) 手法によるデータ抽出は可能ですか？
- [ ] いいえ — データ抽出は確認されなかった  
- [ ] はい — UNIONベースまたはエラーベースの抽出が**可能**である  
- [ ] はい — ブラインド (BooleanベースまたはTimeベース) の抽出が**可能**である  
- [ ] はい — アウトオブバンド (DNS/HTTP) の抽出が**可能**である  

### アプリケーションの機能に二次SQLインジェクション (Second-order SQL Injection) の脆弱性はありますか？
- [ ] いいえ — 保存されたデータは、後のクエリで取得される際も安全に処理されている  
- [ ] はい — ユーザー入力が保存され、後にバリデーションを**行わずに**安全でないSQLクエリで使用されている  

### データベースのサービスアカウント権限は、必要最小限に制限されていますか？
- [ ] はい — サービスアカウントには**最小権限** (Least Privilege) が付与されている（例：特定のテーブルやアクションのみに限定されている）  
- [ ] いいえ — サービスアカウントに高い権限が付与されている（例：`DROP TABLE`、`FILE` 権限、または `xp_cmdshell` が**有効**になっている）  

---