## WSTG-CLNT-03 — HTML インジェクション (HTML Injection)

HTML インジェクション (HTML Injection) は、アプリケーションがブラウザでレンダリングする前にユーザー提供の入力を適切にサニタイズ (Sanitize) しなかった場合に発生し、攻撃者がウェブページの ドキュメントオブジェクトモデル (Document Object Model (DOM)) に任意の HTML タグを注入することを可能にします。クロスサイトスクリプティング (Cross-Site Scripting (XSS)) と似ていますが、HTML インジェクションの主な目的は、ページの視覚的な表現を操作したり、悪意のあるフォームや欺瞞的なコンテンツを注入してフィッシング (Phishing) 攻撃を容易にしたりすることです。攻撃者は、検索キーワード、ユーザープロフィールのフィールド、エラーメッセージなど、UI に反映される パラメータ (Parameters) を標的にして、被害者を騙して機密性の高い 認証情報 (Credentials) を開示させたり、許可されていないサードパーティのリンクを操作させたりします。この脆弱性は、アプリケーションのユーザーインターフェースの整合性に対する重大なリスクを意味し、より複雑な ソーシャルエンジニアリング (Social Engineering) や セッション (Session) 関連の攻撃の予兆となる可能性があります。


| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **テストステータス** | 未実施 (Not Performed) |
| **深刻度** | 低 / 中 (Low / Medium) |

**リファレンス:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### ユーザーが制御可能な入力は、適切な HTML エンコーディング (HTML Encoding) なしで DOM に反映されていますか？
- [ ] いいえ — 反映されるすべての入力は、レンダリング前に厳密に HTML エンコードされています  
- [ ] はい — 一部の文字はエンコードされていますが、特定のタグに対するバイパスが可能です  
- [ ] はい — 入力がそのまま反映されており、HTML インジェクションが可能です  

### ページのレイアウトを変更するために、構造的な HTML 要素を注入できますか？
- [ ] いいえ — アプリケーションは `<div>`, `<table>`, `<iframe>` などの構造的なタグをフィルタリングまたは除去しています  
- [ ] はい — 構造的要素を注入できますが、UI に大きな影響はありません  
- [ ] はい — 注入されたタグにより、重大な UI の改ざん (Defacement) が可能です  

### フォームや悪意のあるリンクのような、機能的なフィッシング要素を注入することは可能ですか？
- [ ] いいえ — `<form>`, `<input>`, `<a>` タグは効果的にブロックまたはサニタイズされています  
- [ ] はい — ユーザーを外部サイトにリダイレクトさせるための悪意のあるリンクを注入できます  
- [ ] はい — 認証情報を収集するための機能的な `<form>` 要素を注入できます *(中)*  

### セキュリティヘッダーまたは コンテンツセキュリティポリシー (Content Security Policy (CSP)) は、インジェクションの影響を軽減していますか？
- [ ] はい — 制限的な CSP が導入されており、`form-action` や `frame-src` が適用されています  
- [ ] いいえ — セキュリティヘッダーは存在しますが、CSP が無効であるか、unsafe-inline やワイルドカードが含まれています  
- [ ] いいえ — CSP や関連するセキュリティヘッダーが有効になっていません  

---