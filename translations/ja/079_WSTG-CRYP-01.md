## WSTG-CRYP-01 — 脆弱なトランスポート層セキュリティ (Transport Layer Security) のテスト

脆弱なトランスポート層セキュリティのテストでは、SSL/TLSの実装と設定を分析し、転送中のデータの機密性と完全性が確保されているかを確認します。攻撃者は、古いプロトコル（SSLv2、TLS 1.0）、脆弱な暗号スイート (Cipher Suites)（NULL、RC4、または anonymous）、および無効または署名の弱い証明書などの設定ミスを標的にして、中間者攻撃 (Man-in-the-Middle (MitM) attacks) を実行します。このテストは通常、暗号化通信が確立されるアプリケーションのウェブサーバーまたはロードバランサーのエンドポイントを対象とします。悪用に成功すると、攻撃者は機密データの傍受、ユーザーセッションのハイジャック、またはプロトコルのダウングレードやトラフィックの復号を通じて、接続を不安全な状態に引き下げることが可能になります。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **テストステータス** | 未実施 |
| **深刻度** | 高 / 中* |

> *機密データ（個人情報 (PII)、認証情報、セッショントークン）が送信される場合は「高」、一般的な情報トラフィックの場合は「中」となります。

**参考文献:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### 古い、または安全でない TLS/SSL プロトコルがサポートされていますか？
- [ ] いいえ — TLS 1.2 および TLS 1.3 のみが **有効** です
- [ ] はい — TLS 1.0 または TLS 1.1 が **有効** です
- [ ] はい — SSLv2 または SSLv3 が **有効** です *(致命的)*

### 脆弱な、または安全でない暗号スイートがサーバーによって受け入れられていますか？
- [ ] いいえ — 強力で最新の暗号（例：AES-GCM、ChaCha20）のみが **サポート** されています
- [ ] はい — 脆弱な暗号化（例：3DES、RC4）またはビット長の短い暗号が **サポート** されています
- [ ] はい — NULL、anonymous (ADH)、または輸出用 (Export) 暗号が **サポート** されています *(致命的)*

### サーバー証明書は有効で信頼されていますか？
- [ ] はい — 証明書は有効で、ドメインと一致しており、**信頼された認証局 (CA)** によって署名されています
- [ ] いいえ — 証明書が **期限切れ** であるか、**弱い署名アルゴリズム**（例：SHA-1）を使用しています
- [ ] いいえ — 証明書が **自己署名** されているか、ドメイン名の **不一致** が発生しています

### サーバーはセキュアな再ネゴシエーション (Secure Renegotiation) をサポートし、ダウングレード攻撃を防いでいますか？
- [ ] はい — セキュアな再ネゴシエーションが **サポート** されており、TLS Fallback SCSV が **有効** です
- [ ] いいえ — セキュアな再ネゴシエーションが **サポートされていない** ため、MitM インジェクションを許容する可能性があります
- [ ] いいえ — サーバーは **POODLE** または **ROBOT** 攻撃に対して脆弱です

### HTTP Strict Transport Security (HSTS) は適切に実装されていますか？

| ヘッダー | あり | なし |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] はい — HSTS ヘッダーが存在し、長い `max-age` が設定され、`includeSubDomains` が **有効** です
- [ ] はい — HSTS ヘッダーは存在しますが、`includeSubDomains` が欠落しているか、`max-age` が **短すぎます**
- [ ] いいえ — HSTS ヘッダーが **存在しません**

---