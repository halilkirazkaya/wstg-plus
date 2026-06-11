## WSTG-CRYP-03 — 暗号化されていないチャネルを介して送信される機密情報のテスト

暗号化されていないチャネル（通常の HTTP など）を介して機密データを送信すると、中間者攻撃 (Man-in-the-Middle (MITM)) による情報の傍受や改ざんのリスクにさらされます。この脆弱性は、通常、ウェブアプリケーションがすべてのエンドポイントに対して Transport Layer Security (TLS) を強制できていない場合に発生します。特に、レガシーコンポーネント、ログインフォーム、または HTTP から HTTPS への移行初期段階で多く見られます。攻撃者の視点からは、これにより、パブリック Wi-Fi のような侵害された、あるいは信頼できないネットワークセグメント上で、認証資格情報、セッショントークン、および個人を特定できる情報 (Personally Identifiable Information (PII)) をパッシブにスニッフィングすることが可能になります。すべての機密データが堅牢な TLS トンネル内にカプセル化されていることを確実にすることは、ユーザーインタラクションの機密性と完全性を維持し、セッションハイジャックを防止するための基本です。

| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **テストステータス** | 未実施 |
| **深刻度** | 高 / 中 |

> *認証資格情報、セッション識別子、または PII が平文で送信される場合、深刻度は「高」となります。

**参考文献:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### アプリケーションは、機密性の高いエンドポイントに対して暗号化されていない HTTP 通信を許可していますか？
- [ ] いいえ — すべてのアプリケーション通信は厳格に HTTPS へ強制されています *(最も安全)*  
- [ ] はい — 機密性の低いページは HTTP でアクセス可能ですが、機密性の高いエンドポイントは**許可されていません**  
- [ ] はい — 機密性の高いエンドポイント（ログイン、チェックアウト、プロファイルなど）が暗号化されていない HTTP でアクセス**可能です** *(高リスク)*  

### HTTP から HTTPS へのグローバルなリダイレクトは設定されていますか？
- [ ] はい — アプリケーションはすべての HTTP リクエストに対して HTTPS への 301/302 リダイレクトを実行します  
- [ ] はい — リダイレクトは特定の機密性の高いエンドポイントに対してのみ実行されます  
- [ ] いいえ — HTTP から HTTPS へのリダイレクトは**適用されていません**  

### HTTP Strict Transport Security (HSTS) は実装されていますか？
- [ ] はい — HSTS が**有効**であり、長い `max-age` が設定され、`includeSubDomains` および `preload` ディレクティブが含まれています  
- [ ] はい — HSTS は**有効**ですが、`includeSubDomains` または `preload` ディレクティブが欠落しています  
- [ ] いいえ — アプリケーションサーバーで HSTS は**有効化されていません**  

### 機密性の高いクッキーは平文送信から保護されていますか？

| クッキー名 | Secure フラグあり | Secure フラグなし |
|-------------|:-------------------:|:------------------:|
| `SessionID` |                     |                    |
| `AuthToken` |                     |                    |
| `CSRF-Token`|                     |                    |

### アプリケーションは暗号化されていないチャネルを介してサードパーティ API に機密データを送信していますか？
- [ ] いいえ — すべての外部 API 呼び出しは暗号化された（HTTPS）トンネルを介して実行されます  
- [ ] はい — 一部の機密性の低いテレメトリが HTTP を介して送信されます  
- [ ] はい — API キー、PII、または認証資格情報が暗号化されていない HTTP を介してサードパーティに送信**されています**  

---