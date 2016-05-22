# kintone_transrec

A barebones Node.js app using [Express 4](http://expressjs.com/).

## 概要

クラウド型留守番電話サービス「[TRANSREC](http://www.transrec.net)」のRESTful APIを利用して、Kintoneに録音されたデータを登録するデモプログラムです。
留守番電話が録音されたら、TRANSRECからのPing通知をトリガーにして、テキストデータと音声データをKintoneに保存します。

## 準備

- [Node.js](http://nodejs.org/) のインストール。
- [TRANSREC](https://www.transrec.net/)の申し込み。
- [Kintone](https://kintone.cybozu.com/jp/)の申し込み。
- インターネット上で公開可能なサーバ（デモプログラムの実行場所）。

## インストール

1.githubからサーバー上の任意のフォルダにソースコードをダウンロードします。

```sh
$ git clone git@github.com:mobilebiz/kintone_transrec.git # or clone your own fork
$ cd kintone_transrec
$ npm install
```

2.TRANSRECのAPIキー(SID、Token)を取得し、/config/default.json 内のtransrecセクション内に記載します。
APIキーの取得方法については、[こちら](https://transrec.zendesk.com/hc/ja/articles/201682239)を参照してください。
なお、Ping通知設定欄の「Ping通知先」については、以下の通りに設定します。
- プロトコル: http
- ポート番号: 3000
- メソッド: POST
- 接続先ホスト: このプログラムを実行するサーバ（例:www.hoge.com）
- 接続先パス: /ping
- 発信者通知: OFF
- 着信番号通知: OFF

3.Kintone側でアプリを作成し、録音データを格納するためのフォームを作成します。
フォームの名前と内容は以下のように設定してください。
- 着信日時（データタイプ：日時、フィールドコード：calledDate）
- 発信者番号（データタイプ：文字列（１行）、フィールドコード：phoneNum）
- メッセージ（データタイプ：文字列（複数行）、フィールドコード：message）
- 音声データ（データタイプ：リンク（URL）、フィールドコード：mp3）
作成したアプリのホスト名（例：hogehoge.cybozu.com）とKintoneのIDとパスワードを、/config/default.json内の、kintoneセクション内に記載します。

## プログラムの起動

```sh
$ npm start
```

3000番ポートでWebサーバーが起動しますので、APIキーを取得したTRANSRECアカウントに留守番電話を吹き込みます。

音声データ（mp3）は、public/mp3フォルダ内に作成されています。

## ドキュメント

- [TRANSREC RESTful API](https://transrec.zendesk.com/hc/ja/categories/200105579)
