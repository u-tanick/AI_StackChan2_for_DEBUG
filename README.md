# AI_StackChan2_for_DEBUG / 起動デバッグ用AIｽﾀｯｸﾁｬﾝ

AIｽﾀｯｸﾁｬﾝのコードを元にデバッグ用にカスタマイズしたものです。「わかりません」で困った人がいたらこれを入れて症状を確認してみるとかがおすすめです。

1. 起動時に読み込んでいるWifiの情報やAPIキーを視認性良く確認できて
2. 簡単操作でVOICEVOX/ChatGPT接続/ChatGPTとの会話の３段階のどこまで動作するかを確認できて
3. エラーがあったときにどこが不具合になってそうかを答えてくれます

打倒「わかりません！」

![わかりません](/img/wakarimasen.jpg)  
引用：https://booth.pm/ja/items/5521810

めちゃくちゃニッチなんだけど、あるとちょっと困ったときにいいかも。みたいな？

以下に簡単に機能と使い方をご紹介します。

### 事前準備

前提として、SDカード(32GB以下推奨)に次の内容のファイルを入れておいてください。
サーボモーターは繋ぐ必要ありません。あくまで通信のデバッグ用として、動作しないようにしています。

初回起動時にSDカードセットしていないと、起動時にWiFiに接続しようとして延々ループします。

![SDカード](/img/sd-card.jpg)

- wifi.txt
  - 1行目：`SSID`
  - 2行目：`パスワード`
- apikey.txt
  - 1行目：`OpenAIのAPIキー`
    - ChatGPTとの通信用
  - 2行目：`VOICEVOXのAPIキー`
    - キャラクターボイス用（デフォルトはずんだもん）
  - 3行目：`OpenAIのAPIキー`　または　`Google Speech-to-TextのAPIキー`
    - AIｽﾀｯｸﾁｬﾝに話しかけた声をテキストに変換する用
    - `Google Speech-to-Text` の方が高速？ただあれこれ用意するのが大変なのでひとまず動作させるなら `OpenAIのAPIキー` でいいと思います。

★ OpenAIのAPIは、アカウントを作って、何ドル分かクレジット購入する必要あります。  
参考：https://qiita.com/kurata04/items/a10bdc44cc0d1e62dad3

★ VOICEVOXのAPIは、以下のサイトを参考に作成してください。  
参考：https://zenn.dev/mongonta/articles/8aac1041c628d4

★ Google Speech-to-Textを使う場合も、APIキーの取得のために支払い設定が必要になります。  
参考：https://cloud.google.com/speech-to-text/docs/before-you-begin?hl=ja

### 1. 起動時に読み込んでいるWifiの情報やAPIキーを視認性良く確認

SDカードがセットされていれば起動時に、読み込んだSDカードの情報や起動時のステップごとの状況が画面に表示されます。正常に起動したらｽﾀｯｸﾁｬﾝの顔が表示されます。かわいいですね。

![起動した状態](/img/wake-up-done.jpg)

例えば、WiFiのSSIDなどが正しかったら、「WiFi Connected」と表示されます。
家のWiFiが何種類かあったり、家とスマホのテザリングを切り替えたりしてるとSSIDとかパスワード間違うこともあるので、最初に表示されるSSIDとパスワードで目視確認できてうれしい感じです。

★ これは「秘密情報」を表示させていることになるので、「起動デバッグ用AIｽﾀｯｸﾁｬﾝ」は他人がいるところでは使わない方がよいです。あくまで家でのデバッグ用です。APIとかが正常そうだと確信がもてたら、同じ設定を本家の「AIｽﾀｯｸﾁｬﾝ」に設定してあげてください。

### 2. 簡単操作でつぎの３段階のどこまで動作するかを確認

正常に起動した「起動デバッグ用AIｽﾀｯｸﾁｬﾝ」は、この状態から次の3つの操作を行うことができます。

![ボタン](/img/button.jpg)

- Aボタン
  - VOICEVOXと通信を行い、固定テキスト `「ボイスボックスと通信できたのだ。」` をずんだもんボイスでしゃべってくれます。
  - Wifiにちゃんと繋がっていたら、正常に動作するはずです。
- Bボタン
  - ChatGPTと通信を行い、固定テキスト `「こんにちわ」` が送信され、その返事をずんだもんボイスでしゃべってくれます。
  - Wifiにつながっていて、OpenAIのAPIキーが有効だったら、正常に動作するはずです。
- Cボタン
  - ChatGPTと通信を行い、あなたが話しかけた言葉が送信され、その返事をずんだもんボイスでしゃべってくれます。
  - Wifiにつながっていて、音声からテキストへの変換ができて、`OpenAIのAPIキー` が有効だったら、正常に動作するはずです。
  - 音声からテキストへの変換に `Google Speech-to-Text` を使っている場合も同様です。


### 3. エラーがあったときにどこが不具合になってそうかを答えてくれる

上記のボタン操作で、SDカードで与えた設定やAPIの状態がどこまで正常なのかを確認することができるのですが、「わかりません」などのエラーが発生した時が問題です。

特にBボタンとCボタンは、`APIキーの有効性` や `Root証明書の有効期限切れ` などによっていくつかの原因が考えられます。
そこで「起動デバッグ用AIｽﾀｯｸﾁｬﾝ」では、この発生したエラー毎に、画面上とボイスで原因となりそうなポイントを教えてくれるようにしています。

各エラーの状況毎に、以下のような説明をしてくれます。

- Bボタン
  - 「わかりません」
    - ChatGPTへの接続に失敗しています。
    - APIキーまたは証明書が古いか、誤っている可能性があります。
  - 「エラーです」
    - ChatGPTから返されたJSONテキストの変換で失敗しています。
    - 通信が不安定か、ChatGPTのAPIの仕様が変わっている可能性があります。

- Cボタン
  - Bボタンのエラーに加えて次のエラーが発生する可能性があります。
  - 「聞き取れませんでした」
    - 話しかけた音声のテキスト変換で失敗しています。
    - `Google Speech-to-TextのAPIキー`をご使用の場合は、APIキーが古いか誤っている、課金が必要などの可能性があります。
    - `OpenAIのAPIキー`をご使用の場合は、APIキーが古いか、誤っている可能性があります。

自分は今回このCボタンの「聞き取れませんでした」でハマったので、勢いでこのデバッグ用AIスタックチャンを作りました。

### 【追加機能】Root証明書をSDカードから読み込む

これはちょっとマニアックな話になるのですが、AIスタックチャンが `OpenAI ChatGPT` や `Google Speech-to-Text` に接続する際に `Root証明書` という認証情報が必要となっています。

この `Root証明書` は、AIスタックチャンのソースコードの中に埋め込んだ形になっているのですが、まれに `認証書` の有効期限が切れしまう場合があります。そうなると世の中のすべてのスタックチャンが「わかりません」になってしまいます。

そのためAIスタックチャンのソースコードを修正して証明書の情報を更新してあげる必要がありますが、毎回コード修正するのもめんどいので「もしかして、証明書が切れたかも？」というときの検証用として「Root証明書をSDカードから読み込む」も追加しています。

- 参考：ソースコード上でのRoot証明書の場所
  - /src/rootCACertificate.h
  - /src/rootCAgoogle.h

使い方は簡単で、SDカードに `OpenAI ChatGPT` と `Google Speech-to-Text` それぞれ用の証明書情報を書き込んだテキストファイル `ca_openai.txt` , `ca_google.txt` を入れておくだけです。

これらのファイルがあれば、そのファイルの証明書を利用してAPIに接続します。ファイルが無い場合は、あらかじめAIスタックチャンに設定されている証明書が利用されます。

- ca_openai.txt
  - 証明書の情報
  - 【取得方法】
    - https://platform.openai.com/api-keys
    - にアクセスして以下の図の手順で取得
    - 取得した `GTS Root R4.crt` ファイルをca_openai.txtという名前に変更

![OpenAIのRoot証明書の取得手順](/img/openai-root-ca.jpg)

- ca_google.txt
  - 証明書の情報
  - 【取得方法】
    - ＜確認中＞
