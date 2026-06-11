## WSTG-INFO-09 — Fingerprint Web Application

Webアプリケーションのフィンガープリント (Fingerprint Web Application) とは、特定のフレームワーク (Framework)、コンテンツ管理システム (CMS)、または基盤となるテクノロジースタック (Technology Stack) とその関連バージョンを特定するプロセスです。この偵察 (Reconnaissance) フェーズは、特定されたコンポーネントを既知の脆弱性 (CVE) や公開されているエクスプロイト (Exploit) と照らし合わせることを可能にし、攻撃対象領域 (Attack Surface) を大幅に絞り込むことができるため、非常に重要です。情報は通常、HTTPレスポンスヘッダー、特有のファイルパス、Cookie名、HTMLソースコードのメタデータ、および静的アセットの暗号化ハッシュから収集されます。テクノロジースタックを正確に特定することで、攻撃者は汎用的な自動スキャンから、バージョン固有の弱点を狙った高度に標的を絞った悪用 (Exploitation) へと移行することができます。


| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **テストステータス** | 未実施 (Not Performed) |
| **深刻度** | 低 (Low) / 情報 (Informational) |

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### HTTPレスポンスヘッダーにテクノロジースタックやバージョン番号が含まれていますか？
- [ ] いいえ — 機密性の高いヘッダーは削除されているか、汎用的な値が含まれています  
- [ ] はい — `X-Powered-By`、`Server`、`X-AspNet-Version` などのデフォルトヘッダーが存在します  
- [ ] はい — ヘッダーに、Webサーバーまたはアプリケーションフレームワークの特定の古いバージョンが含まれています  

### 固有のファイルパスや命名規則によって、アプリケーションフレームワークやCMSを特定できますか？
- [ ] いいえ — ファイルパスは難読化されており、基盤となるテクノロジーを明らかにしていません  
- [ ] はい — 一般的なディレクトリ構造（例：`/wp-content/`、`/_next/`、`/node_modules/`）が確認できます  
- [ ] はい — `robots.txt`、`humans.txt`、`sitemap.xml` などの固有のファイルにテクノロジー固有のメタデータが含まれています  

### HTMLソースコードや静的アセット内に特定のバージョン番号が公開されていますか？
- [ ] いいえ — コメントやアセットのパラメータにバージョン情報は含まれていません  
- [ ] はい — HTMLのコメントやメタタグ（例：`<meta name="generator" content="...">`）が存在します  
- [ ] はい — CSS/JSのファイル名にバージョン文字列が付与されており（例：`jquery.js?v=1.12.4`）、無効化できません  

### デフォルトのエラーページや管理インターフェースからソフトウェア環境が明らかになりますか？
- [ ] いいえ — アプリケーションはすべてのステータスコードに対してカスタマイズされた汎用的なエラーページを使用しています  
- [ ] はい — Webサーバーまたはフレームワークのデフォルトのエラーページが有効になっており、バージョンデータが漏洩しています  
- [ ] はい — 管理ログインポータル（例：`/wp-admin`、`/phpmyadmin`）にアクセス可能であり、特定できます  

### Cookieによってセッション管理フレームワークが特定されますか？
- [ ] いいえ — セッションCookieは汎用的またはカスタムの名称を使用しています  
- [ ] はい — テクノロジー固有のCookie名（例：`PHPSESSID`、`JSESSIONID`、`ASPSESSIONID`）が使用されています  

---