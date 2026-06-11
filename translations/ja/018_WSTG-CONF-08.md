## WSTG-CONF-08 — RIAクロスドメインポリシーのテスト

リッチインターネットアプリケーション (RIA) のクロスドメインポリシーには、`crossdomain.xml` や `clientaccesspolicy.xml` といったファイルが含まれます。これらは Webクライアント、具体的には Adobe Flash や Microsoft Silverlight がクロスオリジンリクエストをどのように処理するかを定義します。これらのファイルは、同一生成元ポリシー (Same-Origin Policy - SOP) を明示的にオーバーライドし、どの外部ドメインがアプリケーションと相互作用し、そのレスポンスを読み取ることを許可されるかを決定するため、セキュリティ上極めて重要です。`domain` 属性にワイルドカードを使用するなどの過度に寛容な設定は、攻撃者がサードパーティサイトに悪意のある RIA オブジェクトをホストし、機密データを流出させたり、認証済みユーザーに代わってアクションを実行したりすることを可能にします。ペネトレーションテスターは通常、これらのファイルを Webルートで探し、サードパーティのオリジンに与えられている信頼レベルを評価し、潜在的なクロスオリジンデータ漏洩を特定します。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **テストステータス** | 未実施 |
| **深刻度** | 中 / 高* |

> *アプリケーションが、寛容なポリシーを介して流出する可能性のある機密ユーザーデータやセッション識別子を扱う場合、深刻度は「高」になります。

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### クロスドメインポリシーファイル（`crossdomain.xml` または `clientaccesspolicy.xml`）は Webルートに存在するか？
- [ ] いいえ — ルートディレクトリにファイルが見つからない  
- [ ] はい — `crossdomain.xml` (Flash) が存在する  
- [ ] はい — `clientaccesspolicy.xml` (Silverlight) が存在する  
- [ ] はい — 両方の RIA ポリシーファイルが存在する  

### `allow-access-from` ドメイン属性は信頼できるオリジンに制限されているか？
- [ ] いいえ — ファイルは存在するが `allow-access-from` エントリが含まれていない  
- [ ] はい — 特定の信頼できるドメインが明示的にホワイトリストに登録されており、バイパスは不可能である  
- [ ] はい — ドメインはホワイトリストに登録されているが、信頼できないサードパーティやサブドメインが含まれている  
- [ ] はい — ワイルドカード `*` が使用されており、あらゆるオリジンからのアクセスを許可している *(Critical)*  

### ポリシーは HTTPS リソースに対して HTTP 経由の不安全な通信を許可しているか？
- [ ] いいえ — `secure="true"` が設定されている（またはデフォルト）、HTTP オリジンによる HTTPS データへのアクセスを防止している  
- [ ] はい — `secure="false"` が明示的に設定されており、中間者攻撃 (MITM) を行う攻撃者が安全なデータを流出させることを許可している  

### クロスドメイン設定において HTTP ヘッダーが過剰に許可されていないか？
- [ ] いいえ — `allow-http-request-headers-from` が制限されている、または有効になっていない  
- [ ] はい — 信頼できるドメインに対して特定のヘッダーが許可されている  
- [ ] はい — ヘッダーにワイルドカード `*` が使用されており、攻撃者が任意のドメインからカスタムヘッダーを送信することを許可している  

### `site-control`（マスターポリシー）設定は制限的か？
- [ ] いいえ — `permitted-cross-domain-policies` が `none` または `master-only` に設定されている  
- [ ] はい — ポリシーにより、サーバー上の他のファイルが独自のルールを定義することを許可している (`all`)  

---