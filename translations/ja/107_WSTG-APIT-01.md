## WSTG-APIT-01 — API偵察 (API Reconnaissance)

API偵察 (API Reconnaissance) は、エンドポイント、サポートされているメソッド、および基盤となるデータ構造を明らかにするために、APIの攻撃対象領域 (Attack Surface) を特定しマッピングする体系的なプロセスです。攻撃者はこのフェーズを実行して、未定義のAPI (Shadow API)、レガシーな脆弱性を持つ非推奨バージョン、および内部のビジネスロジック (Business Logic) を露呈させる Swagger や OpenAPI 定義などの公開されたドキュメントファイルを探索します。予測可能な URL パターン、クライアントサイドの JavaScript ファイル、およびディレクトリに対する総当たり攻撃 (Directory Brute Force) の結果を分析することで、攻撃者は API の包括的なマップを構築し、オブジェクトレベルの認可の不備 (Broken Object Level Authorization - BOLA) やマスアサインメント (Mass Assignment) 攻撃の標的を特定できます。この偵察では通常、`/api/v1/`、`/swagger.json`、`/graphql` などの一般的なパスを対象とし、アプリケーションのバックエンド機能のロードマップを取得します。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **テストステータス** | 未実施 (Not Performed) |
| **深刻度** | 情報 (Informational) / 低 (Low) |

**参考文献:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**ツール:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### APIドキュメントファイル (Swagger, OpenAPI, WSDL) は公開されていますか？
- [ ] いいえ — APIドキュメントはアクセス不能、または認証 (Authentication) によって制限されています。
- [ ] はい — ドキュメントはアクセス可能ですが、有効な認証情報が必要です。
- [ ] はい — APIドキュメント (例: `/swagger-ui.html`) は、認証なしで**パブリックにアクセス可能**です。

### 未定義または「シャドウ (Shadow)」APIエンドポイントは、ファジング (Fuzzing) によって発見可能ですか？
- [ ] いいえ — ディレクトリおよびエンドポイントのファジングによって、未定義のパスは見つかりませんでした。
- [ ] はい — 未定義のエンドポイントが見つかりましたが、認可 (Authorization) なしではアクセスできません。
- [ ] はい — 未定義のエンドポイントが見つかり、有効な認可なしで**アクセス可能**です。

### アプリケーションは複数のAPIバージョン (例: /v1/, /v2/, /beta/) を公開していますか？
- [ ] いいえ — 現在の堅牢化されたAPIバージョンのみがアクセス可能です。
- [ ] はい — 旧バージョンが存在しますが、現在のバージョンと同じセキュリティ制御が実装されています。
- [ ] はい — 旧バージョン (例: `/v1/`) がアクセス可能であり、新しいバージョンのセキュリティ制御が**欠落**しています。

### クライアントサイドのリソース (JavaScript/モバイルアプリ) は、APIエンドポイントの構造やキーを漏洩させていますか？
- [ ] いいえ — クライアントサイドのコードには、ハードコードされたAPIパスや機密性の高いキーは含まれていません。
- [ ] はい — クライアントサイドのコードにAPIエンドポイントのマップが含まれていますが、機密性の高いキーは含まれていません。
- [ ] はい — APIキー、シークレット、または内部限定のエンドポイントURLが、クライアントサイドのリソースに**ハードコードされています**。

### 既知のエンドポイントにおいて、隠しパラメータやヘッダーは発見可能ですか？
- [ ] いいえ — パラメータのファジング (例: `Arjun` を使用) では、隠し入力は見つかりませんでした。
- [ ] はい — 隠しパラメータ (例: `debug=true`, `admin=1`) が見つかりましたが、機能していません。
- [ ] はい — 隠しパラメータが見つかり、アプリケーションの動作を変更するために**使用可能**です。

---