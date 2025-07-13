---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://images.unsplash.com/photo-1667514625485-8ee6281f9dac?ixlib=rb-4.0.3&auto=format&fit=crop&w=2000&q=80
# some information about your slides (markdown enabled)
title:  「箱庭」をハックするGoogle Apps Script： Slack Bot開発で得た知見（可能性と挑戦）
info: |
  TechRAMEN 2025 Conference  
  https://techramenconf.net/

# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
addons:
  - '@katzumi/slidev-addon-qrcode'
  - "@katzumi/slidev-addon-ogp-image"
  - slidev-addon-components
  - slidev-addon-rabbit
---

# 「箱庭」をハックするGoogle Apps Script： Slack Bot開発で得た知見（可能性と挑戦）
TechRAMEN 2025 Conference Jul 26, 2025.  
v0.0.1  
@katzumi(かつみ)

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/k2tzumi/exploring-hakoniwa-hack-via-gas-slackbot" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
Slack Bot開発で得たGASの知見について話します
-->


---
transition: fade-out
layout: two-cols-header
---

# 自己紹介

katzumi（かつみ）と申します。

「障害のない社会をつくる」をビジョンとする「LITALIC（りたりこ）」に所属しています
<a href="https://litalico.co.jp/">
<img src="https://litalico.co.jp/ogp.png" class="w-40" />
</a>

社内では SBO(Slack Bot Officer)を自称

以下のアカウントで活動しています。

::left::

<div class="float-left">
<img src="https://pbs.twimg.com/profile_images/1768978237210935296/idy9J4l6_400x400.jpg" class="rounded-full w-40 mr"/>  
<simple-icons-x /> <a href="https://twitter.com/katzchum">katzchum</a></div>  
<QRCode :width="180" :height="180" value="https://twitter.com/katzchum" color="4329B9" image="Logo_of_X.svg" />

::right::

<img src="https://avatars.githubusercontent.com/u/1182787?v=4" class="rounded-full w-40 mr-12"/>

<logos-github-octocat /> [k2tzumi](https://github.com/k2tzumi)  
<simple-icons-zenn /> [katzumi](https://zenn.dev/katzumi)  

<br />

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
はじめましてkatzumiと申します  
LITALICOで働いており、Slack Bot Officerを名乗っています
-->

---
layout: two-cols-header
transition: fade-out
---


# お願い 🙏

写真撮影、SNS での実況について

登壇者の励みになるので是非ともご意見やご感想など、フィードバック頂けると助かります mm  
あとでスライドを公開します

::left::

<Transform :scale="2.5">
　　　🙆‍♀📷<ph-projector-screen-chart-light /><br />
　　　🙅‍♂📹💸<br />
　　　🙅📸👨‍👦‍👦<br />
</Transform>

::right::

<br />
<Transform :scale="2">
<fa6-brands-square-x-twitter />
</Transform>
<br />
<a href="https://x.com/search?q=%23techramen25conf&f=live">#techramen25conf</a>


<!--
実況大歓迎でーす
-->

---
layout: section
---

# 今日のお題

<!--
本日のお題は
-->

---
layout: section
---

# Google Apps Script(GAS)の知見
立ち食いしてってね

<!--
GASです
-->

---
layout: two-cols-header
---

# 本日のスコープ
GAS の開発手法には２つのパターンがある

::left::

### 📊 コンテナバインド型
<v-click>

Google Workspace ファイルに紐づく

* Sheets, Docs, Forms 等に埋め込み
* シンプルトリガー（onOpen 等）でスクリプト実行
* WebApp 化は技術的に可能だが非推奨
* ファイル自動化・マクロ用途

</v-click>

<br />
<br />
<br />
<br />
<br />
<br />
<br />

::right::

### 🧱 スタンドアロン型
<v-click>
独立したスクリプトプロジェクト

* WebApp として公開可能
* 時間駆動トリガー（定期実行）のみ
* 外部からの HTTP リクエスト対応
* 色々ゴニョゴニョできる

</v-click>

<v-click>

<div class="speech-bubble">
⚠️ 今回はスタンドアロン型webappをベースとします！
</div>

</v-click>


---
layout: section
---

# 作ったSlack Bot(アプリ)

---
layout: section
---

# 12個のアプリ

---
layout: two-cols-header
---

# 作ったSlackBot達

::left::

#### 実務系

* [Mob Timer Bot](https://github.com/k2tzumi/mob-timer-bot)
対話的なモブプログラミングタイマー

* [勤怠Bot](https://github.com/k2tzumi/hue-kintai-slask-command)
勤怠システムと連動した打刻（開発停止中）

* [LGTM画像作成コマンド](https://github.com/k2tzumi/lgtm-slash-command)
LGTM 画像を作成する

#### 会話系

* [OpenAI Bot](https://github.com/k2tzumi/openai-slack-bot)
ChatGPT と Slack を繋ぐ Bot

* [echo bot](https://github.com/k2tzumi/slack-echo-bot)
invite されたチャネルの内容を別チャネルに echo する。times の集約に利用

::right::

#### 便利系

* [選んでコマンド](https://github.com/k2tzumi/choice-slash-command)
通称綾鷹コマンド

* [Pic Search](https://github.com/k2tzumi/pic-search-slash-command)
画像検索コマンド。ksk コマンド対応

* [お天気アプリ](https://github.com/k2tzumi/slack-jma-bot)
気象庁の API から天気等を取得する

#### 通知系

* [Emoji Webhook](https://github.com/k2tzumi/new_emoji_webhook)
新しい絵文字を通知

* [チャネル作成Webhook](https://github.com/k2tzumi/new_channel_webhook)
新しく作成されたチャネルを通知

#### URL展開

* [esa](https://github.com/k2tzumi/slack-esa-unfurling)
esa の URL を展開

* [Strava](https://github.com/k2tzumi/slack-strava-unfurling)
Strava のアクティビティをドヤるやつ

---
layout: section
---

# Why GAS?

---
layout: two-cols-header
---

# 趣味プログラミングの環境としてアリなのでは？
環境構築の面倒だったり、プログラミング以外のコストもかけたくない...

::left::

* 💳 クレカ登録不要  
Google アカウントがあれば  
*今すぐ無料で開始*

* 🚀 環境構築の手数が少ない  
設定作業が最小限  
実行環境の準備が不要  
バージョン管理が容易  
デプロイがシンプル  
**数ステップでデプロイ完了**

<br />

<br />
<br />
<br />
<br />
<br />

::right::

* 🛡️ フルマネージド  
サーバー管理・スケーリング・監視  
**Google任せで安心**  

* 💛 TypeScript 開発
clasp でローカル開発  
**型安全でモダンな開発**

---
layout: two-cols-header
---

# clasp導入によるモダン開発環境の構築
🚀　clasp で TypeScript 開発環境を手に入れよう

::left::

* **clasp導入のメリット**  
  * 普段遣いのローカルのエディタで開発できる  
Web ベースのスクリプトエディタからの卒業
  * コードを git 管理できる
  * Typescript で開発できる
  * デプロイバージョンの管理ができる

<v-click>

<div class="box-text-memo">
clasp v3はBreaking Changes があります<br />  
Typescript supportがDropされます<br />
Rollupを使ってのトランスパイルが必要になります
</div>

</v-click>

::right::
* **classpの導入方法**  
  * classp インストール
    ```console
    $ npm install -g @google/clasp
    ```
  * Typescript で開発  
    ```bash
    # TypeScript環境をセットアップ
    npm init -y
    npm install -D typescript @types/google-apps-script
    ```
    `tsconfig.json` 作成
      ```json
      {
        "compilerOptions": {
          "target": "ES2019",
          "lib": ["ES2019"],
          "outDir": "./dist",
          "rootDir": "./src"
        },
        "include": ["src/**/*"]
      }
      ```

---

# GASつらい課題
こんな壁があったりします

* npm ライブラリが使えない  
普通の JavaScript が動かない...

* Web アプリの実行モデルが同期  
レスポンスを返すと、この時点で実行終了。非同期処理ができない

* ログが見えない  
WebApp のログの確認方法が確立されていない  
デバッグが地獄..

* HTTP リクエストが特殊  
ホワイトリストで登録されたものしか
UserAgent を変更できない

* 画像加工が鬼門  
Canvas もなければライブラリもない

* データ永続化が特殊  
workspace のファイルをストレージ代わりにすることはできるけれど

---

# npmライブラリ使えない問題
独自ライブラリがあるけれどね

GAS は独自環境のため、Node.js やブラウザ向けの npm ライブラリは基本的に使用できません。  
モジュールシステム非対応です😅

Slack Bolt 使えません😭

GAS 向けの野良ライブラリもあるけれど

ここらへんの API 使えない

```js
// Node.js API（使用不可）
const fs = require('fs');           // ファイルシステム ❌
const path = require('path');       // パス操作 ❌
const http = require('http');       // HTTP サーバー ❌

// ブラウザAPI（使用不可）
document.getElementById('id');      // DOM操作 ❌
window.location.href;              // ブラウザ情報 ❌

// GAS専用API（使用可能）
SpreadsheetApp.getActiveSheet();   // スプレッドシート ✅
UrlFetchApp.fetch(url);           // HTTP通信 ✅
```

<!--
モジュールシステム非対応で、スクリプトファイル間で関数や変数を共有するスタイルです
-->
---

# 自前でSlack APIのクライアントを実装
Slack の Web API は [公開されている](https://api.slack.com/web) ので生暖かく `UrlFetchApp.fetch`

`UrlFetchApp.fetch` は GAS で HTTP 通信を行うための最重要 API です。  
外部 API との連携、Web スクレイピングなどで必須の機能です。

<Transform :scale="0.7">

```ts {*}{lines:true}
private invokeAPI(endPoint: string, payload: Record<never, never>): Response {
  let response: HTTPResponse;

  try {
    switch (this.preferredHttpMethod(endPoint)) {
      case "post":
        response = UrlFetchApp.fetch(
          endPoint,
          this.postRequestOptions(payload)
        );
        break;
      case "get":
        response = UrlFetchApp.fetch(
          this.formUrlEncoded(endPoint, payload),
          this.getRequestOptions()
        );
        break;
      default:
        throw new Error("Unknown method.");
    }
  } catch (e) {
    console.warn(`DNS error, etc. ${e.message}`);
    throw new NetworkAccessError(500, e.message);
  }
```
</Transform>

---

# SlackBotの３秒ルール問題
3 秒間レスポンスを返さないと

<blockquote>
<p>Acknowledgment response</p>
<p>All apps must, as a minimum, acknowledge the receipt of a valid interaction payload.</p>
<br />
<p>To do that, your app must reply to the HTTP POST request with an HTTP 200 OK response. This must be sent within 3 seconds of receiving the payload. If your app doesn't do that, the Slack user who interacted with the app will see an error message, so ensure your app responds quickly. Otherwise, the user won't see anything when your app only sends an acknowledgment response. If you want to do more, keep reading.</p>
</blockquote>
(参考: <a href="https://api.slack.com/interactivity/handling#acknowledgment_response">"Respond immediately to the initial request"</a>)


slash コマンドの実行時など問題になりがち  

---
layout: two-cols-header
---

# SlackBotの３秒ルール問題


::left::

3 秒を超えたら即エラー = Bot 死亡 💀

```ts　{*}{lines:true}
function doPost(e) {
  // SlackBotは3秒以内にレスポンス必須！
  
  // 外部API呼び出し（5秒かかる）
  const result = callExternalAPI(); // ⏰ 3秒超過
  
  // ここまで到達する前にSlackがエラー扱い
  return ContentService.createTextOutput(
    JSON.stringify({text: result})
  );
}
```

::right::

<v-click>

GAS は同期的実行モデル

```ts　{*}{lines:true}
// Webアプリのエンドポイント
function doGet() {
  // レスポンスをすぐに返す
  const output = 
    ContentService.createTextOutput(
      "リクエストを受け付けました。処理はバックグラウンドで開始されます..."
      );

  // ここで非同期処理をキックしようとする
  asyncFunctionThatWillBeTerminated();

  return output; // この時点でスクリプトの実行は終了する
}

async function asyncFunctionThatWillBeTerminated() {
  Logger.log("非同期処理を開始...");
  Utilities.sleep(10000); // 10秒待機
  Logger.log("非同期処理が完了しました！"); // **このログは出力されない**
}
```

</v-click>

<!--
でもレスポンスを返しちゃうと後続処理が終了してしまう
-->

---

# 簡易Jobキューシステムを作った
3 秒ルール回避 + 疑似非同期 = 生存 🎉

GAS の Trigger を使って簡単に非同期を行えるようにライブラリ化しました。  
TimeBased Trigger が JobBroker に実行を通知し、JobBroker がキャッシュされたパラメータを用いてアプリケーション本体の関数を非同期で実行します。
<OgpImage url="https://zenn.dev/katzumi/articles/58354fb4d05038" />


アプリケーションの globalThis をライブラリに渡すことで、ライブラリがアプリケーション本体のグローバル関数を動的に呼び出せます

<OgpImage url="https://zenn.dev/katzumi/articles/gas-library-globalthis-scope" />

---
layout: section
---

# 思いがけない副次効果

---

# Webアプリのログ問題が解消！！
デバッグが凄くやりやすくなりました

トリガー実行時にログ出力(`console.info()`)すると、ログ表示できます

<img src="./cloud-log.png" />

種類が webapp の場合は、GAS エディタ上でログ表示できません。  
本当は GCP を作って、StackDriver Logging（現 Cloud Logging）に出力させる必要があったみたい

<!--
他の代替手段ではスプレッドシートへの出力させたり、Google Cloud プロジェクトを作る手間が必要でした　　
デバックのしやすさを考えるとGASエディタ上で確認できるのでおすすめです。  
予期しない例外があったら非同期でログ出力するようにします。
-->

---

# HTTPリクエストが特殊
スクレイピングに制約

* UserAgent を更新できない  
bot として割と致命的  
```javascript
// GASで設定できないヘッダーの例
const options = {
  'method': 'GET',
  'headers': {
    'User-Agent': 'Custom Agent', // ❌ 無効
    'Host': 'example.com',        // ❌ 無効
    'Content-Length': '100',      // ❌ 無効
    'Accept': 'text/html',        // ✅ 有効
    'Accept-Language': 'ja-JP',   // ✅ 有効
    'Cookie': 'session=abc123',   // ✅ 有効
    'X-Custom-Header': 'value'    // ✅ 有効
  }
};
```

---

# HTTPリクエストが特殊
スクレイピングに制約

* リクエスト時の IP を固定できない  
GAS では、どのサーバーがスクリプトを実行するか、どの IP アドレスからアクセスするかを予測できません 
GAS からのリクエストは、Google の特定の IP アドレス範囲 ^[https://www.gstatic.com/ipranges/goog.jsonで公開されています] から送信されます。

---

# HTTPリクエストが特殊
スクレイピングに制約

* ホワイトリストに登録していない URL にアクセスできない ^[参考: https://developers.google.com/apps-script/manifest/allowlist-url]  
マニフェストファイル（`appsscript.json`）に urlFetchWhitelist フィールドを含めます
```json {*}{lines:true}
{
  "timeZone": "Asia/Tokyo",
  "dependencies": {},
  "exceptionLogging": "STACKDRIVER",
  "runtimeVersion": "V8",
  "urlFetchWhitelist": [
    "https://slack.com/",
    "https://api.slack.com/",
    "https://hooks.slack.com/",
    "https://www.googleapis.com/"
  ]
}
```

---

# 諦めて他のFaaSにリクエスト処理を移譲
どう頑張っても UserAgent は変えられない

勤怠 Bot では UserAgent で弾かれたので、仕方無しに Netlify 経由でアクセスするようにしました

<OgpImage url="https://github.com/k2tzumi/works-kintai-slask-command-netlify-functions" />

使えていたのですが、認証時のセキュリティが強化された ^[CanvasでのFingerPrintを取得] 為、開発停滞中  
ヘッドレスブラウザ利用も GAS では難しい

----

# 画像加工も苦労した
LGTM 画像を作りたかった

インチキしています。Cloudinary が使いやすかったです 😅
<OgpImage url="https://zenn.dev/katzumi/articles/0e9c0a49258afc" />  


Pure JS の画像編集ライブラリを WebPack でバンドル。トランスパイルして GAS で動かそうと試みた...
<OgpImage url="https://github.com/k2tzumi/lgtm-slash-command-npm-library-based" />

<!--
ポリフィルが上手く行かず断念
-->

---

# 画像レスポンスも工夫が必要
ContentService.setMimeType できるけれど、CSV, iCal, JavaScript, JSON, Text, vCard のみ

GAS では画像を直接レスポンス出来ないので、Google Drive にアップロードして共有 URL を作成して配信させる

<OgpImage url="https://github.com/k2tzumi/slack-strava-unfurling" />  

---

# セッション管理も時前で行う必要がある
データの永続化が特殊

* PropertiesService  
KVS（Key-Value Store）として利用  
スクリプト単位で永続化  
JSON 文字列で複雑なデータも保存  
容量制限：500KB  
* Hidden パラメータ持ち回り  
Slack の Interactive Components に hidden データを埋め込んで状態管理  
Slack ならユーザーには見えづらいので、やりやすい


---

# まだまだできないこと

* リダイレクトができない   
HTML をレスポンスさせて強制的にリフレッシュさせることはできるけれど、使い所が限定的  
`addOns.common.openLinkUrlPrefixes` の指定も必要

---

# 将来の可能性

<OgpImage url="https://zenn.dev/mascii/articles/go-wasm-on-google-apps-script" />  

<!--
GASでWasm利用の事例をご存知の方がいれば、ぜひ教えてください！
-->

---
layout: end
---

ご清聴ありがとうございました

<!--
ご清聴ありがとうございました
-->
