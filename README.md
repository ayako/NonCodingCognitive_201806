# Cognitive Services 活用! Microsoft PowerApps &amp; Flow / Azure Logic Apps によるノンコーディング開発

Microsoft PowerApps &amp; Flow / Azure Locig Apps からノンコーディングで Cognitive Services を活用したアプリを開発するハンズオンです。

## [Azure Logic Apps 編](NonCodingCognitive_AzureLogicApps.md)

Webサイトのお問い合わせフォームまたはメールによる、顧客からのお問い合わせメールの内容を判別するシステムを構築します。
Azure Locig Apps を利用して、ノンコーディングで下記のフローを構築します。

- メールを受信したら、メール件名からお問い合わせメールを判別
- Cognitive Services Text Analytics API を利用した Sentiment(ネガポジ)分析を行い、
  - お問い合わせに対して自動で受付返答メールを送信
  - お問い合わせメールの内容をお問い合わせメール DB (OneDrive に保存した Excel ファイル)  に保存

### 準備するもの
- Azure サブスクリプション
- Cognitive Services Text Analytics の API Key
- Office 365 アカウント (Office 365 Business)

準備方法は [ハンズオン](NonCodingCognitive_AzureLogicApps.md) の冒頭をご確認ください。
