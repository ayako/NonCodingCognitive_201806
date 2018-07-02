# Cognitive Services 活用! Microsoft PowerApps & Flow / Azure Locig Apps によるノンコーディング開発 

## Azure Logic Apps を活用した、お問い合わせアラートシステムの開発

今回は Webサイトのお問い合わせフォームまたはメールによる、顧客からのお問い合わせメールの内容を判別するシステムを構築します。
Azure Locig Apps を利用して、ノンコーディングで下記のフローを構築します。

- メールを受信したら、メール件名からお問い合わせメールを判別
- Cognitive Services Text Analytics API を利用した Sentiment(ネガポジ)分析を行い、
  - お問い合わせに対して自動で受付返答メールを送信
  - お問い合わせメールの内容をお問い合わせメール DB (Sharepoint に保存した Excel ファイル)  に保存

### 準備するもの

- Azure サブスクリプション
- Office 365 アカウント (Outlook, Excel, Sharepoint を使用します)

## 手順

### 1. メール内容保存 DB (Excel ファイル) の作成

<img src="media/LogicApps_20180625_01.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_02.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_03.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_04.PNG" width="450" height="291">

<img src="media/LogicApps_20180625_05.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_06.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_07.PNG" width="450" height="291">

<img src="media/LogicApps_20180625_08.PNG" width="450" height="291">  

### 2. Azure Logic Apps の新規アプリ作成

<img src="media/LogicApps_20180625_11.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_12.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_13.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_14.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_15.PNG" width="450" height="291">  

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
