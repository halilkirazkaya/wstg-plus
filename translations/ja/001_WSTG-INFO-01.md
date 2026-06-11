## WSTG-INFO-01 — 情報漏えいに関する検索エンジンのディスカバリーと偵察の実施

検索エンジンによるディスカバリー（Search Engine Discovery）とは、公開されている検索エンジン、キャッシュされたページ、およびインデックス作成サービスを活用して、対象組織が意図せず公開してしまった情報を特定することです。攻撃者は、高度な検索演算子であるグーグルドーク（Google Dorks）やサードパーティのインデックス作成サービスを使用して、本来公開されるべきではない機密ファイル、ディレクトリ、ログインポータル、エラーメッセージ、およびメタデータ（Metadata）を探索します。この偵察（Reconnaissance）フェーズは、ターゲットと直接対話する必要がないため、事実上検出不可能でありながら、公開された管理パネル、設定ファイル、データベースダンプ、内部ドキュメントなどの価値の高いエントリポイントを明らかにする可能性があるため、非常に重要です。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **テストステータス** | 未実施 |
| **深刻度** | 中 |

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**ツール:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### 機密ファイルやディレクトリが検索エンジンにインデックスされていますか？
- [ ] いいえ — 検索結果に機密コンテンツは見つかりませんでした  
- [ ] はい — 設定ファイル、バックアップ、または内部ドキュメントがインデックスされています *(重大)*  

### Google Dorkingによって、公開された管理インターフェースやログインインターフェースが判明しますか？
- [ ] いいえ — 管理パネルやログインページはインデックスされていません  
- [ ] はい — 管理パネルはインデックスされていますが、強力な多要素認証（MFA）が必要です  
- [ ] はい — 管理パネルがインデックスされており、十分な制御なしにパブリックにアクセス可能です  

### 削除済みの機密データが、ページのキャッシュバージョンによって公開されていますか？
- [ ] いいえ — キャッシュされたスナップショットに機密データは見つかりませんでした  
- [ ] はい — キャッシュされたページにクレデンシャル、内部IPアドレス、または機密情報が含まれています  

### `robots.txt` ファイルが意図せず機密性の高いパスを公開していませんか？
- [ ] いいえ — `robots.txt` は機密性の高いディレクトリ構造を明らかにしていません  
- [ ] はい — `robots.txt` に攻撃者の偵察を助ける機密性の高いパスがリストされています  
- [ ] `robots.txt` ファイルが存在しません  

### サードパーティサービス（Shodan, Censys, Wayback Machine）によって、過去または現在の露出が判明していますか？
- [ ] いいえ — サードパーティのインデックスサービスから重要な発見はありませんでした  
- [ ] はい — 過去のスナップショットやサービススキャンにより、機密性の高いメタデータやサービスバナーが判明しました  

---