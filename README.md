#### 開発メモ
ワークフロー
<br>　<img width="600"  src="https://user-images.githubusercontent.com/40127279/126858838-6207222c-1d9c-45d6-85e1-86e0303f80a9.png">
### 1.NatureRemoのAPI
　URLでNatureRemoをコントロールできます
<br>　Tokenの取得やAPIの詳細はインターネットで調べてください
<br>　APIはCURLのPOSTで操作が可能ですのでターミナルで遊んでみてください
<br>　なお、Tokenは個々のNatureRemoへのアクセス用になるので、公開できない情報です
<br>　（公開するとあなたの家電を第3者がコントロールできちゃいますから）
### 2.export禁止変数
<br>　tokenのような情報はexport禁止の変数として管理します
<br>　ワークフロー右上の[χ]をクリックして環境変数の一覧を表示させてください
<br>　インストール後には、Nameにtoken、valueが空欄、Don't Exportにチェックとなっています
<br>　valueにご自身の使いたいNatureRemoのトークンを貼り付けてください
<br>　引用符は不要です
<br>　
<br>　Configure workflow and variables
<br>　<img width="600"  src="https://user-images.githubusercontent.com/40127279/126858892-357ec2f8-864c-469d-ba0c-4459d081ee21.png">
### 3.キーワード（TV）入力時のパラメータ
　キーワドなしとキーワドありをサポートしたいのでArgument Optionalを指定しています
<br>　この後のconditionalユーティリティで処理の分岐を制御しています
<br>　
<br>　Keyword
<br>　<img width="485" src="https://user-images.githubusercontent.com/40127279/126858910-588bfe67-1873-4ca3-99cd-9827dafb87f6.png">
### 4.入力パラメータによって処理を分岐させる（conditionalユーティリティ）
　今回のconditionalユーティリティは4分岐としています
<br>　シェルスクリプトのソースは基本コマンドラインの集合で、構造化に向いていないので
<br>　このように分岐させるとわかりやすいかな
<br>
<br>　conditionalユーティリティを開いてみてください
<br>　1行目の条件はパラメータがない状態を意味します（『is equal to』と『（空欄）』）
<br>　なお『then』のあとのテキスト（『On/Off』）はalfredワークフローをわかりやすくする
<br>　ための表示用文字列です。ソースコードを書くような感じに見えますが勘違いしないようにご注意ください
<br>
<br>　2行目の条件（『matches regex』）は正規表現で条件をかけますが
<br>　『+』,『++』,・・・と『+』10個までの文字列にマッチします
<br>　結構難しく、試行錯誤を繰り返してしまいましたが、TV ++として音量を2つあげるということを当初の
<br>　目的を達成できました
<br>
<br>　3行目は上記のマイナスバージョンです
<br>　後続のRun Scriptはほぼ同じですが、remo操作用のIDが異なるので、ここで分岐させました
<br>
<br>　4行目はその他でチャンネル変更になります
<br>　数字や文字での指定が可能ですが、詳細はスクリプトで実装します
<br>
<br>　conditional 
<br>　<img width="600" src="https://user-images.githubusercontent.com/40127279/126858946-c9dc59d8-d245-493c-9df1-065ed157fc70.png">
### 5.APIを利用する
　NatureRemoのAPIでは、tokenとIDで、どのNatureRemoで何の操作をするかを指定します
<br>　tokenはパネルで設定していますが、スクリプト内では$tokenとして記述できます
<br>
<br>　IDは、それぞれの環境で異なると思いますので、変数として切り出しました
<br>　またCurlが長文になるので\で区切って改行させています
<br>
<br>　tokenは変数として固有の値を設定しています。スクリプト内では$tokenとしてha
<br>　ヴォリューム用のスクリプトでは文字の長さに応じて繰り返し処理をしています
<br>　下記の＄{#1}がパラメータの長さを取得するコードです
<br>　『++』であれば2、『---』であれば3となります
```
　for ((i=0;i<${#1};i++)) ; do
```
<br>　
<br>　RunScript （Volume +）
<br>　<img width="600" src="https://user-images.githubusercontent.com/40127279/126858977-c705e7b3-a334-48ea-adfc-c1c8efc2b4ed.png">

### 4.チャンネル変更用のIDをセットする
　チャンネル変更のスクリプトはcaseで多重分岐を処理しています
<br>　条件が文字列マッチで、正規表現のように|がorの意味です
<br>
<br>　ちなみにcaseはesacで締め、ifはfiで締めていて粋な感じですね
<br>　for（というかdo?）はrofでなないのですね
<br>　
<br>　RunScript （Channel）
<br>　<img width="600" src="https://user-images.githubusercontent.com/40127279/126859013-b9c3ce5c-fef9-4430-b4b2-26797296170a.png">

#### 取扱説明
### 機能:
　NatureRemoのAPIでTVをリモコン操作する
<br>　※conditionalユーティリティ、export禁止変数のサンプルです
### インストール:
　1.[alfredworkflow](https://github.com/KitanoTamotsu/natureremo/releases/download/1.0/TV.with.NatureRemo.alfredworkflow.zip)をダウンロード 
<br>　2.ファイルをダブルクリックしてワークフローに登録
<br>　3.NatureRemoのアクセス用トークンを入力し変数を設定（インストール後で可能）
<br>　　※ワークフロー右上の[χ]をクリックして設定
<br>　4.多分スクリプトにある$IDも変更が必要（インストール後に変更）
<br>　　※Run Scriptを開いて書き換え
### 使い方:
<br>　1.Hotkey『⌃t』（HOTKEYはご自身で設定が必要です）でテレビのON/OFF
<br>　
<br>　2.キーワード『TV』＋　パラメータでテレビをコントロール
<br>　例）
<br>　　TV（パラメータなし）　ON/OFF
<br>　　TV　数字/文字　      チャンネル変更
<br>　　TV　+/-　           Vol変更


#### 修正履歴
### ver1.0既知のバグ
<br>　チャンネル変更でetvをセットする条件が誤っていました
<br>　3|etv|eとなっていますが、2|etv|eが正解です
<br>　セットするIDについても違っていたので修正しています
<br>　TV東京も同様にIDが12のものでした。7が正しいです
 
### ver1.1
 TV番組表.alfredworkflowと連携させるため、青い部分を追加しています
<br>　開発メモ等の詳細は、https://kitanotamotsu.github.io/tv をご覧ください
<br>
<br>
[トップページに戻る](https://kitanotamotsu.github.io/)

