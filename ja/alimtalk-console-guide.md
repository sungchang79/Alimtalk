## Notification > KakaoTalk Bizmessage > お知らせトーク > コンソール使用ガイド

## お知らせトーク一般送信

お知らせトークを送信するには、**NHN Cloud Console**で**Notification > KakaoTalk Bizmessage > お知らせトーク**を選択します。

![alimtalk_01_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_01_201812.png)

プラスフレンドを選択した後、登録されたテンプレートを読み込みます。
プラスフレンドは、**Notification > KakaoTalk Bizmessage > プラスフレンド管理**タブで確認できます。
テンプレートは、**Notification > KakaoTalk Bizmessage > お知らせトーク > テンプレート管理**タブで管理できます。

下にある**一般送信**タブで、テンプレートに存在する置換キー値を入力し、受信者番号を入力します。

入力完了後、右にある**送信**ボタンをクリックして即時送信します。

## お知らせトークの大量送信

### 大量送信リクエスト

下にあるタブで**大量送信**を選択します。

![alimtalk_02_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_02_201812.png)

* テンプレートを選択後、**テンプレートダウンロード**ボタンをクリックすると、テンプレート日本語識別子を含むCSVとXLSXファイルをダウンロードできます。テンプレート日本語識別子が含まれていない場合、送信失敗処理されます。
* CSVファイルをExcelで開いて保存する時、ハングルが正常に保存されないことがあるため、検収後に送信し、置換が正常に行われているかを確認することを推奨します。
* ファイルのアップロードはCSVとXLSXファイルのみ可能で、サイズは最大20MB、受信者の上限は1,000,000人です。

![alimtalk_03_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_03_201812.png)

***送信**ボタンをクリックすると、**検収後に進行**、**即時送信**の2つから選択して送信できます。

![alimtalk_04_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_04_201812.png)

## 送信結果の照会

一般送信の送信結果を確認できます。

発信日時は必須条件です。

![alimtalk_05_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_05_201812.png)

**登録日時**、**送信日時**は、どちらか１つのみ選択して照会が可能で、これは必須条件です
各送信メッセージの詳細を確認するには、検索結果項目をクリックすると表示される**詳細照会**を確認します。

![alimtalk_06_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_06_201812.png)

### 送信キャンセル

一般送信のうち、発信リクエスト日時を未来の日時でリクエストした予約送信の場合は、キャンセルが可能です。
予約発信リクエストを照会すると、リクエストIDの左にチェックボックスを確認できます。
チェックボックスはキャンセルされていない予約リクエストにのみ表示されます。
キャンセルしたいリクエストのチェックボックスを選択した後、上にある**選択予約キャンセル**ボタンを押すと、該当リクエストがキャンセルされます。
照会リストの上部にあるチェックボックスを使って、該当リストの全体選択およびキャンセルができます。

## 大量送信照会

### 送信/キャンセル

大量お知らせトークの送信時、**検収後に進行**を選択すると、**大量送信照会**タブでお知らせトークの送信やキャンセルができます。

![alimtalk_07_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_07_201812.png)

***送信**ボタンをクリックすると、大量送信を行います。検索結果で項目をクリックし、**詳細照会**ウィンドウで置換値を確認することを推奨します。
* 送信中の状態で**キャンセル**ボタンをクリックすると、一部の受信者に送信が行われることがあります。

* 大量送信照会タブで、検索結果の特定列をクリックすると、受信者項目が表示されます。

![alimtalk_08_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_08_201812.png)

* **受信者別照会**で該当受信者を選択し、送信内容が正常に置換されたかを確認できます。

![alimtalk_09_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_09_201812.png)

### 大量送信の進行状態
  - <b>待機：</b> テンプレートファイルデータを読み込む作業を進行する前の状態です。
  - <b>送信準備：</b> テンプレートファイルデータをロード中の状態です。
  - <b>送信準備完了：</b> テンプレートファイルデータをすべてロードし、送信準備が完了した状態です。リクエスト件(リストの行)を選択すると、下にあるリストで受信番号と送信内容を確認できます。
  - <b>送信待機：</b> 送信作業を行う前の状態です。
  - <b>送信中：</b> 送信が進行中の状態です。
  - <b>送信完了：</b> 送信リクエストが正常に完了した状態です。
  - <b>送信失敗：</b> 送信進行中に送信エラーが発生した場合です。
  - <b>送信取消：</b> ユーザーが送信をキャンセルした状態です。


## テンプレートの管理

### テンプレートの登録

**テンプレート登録**ボタンをクリックし、テンプレートを登録できます。

![alimtalk_10_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_10_201812.png)

* テンプレートコードは、10文字以内の英数字のみ登録できます。
* テンプレート名は150文字まで登録できます。
* テンプレートの内容は、ハングル/英字の区別なく、変数およびURLを含めて1,000文字まで登録できます。
* ボタンリンクURLは500文字まで登録できます。
* テンプレート日本語識別子は、次のように登録してください。 #{日本語識別子}
* ボタンリンクに日本語識別子を使用する時、http:// またはhttps:// プロトコルを必ず入力する必要があります。例) http://#{URL}またはhttps://#{URL}
* Excelファイルを利用して大量登録ができます。大量登録時に重複していたり、無効なテンプレートが存在する場合は正常なテンプレートのみ登録されます。
* テンプレート登録時、<b>リクエスト -> 検収中 -> 承認/差し戻し</b>の順にステータスがアップデートされます。
* テンプレートが差し戻された時、<b>お問い合わせ登録</b> および <b>修正</b>機能で再検収できます。
* 差し戻されたテンプレートは、<b>削除</b> 後に再登録できます。
* 詳細は[[テンプレート検収ガイド](https://www.bizmsg.kr/collected_statics/assets_landing/doc/alimtalk_template_guide.pdf)]を参照してください。

#### テンプレートの修正

* 承認されたテンプレートを修正した後に検収が完了すると、既存テンプレートの内容が修正した内容に代替されます。

#### テンプレートの削除

* <b><span style="color:red">リクエスト/差し戻し</span></b> 状態のテンプレートのみ削除できます。

#### テンプレートのお問い合わせ登録

* <b><span style="color:red">検収中/差し戻し</span></b> 状態のテンプレートのみお問い合わせを登録できます。
* 登録したお問い合わせは、検収結果に追加され、カカオ検収者が確認します。
* 検収結果にテンプレートの用途に関するお問い合わせ、差し戻し理由が追加されます。

## 代替送信の管理

* お知らせトーク/カカともへのメッセージなど、各メッセージタイプに応じてプラスフレンドの代替送信を設定できます。
* 代替送信を設定したプラスフレンドのメッセージのみLMSまたはSMSで代替送信されます。
* SMSアプリケーションキーを修正すると、すべてのプラスフレンドの代替送信設定は初期化されます。
* SMSサービスで代替送信されるため、SMSサービスの送信APIの仕様に応じてフィールドを入力する必要があります。(SMSサービスに登録された発信番号、各種フィールドの長さ制限など)
* 指定した代替送信タイプのバイト制限を超える代替送信のタイトルや内容は、途中で切れて代替送信されることがあります。([[SMS注意事項](https://docs.toast.com/ko/Notification/SMS/ko/api-guide/#_1)]参考)

![plusfriend_03_201812.png](https://static.toastoven.net/prod_alimtalk/plusfriend_03_201904.png)


## 統計
### 統計照会

送信リクエスト期間、プラスフレンド、テンプレート別に統計を確認できます。
送信リクエスト、成功、失敗、代替送信件をグラフと表で確認できます。

![alimtalk_11_201812.png](https://static.toastoven.net/prod_alimtalk/alimtalk_11_201812.png)
