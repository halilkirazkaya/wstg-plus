## WSTG-SESS-05 — クロスサイトリクエストフォージェリ（Cross-Site Request Forgery / CSRF）のテスト

クロスサイトリクエストフォージェリ（Cross-Site Request Forgery / CSRF）は、攻撃者が被害者のブラウザを操作し、被害者が現在認証されている別のウェブサイト上で意図しない操作を実行させる脆弱性です。このエクスプロイト（Exploit）は、セッションクッキー（session cookies）や Authorization ヘッダーなどの「アンビエント（自動付随）」な認証情報を、ブラウザが送信リクエストに自動的に付加する動作を悪用します。攻撃者は通常、パスワードの変更、メールアドレスの更新、送金などの状態変化を伴う操作を標的とし、サードパーティのサイトに悪意のあるスクリプトや隠しフォームを設置します。攻撃が成功すると、ユーザーの関知や同意なしに、完全なアカウント乗っ取り（Account Takeover）や不正なデータ改ざんを招く可能性があります。


| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **テストステータス** | 未実施 |
| **深刻度** | 中 / 高* |

> *脆弱なアクションによって、アカウント乗っ取り、権限昇格、または不正な金融取引が可能な場合、深刻度は「高」となります。

**参照先:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**ツール:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### 状態変化を伴うリクエストに対して anti-CSRF トークンが実装されていますか？
- [ ] はい — すべての状態変化を伴うアクションに対して、一意で暗号的に強力なトークンが要求される  
- [ ] はい — トークンは存在するが、セッションごとに一意ではない、あるいは予測可能である  
- [ ] いいえ — anti-CSRF トークンが実装されていない  

### サーバー側での anti-CSRF トークンの検証は堅牢ですか？
- [ ] はい — サーバーはトークンを厳密に検証しており、バイパス（Bypass）は不可能である  
- [ ] はい — 検証は行われているが、トークンパラメータを削除することでバイパスが可能である  
- [ ] はい — 検証は行われているが、空のトークンやダミートークンを提供することでバイパスが可能である  
- [ ] はい — 検証は行われているが、トークンがユーザーのセッションに関連付けられていない  

### アプリケーションは、容易にバイパス可能な CSRF 保護手法に依存していませんか？
- [ ] いいえ — アプリケーションは、脆弱なヘッダーや Origin チェックのみに依存していない  
- [ ] はい — 保護が `Referer` または `Origin` ヘッダーのみに依存しており、これらは偽装または削除が可能である  
- [ ] はい — 保護が `X-Requested-With` ヘッダーのチェックに依存しており、CORS の設定ミスを通じてバイパスが可能である  

### セッションクッキーに `SameSite` 属性が設定されていますか？
- [ ] はい — すべてのセッション関連のクッキーに対して、`SameSite` が `Strict` または `Lax` に設定されている  
- [ ] いいえ — `SameSite` 属性が欠落しており、ブラウザ固有のデフォルト動作に依存している  
- [ ] いいえ — `SameSite` が明示的に `None` に設定されており、`Secure` フラグや追加の緩和策が講じられていない  

### HTTP リクエストメソッドを変更することで CSRF 保護をバイパスできますか？
- [ ] いいえ — 使用される HTTP メソッドに関係なく、保護が強制されている  
- [ ] はい — トークンの検証は `POST` リクエストでのみ行われるが、アクションは `GET` を介して実行可能である  
- [ ] はい — メソッド（例：`PUT` や `DELETE`）を変更することでトークンのチェックをバイパスできる  

---