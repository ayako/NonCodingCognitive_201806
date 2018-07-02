# Cognitive Services 活用! Microsoft PowerApps & Flow / Azure Locig Apps によるノンコーディング開発 

## Azure Logic Apps を活用した、お問い合わせアラートシステムの開発

今回は Webサイトのお問い合わせフォームまたはメールによる、顧客からのお問い合わせメールの内容を判別するシステムを構築します。
Azure Locig Apps を利用して、ノンコーディングで下記のフローを構築します。

- メールを受信したら、メール件名からお問い合わせメールを判別
- Cognitive Services Text Analytics API を利用した Sentiment(ネガポジ)分析を行い、
  - お問い合わせに対して自動で受付返答メールを送信
  - お問い合わせメールの内容をお問い合わせメール DB (Sharepoint に保存した Excel ファイル)  に保存

### 準備するもの

#### Azure サブスクリプション

持っていない場合は無料評価版でOKですので取得しておきます。

>[Azure の無料サブスクリプションの申し込み方法](https://github.com/ayako/AAJP-EmotionBotHoL/blob/master/AzureSubscriptionTrial.md)

#### Cognitive Services Text Analytics の API Key

[Cognitive Services の無料サブスクリプションの申し込み方法: 2. Free Tier(F0)](https://github.com/ayako/NonCodingCognitive_201806/blob/master/CognitiveSubscriptionTrial.md#2-azure-portal-%E3%81%8B%E3%82%89-free-tierf0-%E7%84%A1%E6%96%99%E3%83%97%E3%83%A9%E3%83%B3-%E3%81%AE%E7%94%B3%E3%81%97%E8%BE%BC%E3%81%BF%E6%96%B9%E6%B3%95) の手順で **Text Analytics** のサービスを申し込み、API Key を取得しておきます。
 
#### Office 365 アカウント (Office 365 Business Premium)

Outlook Online, Excel Online, Sharepoint Online を使用します。持っていない場合は無料評価版でOKですので取得しておきます。

>[法人向け Office 365](https://products.office.com/ja-jp/compare-all-microsoft-office-products?tab=2)

<br />
<br />

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

<img src="media/LogicApps_20180625_16.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_17.PNG" width="450" height="291">  

### 3. メールの受信: Office 365 Outlook コネクターの設定

<img src="media/LogicApps_20180625_18.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_19.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_20.PNG" width="450" height="291">  

### 4. メール本文の取り出し: Content Conversion (HTML to Text) コネクターの設定

<img src="media/LogicApps_20180625_21.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_22.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_23.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_24.PNG" width="450" height="291">  

### 5. メール件名による条件分岐

<img src="media/LogicApps_20180625_25.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_26.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_27.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_28.PNG" width="450" height="291">  

### 6. メール本文のネガポジ判別(1): Microsoft Translator コネクターの設定

<img src="media/LogicApps_20180625_29.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_30.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_31.PNG" width="450" height="291">  

### 7. メール本文のネガポジ判別(2): テキスト分析 (Text analytics) コネクターの設定

<img src="media/LogicApps_20180625_32.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_33.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_34.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_35.PNG" width="450" height="291">  

### 8. ネガポジ判定による条件分岐

<img src="media/LogicApps_20180625_36.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_37.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_38.PNG" width="450" height="291">  

### 9. ネガ判定時のアクション設定(1): メール送信

<img src="media/LogicApps_20180625_39.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_40.PNG" width="450" height="291">  

### 10. ネガ判定時のアクション設定(2): DB 保存

<img src="media/LogicApps_20180625_41.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_42.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_43.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_44.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_45.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_46.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_47.PNG" width="450" height="291">  

### 11. ポジ判定時のアクション設定

<img src="media/LogicApps_20180625_48.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_49.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_50.PNG" width="450" height="291">  

### 12. Logic Apps の実行テスト

<img src="media/LogicApps_20180625_51.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_61.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_62.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_63.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_64.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_65.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_66.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_67.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_68.PNG" width="450" height="291">  


