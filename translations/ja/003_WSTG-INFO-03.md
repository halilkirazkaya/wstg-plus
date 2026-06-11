## WSTG-INFO-03 — 情報漏洩に関するウェブサーバーのメタファイルの確認 (Review Webserver Metafiles for Information Leakage)

`robots.txt`、`sitemap.xml`、`security.txt` などのウェブサーバーのメタファイルは、クローラー (Crawlers) を誘導したり管理用メタデータを提供したりすることを目的としていますが、意図せず機密性の高いディレクトリ構造や隠れたエンドポイント (Endpoints) を明らかにしてしまうことがよくあります。攻撃者はこれらのファイルを分析してアプリケーションのアタックサーフェス (Attack Surface / 攻撃対象領域) を列挙し、リンクされていない管理パネル、バックアップディレクトリ、または開発環境を指し示していることが多い「Disallow」ディレクティブを特定します。検索エンジンの指示以外にも、`.well-known/` ディレクトリ内のファイルや `humans.txt` などのファイルは、テクノロジースタック (Technology Stack)、開発者情報、または組織のセキュリティ連絡先を開示する可能性があります。これらのファイルを系統的にレビューすることで、テスターは強引なブルートフォース (Brute Force / 総当たり攻撃) による探索を行うことなく、構造的および技術的な洞察を得ることができます。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **テストステータス** | 未実施 (Not Performed) |
| **深刻度** | 低 (Low) / 情報 (Informational)* |

> *メタファイルが未認証の管理用インターフェースや機密性の高い設定バックアップを明らかにしている場合、深刻度は「中 (Medium)」になります。

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### `robots.txt` ファイルが存在し、機密性の高いディレクトリを露出させていないか？
- [ ] いいえ — `robots.txt` は存在しません  
- [ ] はい — `robots.txt` は存在しますが、標準的な公開パスのみが含まれています  
- [ ] はい — `robots.txt` は機密性の高いディレクトリパスまたは管理用インターフェースを明らかにしています  
- [ ] はい — `robots.txt` はコメント内に認証情報 (Credentials) または特定のソフトウェアバージョンを明らかにしています  

### `sitemap.xml` ファイルが存在し、隠れたアプリケーション構造を明らかにしていないか？
- [ ] いいえ — `sitemap.xml` は存在しません  
- [ ] はい — `sitemap.xml` は存在しますが、公開されている URL のみをリストしています  
- [ ] はい — `sitemap.xml` は、アクセス可能なリンクされていないエンドポイントや内部専用のリソースをリストしています  

### セキュリティおよび組織のメタファイルが技術的な詳細を露出させていないか？
- [ ] いいえ — ルートディレクトリまたは `.well-known/` 内にメタファイルは見つかりませんでした  
- [ ] はい — `security.txt` または `humans.txt` は存在しますが、標準的な情報のみが含まれています  
- [ ] はい — メタファイルが内部ホスト名、開発者の身元、またはテクノロジースタックの詳細を開示しており、ソーシャルエンジニアリング (Social Engineering) やさらなる攻撃に利用される可能性があります  

### レガシーまたはフレームワーク固有のメタファイルが存在するか？
- [ ] いいえ — フレームワーク固有のファイル（例：`info.php`、`composer.json`、`package.json`）は見つかりませんでした  
- [ ] はい — `README.md`、`CHANGELOG.txt`、`.DS_Store` などのファイルは存在しますが、サニタイズ (Sanitized) されています  
- [ ] はい — フレームワークまたはバージョン固有のファイルが存在し、正確なテクノロジーのフィンガープリント (Fingerprinting) を可能にしています  

---