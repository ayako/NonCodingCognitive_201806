# Cognitive Services 活用! Microsoft PowerApps & Flow / Azure Locig Apps によるノンコーディング開発 

## Azure Logic Apps を活用した、お問い合わせアラートシステムの開発

今回は Webサイトのお問い合わせフォームまたはメールによる、顧客からのお問い合わせメールの内容を判別するシステムを構築します。
Azure Locig Apps を利用して、ノンコーディングで下記のフローを構築します。

- メールを受信したら、メール件名からお問い合わせメールを判別
- Cognitive Services Text Analytics API を利用した Sentiment(ネガポジ)分析を行い、
  - お問い合わせに対して自動で受付返答メールを送信
  - お問い合わせメールの内容をお問い合わせメール DB (OneDrive に保存した Excel ファイル)  に保存

### 準備するもの

#### Azure サブスクリプション

持っていない場合は無料評価版でOKですので取得しておきます。

>[Azure の無料サブスクリプションの申し込み方法](https://github.com/ayako/AAJP-EmotionBotHoL/blob/master/AzureSubscriptionTrial.md)

#### Cognitive Services Text Analytics の API Key

[Cognitive Services の無料サブスクリプションの申し込み方法: 2. Free Tier(F0)](https://github.com/ayako/NonCodingCognitive_201806/blob/master/CognitiveSubscriptionTrial.md#2-azure-portal-%E3%81%8B%E3%82%89-free-tierf0-%E7%84%A1%E6%96%99%E3%83%97%E3%83%A9%E3%83%B3-%E3%81%AE%E7%94%B3%E3%81%97%E8%BE%BC%E3%81%BF%E6%96%B9%E6%B3%95) の手順で **Text Analytics** のサービスを申し込み、API Key を取得しておきます。
 
#### Office 365 アカウント (Office 365 Business Premium)

Outlook Online, Excel Online を使用します。持っていない場合は無料評価版でOKですので取得しておきます。

>[法人向け Office 365](https://products.office.com/ja-jp/compare-all-microsoft-office-products?tab=2)

<br />
<br />


## 手順

### 1. メール内容保存 DB (Excel ファイル) の作成

[Office Online のサイト](https://www.office.com) にアクセスしてログインします。

<img src="media/LogicApps_20180625_01.PNG" width="450" height="291">  

最初にお問い合わせ DB となる Excel を作成します。 Office Online のホーム画面 から **Excel** をクリックして、**新しい空白のブック** をクリックして作成します。

<img src="media/LogicApps_20180625_02.PNG" width="450" height="291">  

Excel ブック の Sheet1 の 1 行目 A1~E1 のセル に **受信日時** **メールアドレス** **メール内容** **スコア** **クレーム** と入力します 

<img src="media/LogicApps_20180625_03.PNG" width="450" height="291">  

A1~E1 のセルを選択し、ツールバーの **テーブルとして書式設定** をクリックします。

<img src="media/LogicApps_20180625_04.PNG" width="450" height="291">

*先頭行をテーブルの見出しとして設定する* にチェックをつけて、[**OK**] をクリック、テーブルの設定を行います。

<img src="media/LogicApps_20180625_05.PNG" width="450" height="291">  

<img src="media/LogicApps_20180625_06.PNG" width="450" height="291">  

ツールバーの **ファイル** > **名前を付けて保存** をクリックして、**名前を付けて保存** をクリックします。

<img src="media/LogicApps_20180625_07.PNG" width="450" height="291">

**LogicApps-HandsOn** という名前を付けてExcelブックを保存します。これで、お問い合わせメール DB としての設定が完了です。

<img src="media/LogicApps_20180625_08.PNG" width="450" height="291">  

<br />


### 2. Azure Logic Apps の新規アプリ作成

[Azure Portal](https://portal.azure.com) をブラウザーで開き、Azure サブスクリプションを申し込んだアカウントでサインインします。

<img src="media/LogicApps_20180625_11.PNG" width="450" height="291">  

**+リソースの作成** をクリックして、検索欄に **Logic App** と入力して、Logic Apps を検索、一覧から **Logic App** をクリックし、*Logic App* ペイン で [**作成**] をクリックして、新規作成手順に進みます。

<img src="media/LogicApps_20180625_12.PNG" width="450" height="291">  

*ロジック アプリの作成* ペインで、*名前* にお好みの名前を入力し、*リソースグループ* は **新規作成** を選択してお好みのグループ名を入力します。[**作成**] をクリックして、Logic Apps アプリを作成します。

<img src="media/LogicApps_20180625_13.PNG" width="450" height="291">  

*作成が完了しました* というメッセージが表示されたら、クリックして、作成した Logic Apps アプリを表示します。

<img src="media/LogicApps_20180625_14.PNG" width="450" height="291">  

自動で表示される (または Logic Apps アプリ のメニューから **ロジックアプリデザイナー** をクリック)、*ロジックアプリデザイナー* からロジックの作成を行います。

<img src="media/LogicApps_20180625_15.PNG" width="450" height="291">  

<br />


### 3. メールの受信: Office 365 Outlook コネクターの設定

*Logic Apps デザイナー* 画面を下にスクロールして、*テンプレート* を表示、**空のロジックアプリ** をクリックして、空白の状態からフローの作成を開始します。

<img src="media/LogicApps_20180625_16.PNG" width="450" height="291">  

フローを開始するためのトリガーを設定します。検索欄に **Outlook** と入力して、Outlook 365 の特定のアカウントに対するトリガーを検索します。

<img src="media/LogicApps_20180625_17.PNG" width="450" height="291">  

**Office 365 Outlook - 新しいメールが届いたとき** をクリックして選択します。

<img src="media/LogicApps_20180625_18.PNG" width="450" height="291">  

*Office 365 Outlook - 新しいメールが届いたとき* のアクションで、トリガーとなる Office 365 Outlook のアカウントを設定するため、[**サインイン**] をクリックして、該当するアカウントの情報でサインインを行います。

<img src="media/LogicApps_20180625_19.PNG" width="450" height="291">  

*Office 365 Outlook - 新しいメールが届いたとき* のアクション詳細設定で、メールを確認する頻度を設定します。ここではテストを行いやすくするため、間隔を **1分** に設定します。

<img src="media/LogicApps_20180625_20.PNG" width="450" height="291">  

<br />


### 4. メール本文の取り出し: Content Conversion (HTML to Text) コネクターの設定

[**＋新しいステップ**] をクリック、[**アクションの追加**] をクリックして、次のアクションを追加します。

<img src="media/LogicApps_20180625_21.PNG" width="450" height="291">  

検索欄に **html** と入力し、**Content Conversion - Html to text** をクリックして選択します。

>受信するメールが html 形式の場合を考慮し、本文を Plain text として抽出するためのステップです。 

<img src="media/LogicApps_20180625_22.PNG" width="450" height="291">  

*Html to text* のアクション詳細設定で、*Content* の欄をクリックします。

<img src="media/LogicApps_20180625_23.PNG" width="450" height="291">  

表示される *動的コンテンツ* 一覧から **本文** をクリックして、*Content* に設定します。

<img src="media/LogicApps_20180625_24.PNG" width="450" height="291">  

<br />


### 5. メール件名による条件分岐

[＋新しいステップ] をクリック、[条件の追加] をクリックして、条件分岐を追加します。

<img src="media/LogicApps_20180625_25.PNG" width="450" height="291">  

条件の詳細設定と、条件に対して True の場合 と False の場合の条件分岐が設定されます。

<img src="media/LogicApps_20180625_26.PNG" width="450" height="291">  

条件の詳細設定で、左欄の値に 動的コンテンツから **件名** をクリックして設定、条件として **次の値を含む** を選択、右欄の値に **お問い合わせ** と入力して、メールの件名に "お問い合わせ" を含む場合、という条件を設定します。

その後、[**＋追加**] をクリックして **行の追加** を選択して条件を追加します。

<img src="media/LogicApps_20180625_27.PNG" width="450" height="291">  

同様に **件名**　に "お問合せ" や　"問合せ" といった文言を含む場合、という条件を設定します。

<img src="media/LogicApps_20180625_28.PNG" width="450" height="291">  

<br />


### 6. メール本文のネガポジ判別(1): Microsoft Translator コネクターの設定

条件が True の場合のアクションを設定します。アクションの検索欄に **翻訳** と入力し、**Microsoft Translator - テキストの翻訳** をクリックします。

<img src="media/LogicApps_20180625_29.PNG" width="450" height="291">  

*テキストの翻訳* のアクション詳細設定で、**テキスト** には 動的コンテンツから *Html to Text* コネクターで抽出された **The plain text content** をクリックして選択します。**対象言語** は **English** を選択し、**詳細オプションを表示する** をクリックして詳細を設定します。

> ネガポジ分析を行う上て、より精度の高い英語で分析するため、一旦メール本文を英語に翻訳します。

<img src="media/LogicApps_20180625_30.PNG" width="450" height="291">  

**カテゴリ** に **generalnn** と入力します。

>Translator API の仕様で、Neural Network を使った翻訳を行うオプションを設定しています。

<img src="media/LogicApps_20180625_31.PNG" width="450" height="291">  

<br />


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


