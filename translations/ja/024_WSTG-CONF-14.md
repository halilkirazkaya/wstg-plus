## WSTG-CONF-14 — その他の HTTP セキュリティヘッダー設定不備のテスト

HTTP セキュリティヘッダーの設定不備のテストでは、一般的なウェブベースの攻撃ベクトルに対する不可欠な防御を提供する、防御用ヘッダーの存在とその正しい実装を検証します。これらのヘッダーは、コードレベルの根本的な脆弱性を修正するものではありませんが、その欠如は中間者攻撃 (Man-in-the-middle, MITM 攻撃)、MIME スニッフィング (MIME-sniffing) の悪用、およびリファラ (Referrer) を介した意図しないクロスオリジンデータ漏えい (Cross-origin data leakage) のリスクを大幅に増大させます。攻撃者の視点からは、セキュリティヘッダーの欠如は、十分にハードニング（堅牢化）されていない環境であることを示す視認性の高い指標であり、セッションハイジャック (Session hijacking) や情報の窃取 (Information exfiltration) といった、より複雑な攻撃チェーンの足掛かりとなることがよくあります。これらの設定は通常、サーバーレベルで評価され、API エンドポイントや静的リソースを含む、すべてのレスポンスタイプにおいて一貫して適用される必要があります。

| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **テストステータス** | 未実施 |
| **深刻度** | 低 / 中 |

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### セキュリティヘッダー存在確認監査
| ヘッダー | 存在 | 欠如 | 推奨設定 |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` または `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (機能固有、例: `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### アプリケーション全体でセキュリティヘッダーはどのように適用されていますか？
- [ ] はい — ヘッダーは中央のロードバランサまたはリバースプロキシを介してグローバルに適用されており、上書きすることは**できない**  
- [ ] はい — ヘッダーはメインドキュメントのレスポンスには適用されているが、API レスポンスや静的アセットには**欠如している**  
- [ ] いいえ — ヘッダーの適用が一貫していない、またはアプリケーションドメイン全体で**欠如している**  

### Strict-Transport-Security (HSTS) ヘッダーは正しく設定されていますか？
- [ ] はい — `max-age` が長期間（例：2年）に設定され、`includeSubDomains` が**有効**になっている  
- [ ] はい — `max-age` は設定されているが、`includeSubDomains` が**無効**であり、サブドメインが脆弱な状態にある  
- [ ] いいえ — HSTS ヘッダーが**存在しない**、または `max-age` が 0 に設定されており、プロトコルのダウングレード攻撃 (Protocol downgrade attacks) を許容している  

### Referrer-Policy は機密性の高い URL パラメータの漏えいを防止していますか？
- [ ] はい — ポリシーが `no-referrer` または `same-origin` に設定されている（*最も安全*）  
- [ ] はい — ポリシーが `strict-origin-when-cross-origin` に設定されている  
- [ ] いいえ — ポリシーが**設定されていない**、または `unsafe-url` に設定されており、リファラヘッダー内でトークンや機密性の高い個人を特定できる情報 (Personally Identifiable Information, PII) が漏えいする可能性がある  

### X-Content-Type-Options ヘッダーは MIME スニッフィングを防止していますか？
- [ ] はい — `nosniff` ディレクティブが**存在**し、正しく実装されている  
- [ ] いいえ — ヘッダーが**欠如**しており、ブラウザが実行可能ではないファイル（例：画像）をスクリプトとして実行してしまう可能性がある  

---