# Cognitive Services 活用! Microsoft PowerApps & Flow / Azure Locig Apps によるノンコーディング開発 

## Azure Logic Apps を活用した、お問い合わせアラートシステムの開発

今回は Webサイトのお問い合わせフォームまたはメールによる、顧客からのお問い合わせメールの内容を判別するシステムを構築します。
Azure Locig Apps を利用して、ノンコーディングで下記のフローを構築します。

- メールを受信したら、メール件名からお問い合わせメールを判別
- Cognitive Services Text Analytics API を利用した Sentiment(ネガポジ)分析を行い、
  - お問い合わせに対して自動で受付返答メールを送信
  - お問い合わせメールの内容をお問い合わせメール DB (Sharepoint に保存した Excel ファイル)  に保存


### 1. メール内容保存 DB (Excel ファイル) の作成
### 2. Azure Logic Apps の新規アプリ作成
### 3. メールの受信: Office 365 Outlook コネクターの設定
### 4. メール本文の取り出し: Content Conversion (HTML to Text) コネクターの設定
### 5. メール件名による条件分岐
### 6. メール本文のネガポジ判別(1): Microsoft Translator コネクターの設定
### 7. メール本文のネガポジ判別(2): テキスト分析 (Text analytics) コネクターの設定
### 8. ネガポジ判定による条件分岐
### 9. ネガ判定時のアクション設定(1): メール送信
### 10. ネガ判定時のアクション設定(2): DB 保存
### 11. ポジ判定時のアクション設定
### 12. Logic Apps の実行テスト
