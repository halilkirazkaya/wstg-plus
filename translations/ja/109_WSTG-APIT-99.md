## WSTG-APIT-99 — GraphQLのテスト (Testing GraphQL)

GraphQLは、クライアントが特定のデータ構造を要求できるようにするAPI向けのクエリ言語（Query Language）ですが、設定ミス（Misconfigurations）によって情報漏えい（Information Disclosure）やリソース枯渇（Resource Exhaustion）などの重大なセキュリティリスクを招くことがよくあります。脆弱性は通常、有効化されたイントロスペクション（Introspection）、クエリの深度や複雑性の制限の欠如、およびリゾルバ（Resolver）関数内におけるオブジェクトレベルの認可の不備（BOLA: Broken Object-Level Authorization）から発生します。攻撃者は、イントロスペクションを使用してスキーマ（Schema）全体をマッピングしたり、循環フラグメント（Circular Fragments）を利用してサービス拒否（DoS: Denial of Service）を引き起こしたり、ネストされたクエリを通じて権限のないフィールドにアクセスして認可をバイパス（Bypass）したりすることで、これらのエンドポイント（Endpoints）を悪用します。テストでは、`/graphql`、`/v1/graphql`、または `/graphiql` エンドポイントに焦点を当て、実装が厳格なアクセス制御（Access Control）とリソース管理を強制していることを確認します。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **テストステータス** | 未実施 |
| **深刻度** | 高 / 緊急* |

> *オブジェクトレベルの認可の不備（BOLA）により、PII（個人を特定できる情報）への不正アクセスや管理用ミューテーション（Mutations）が可能になる場合、深刻度は「緊急（Critical）」となります。

**参考資料:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**ツール:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### GraphQLのイントロスペクションシステムは有効化されていますか？
- [ ] いいえ — イントロスペクションは無効化されており、スキーマのマッピングは不可能です。
- [ ] はい — イントロスペクションは有効化されていますが、管理者認証が必要です。
- [ ] はい — イントロスペクションは有効化されており、公開されています *(高)*

### クエリに対してリソース制限（深度および複雑性）は強制されていますか？
- [ ] はい — 厳格なクエリ深度および複雑性の制限が適用され、強制されています。
- [ ] はい — 制限は適用されていますが、エイリアス（Aliases）やフラグメントを使用してバイパス可能です。
- [ ] いいえ — 制限は適用されておらず、再帰的なクエリやDoSが可能です。

### リゾルバにオブジェクトレベルの認可の不備（BOLA）は存在しますか？
- [ ] いいえ — すべてのクエリに対して、フィールドおよびオブジェクトレベルで認可（Authorization）が適用されています。
- [ ] はい — 一部のフィールドには認可が適用されていますが、機密性の高いオブジェクトが露出しています。
- [ ] はい — 認可が適用されておらず、ID操作（ID Manipulation）を通じて任意のオブジェクトへのアクセスが可能です。

### GraphQLのミューテーションは不正使用から適切に保護されていますか？
- [ ] いいえ — スキーマ内にミューテーション（Mutations）は存在しません。
- [ ] はい — ミューテーションには有効な認証と厳格な認可チェックが必要です。
- [ ] はい — 未認証ユーザーによるミューテーションが可能、あるいは認可が欠如しています。

### エラーメッセージから機密性の高い実装やスキーマの詳細が漏えいしていませんか？
- [ ] いいえ — エラーメッセージは一般的（Generic）であり、内部ロジックを明らかにしません。
- [ ] はい — エラーメッセージにより、基盤となるデータベースの型や「Did you mean...?」による提案が露出しています。
- [ ] はい — レスポンス内に完全なスタックトレース（Stack Traces）や機密性の高い設定データが露出しています。

---