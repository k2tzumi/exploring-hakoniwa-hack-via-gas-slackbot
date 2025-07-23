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

# <carbon-user-avatar /> 自己紹介

katzumi（かつみ）と申します。

「障害のない社会をつくる」をビジョンとする「LITALICO（りたりこ）」に所属しています
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
LITALICOという会社で、社内ではSlack Bot Officerとして活動しています。  
好きなラーメンは油そばとベジポタラーメンです
-->

---
layout: two-cols-header
transition: fade-out
---

# <carbon-information /> お願い 🙏

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
<a href="https://x.com/search?q=%23techramen25conf%20%23shoyu&f=live">#techramen25conf #shoyu</a>

<!--
皆さんの好きなラーメンを呟いてください！
-->

---
layout: section
---

# <carbon-bullhorn /> 今日のお題

<!--
-->

---
layout: section
---

# <simple-icons-googleappsscript /> Google Apps Script(GAS)の
# 知見
立ち食いしてってね

<!--
本日はGoogle Apps Script (GAS) の知見についてお話しします
-->

---
layout: two-cols-header
---

# <f7-scope /> 本日のスコープ
GAS の開発手法には２つのパターンがある

::left::

### <octicon-container-16 /> コンテナバインド型
<v-click>

<logos-google-workspace /> ファイルに紐づく

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

### <line-md-file-off-twotone /> スタンドアロン型
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

<!--
GASの開発手法には2つのパターンがあります。  
[click] Google Workspaceのファイルに紐づいて動くコンテナバインド型と

[click] 独立したスクリプトプロジェクトとして動くスタンドアロン型です。  

[click] 今回はスタンドアロン型のWebAppをベースにお話しします。
-->

---
layout: section
---

# <devicon-slack /> 作ったSlack Bot(アプリ)

<!--
私が作ったSlack Botを軽く紹介させていただきますと
-->

---
layout: section
---

# <f7-number-circle /> 12個のアプリ

<!--
現在12個ほど作成しており
-->

---
layout: two-cols-header
---

# <devicon-slack /> 作ったSlack Botたち <carbon-chat-bot />

::left::

#### <fa-cogs /> **実務系**

* [Mob Timer Bot](https://github.com/k2tzumi/mob-timer-bot)
対話的なモブプログラミングタイマー

* [勤怠Bot](https://github.com/k2tzumi/hue-kintai-slask-command)
勤怠システムと連動した打刻（開発停止中）

* [LGTM画像作成コマンド](https://github.com/k2tzumi/lgtm-slash-command)
LGTM 画像を作成する

#### <carbon-chat /> **雑談系**

* [OpenAI Bot](https://github.com/k2tzumi/openai-slack-bot)
ChatGPT と Slack を繋ぐ Bot

* [echo bot](https://github.com/k2tzumi/slack-echo-bot)
invite されたチャネルの内容を別チャネルに echo する。times の集約に利用

::right::

#### <carbon-tool-kit /> **便利系**

* [選んでコマンド](https://github.com/k2tzumi/choice-slash-command)
通称綾鷹コマンド

* [Pic Search](https://github.com/k2tzumi/pic-search-slash-command)
画像検索コマンド。ksk コマンド対応

* [お天気アプリ](https://github.com/k2tzumi/slack-jma-bot)
気象庁の API から天気等を取得する

#### <carbon-notification /> **通知系**

* [Emoji Webhook](https://github.com/k2tzumi/new_emoji_webhook)
新しい絵文字を通知

* [チャネル作成Webhook](https://github.com/k2tzumi/new_channel_webhook)
新しく作成されたチャネルを通知

#### <carbon-link /> **URL展開**

* [esa](https://github.com/k2tzumi/slack-esa-unfurling)
esa の URL を展開

* [Strava](https://github.com/k2tzumi/slack-strava-unfurling)
Strava のアクティビティをドヤるやつ

<!--
実務系、便利系、雑談系など、幅広いレパートリーがあります  
社内で一番野良Botを生やしていると自負しています
-->

---
layout: section
---

# <tabler-mood-puzzled /> Why GAS?

<!--
なぜGASを選んだかというと、
-->

---
layout: two-cols-header
---

# <carbon-code /> 趣味プログラミングの環境としてアリなのでは？
環境構築の面倒だったり、プログラミング以外のコストもかけたくない...

::left::

* <noto-credit-card /> **クレカ登録不要**  
  Google アカウントがあれば  
  *今すぐ無料で開始*

* <logos-aws-codedeploy /> **環境構築の手数が少ない**  
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

* <carbon-managed-solutions /> **フルマネージド**
  サーバー管理・スケーリング・監視  
  **Google任せで安心**  

* <devicon-typescript /> **TypeScript 開発**  
  clasp でローカル開発  
  **型安全でモダンな開発**

<!--
趣味プログラミングの環境として非常に優れていると考えているからです。  

クレジットカード登録が不要で無料で開発でき、Googleアカウントさえあればすぐに始められます。  

環境構築の手間も少なく、約5年間Botを運用していますが、Google側で問題が起きて止まったということがなく、心の平穏が保たれて良きです。

また、TypeScriptでの開発も可能で、言語学習の良いお題だと考えました。開発を始めて最初の壁にぶち当たるまでは。。
-->

---
layout: two-cols-header
---

# <carbon-terminal /> clasp導入によるモダン開発環境の構築
🚀　clasp で TypeScript 開発環境を手に入れよう

::left::

* <icon-park-outline-good-one />**clasp導入のメリット**  
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
* <entypo-install /> **classpの導入方法**  
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

<!--
claspというCLIツールを使えば、TypeScriptで開発できるようになります。

[click] ただし、最近v3のリリースが計画されており、Breaking Changesがあり、TypeScriptのサポートがドロップされるようです。  
自前でトランスパイルが必要になりそうです。
-->

---
layout: two-cols-header
---

# <mdi-wall-fire /> GASつらい課題
こんな壁があったりします

::left::

* <carbon-package /> npm ライブラリが使えない  
  普通の JavaScript が動かない...

* <mdi-sign-direction /> Web アプリの実行モデルが同期  
  レスポンスを返すと、この時点で実行終了。非同期処理ができない

* <tabler-sunglasses /> ログが見えない  
  WebApp のログの確認方法が確立されていない  
  デバッグが地獄..

* <carbon-network-4 /> HTTP リクエストが特殊  
  ホワイトリストで登録必須    
  UserAgent を変更できない

::right::

* <carbon-image-search /> 画像加工が鬼門  
  Canvas もなければライブラリもない

* <carbon-save /> データ永続化が特殊  
  workspace のファイルをストレージ代わりにできるけれど

<!--
GASにはいくつか辛い課題がありました。  
色々なBotを作っていく中で、ここに挙げたような課題を乗り越えたり、乗り越えられななかったりした知見を話します
-->

---

# <carbon-package /> npmライブラリ使えない問題
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
まず「npmライブラリが使えない問題」です。  

GASは独自実行環境のため、Node.jsやブラウザ向けのnpmライブラリは基本的に使えません。  
モジュールシステムが非対応であるため、Slack Boltのようなフレームワークも使えません。
-->

---

# <carbon-code-reference /> 自前でSlack APIのクライアントを実装
Slack の Web API は [公開されている](https://api.slack.com/web) ので生暖かく `UrlFetchApp.fetch`

GAS では `UrlFetchApp` 以外に HTTP 通信を行う手段はありません。  
Google Workspace 以外の外部サービスとの連携や Web スクレイピングに欠かせません。

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

<!--
ではどうしたかというと、自前でSlack APIのクライアントを実装しました。  
SlackのWeb APIは公開されているので、UrlFetchAppを使って書いていく形です。  
これはGASで外部サービス連携やWebスクレイピングする為の唯一の手段です。
-->

---

# <carbon-timer /> Slack Botの３秒ルール問題
3 秒間レスポンスを返さないと

<blockquote>
<p>Acknowledgment response</p>
<p>All apps must, as a minimum, acknowledge the receipt of a valid interaction payload.</p>
<br />
<p>To do that, your app must reply to the HTTP POST request with an HTTP 200 OK response. This must be sent within 3 seconds of receiving the payload. If your app doesn't do that, the Slack user who interacted with the app will see an error message, so ensure your app responds quickly. Otherwise, the user won't see anything when your app only sends an acknowledgment response. If you want to do more, keep reading.</p>
</blockquote>
(参考: <a href="https://api.slack.com/interactivity/handling#acknowledgment_response">"Respond immediately to the initial request"</a>)


slash コマンドの実行時など問題になりがち

<!--
次に「Slack Botの3秒ルール問題」です。
-->

---
layout: two-cols-header
---

# <carbon-timer /> Slack Botの３秒ルール問題


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
3秒以内にレスポンスを返さないとエラーになります。  

[click] GASは同期実行モデルであるため、レスポンスを返すとその時点でスクリプトが終了し、非同期処理中でも強制終了される問題がありました。
-->

---

# <carbon-cics-system-group /> 簡易Jobキューシステムを作った
3 秒ルール回避 + 疑似非同期 = 生存 🎉

GAS の Trigger を使って簡単に非同期を行えるようにライブラリ化しました。  
TimeBased Trigger が JobBroker に実行を通知し、JobBroker がキャッシュされたパラメータを用いてアプリケーション本体の関数を非同期で実行します。
<OgpImage url="https://zenn.dev/katzumi/articles/58354fb4d05038" />


アプリケーションの globalThis をライブラリに渡すことで、ライブラリがアプリケーション本体のグローバル関数を動的に呼び出せます

<OgpImage url="https://zenn.dev/katzumi/articles/gas-library-globalthis-scope" />

<!--
これに対し、簡易Jobキューシステムを自作しました。  
GASのトリガーを使って簡単に非同期処理を行えるようにライブラリ化しました。  
Triggerがライブラリに実行を通知し、ライブラリ側がアプリケーション側の関数をキャッシュされたパラメーターで非同期で呼び出す仕組みです。  

詳細は記事がありますので、そちらをご覧ください。  
また、小ネタですが、アプリケーションのglobalThisをライブラリに渡すことで、アプリケーション本体のグローバル関数をコールバックさせることができます。
-->

---
layout: section
---

# <carbon-magic-wand /> 思いがけない副次効果

<!--
そして、思いがけない副次効果もありました。
-->

---

# <carbon-document-preliminary /> Webアプリのログ問題が解消！！
デバッグが凄くやりやすくなりました

トリガー実行時にログ出力(`console.info()`)すると、ログ表示できます

<img src="/cloud-log.png" />

種類が webapp の場合は、GAS エディタ上でログ表示できません。  
本当は GCP を作って、StackDriver Logging（現 Cloud Logging）に出力させる必要があったみたい

<!--
Webアプリのログが見れない問題が解消しました。  
トリガー実行時にconsole.info()でログを出力すると、ログが表示できるようになりました。  

WebAppの種類だとGASエディタ上でログ表示できませんが、トリガー実行の場合は可能です。  
本来はGCPプロジェクトを作成してCloud Loggingへ出力させる必要があります。先程紹介したライブラリと組み合せて、この方法であればセットアップや課金も気にせず、簡単にログを見ることができます。
-->

---

# <carbon-network-4 /> HTTPリクエストが特殊
スクレイピングに制約

* <mdi-pencil-off-outline /> UserAgent を更新できない  
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

<!--
「HTTPリクエストが特殊」という課題もあります。  
User-Agentを更新できなかったり
-->

---

# <carbon-network-4 /> HTTPリクエストが特殊
スクレイピングに制約

* <carbon-network-public /> リクエスト時の IP を固定できない  
GAS では、どのサーバーがスクリプトを実行するか、どの IP アドレスからアクセスするかを予測できません 
GAS からのリクエストは、Google の特定の IP アドレス範囲 ^[https://www.gstatic.com/ipranges/goog.jsonで公開されています] から送信されます。

<!--
リクエスト元のIPアドレスが固定できなかったりします。
-->

---

# <carbon-network-4 /> HTTPリクエストが特殊
スクレイピングに制約

* <carbon-rule /> ホワイトリストに登録していない URL にアクセスできない ^[参考: https://developers.google.com/apps-script/manifest/allowlist-url]  
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

<!--
また、マニフェストファイルの urlFetchWhitelist にURLを登録しないと、リクエストを送信できません。
-->

---

# <carbon-cloud-upload /> 諦めて他のFaaSにリクエスト処理を移譲
どう頑張っても UserAgent は変えられない

勤怠 Bot では UserAgent で弾かれたので、仕方無しに Netlify 経由でアクセスするようにしました

<OgpImage url="https://github.com/k2tzumi/works-kintai-slask-command-netlify-functions" />

使えていたのですが、認証時のセキュリティが強化された ^[CanvasでのFingerPrintを取得] 為、開発停滞中  
ヘッドレスブラウザ利用も GAS では難しい

<!--
User-Agentを変更できない問題については、他のFunction as a Serviceにリクエスト処理を移譲しました。
-->

---

# <carbon-image-search /> 画像加工も苦労した
LGTM 画像を作りたかった

インチキしています。Cloudinary が使いやすかったです 😅
<OgpImage url="https://zenn.dev/katzumi/articles/0e9c0a49258afc" />  


Pure JS の画像編集ライブラリを WebPack でバンドル。トランスパイルして GAS で動かそうと試みた...
<OgpImage url="https://github.com/k2tzumi/lgtm-slash-command-npm-library-based" />

<!--
画像加工も苦労しました　　
LGTM画像を作りたかったのですが、Canvasや画像加工ライブラリがGASにはないため、外部サービスを利用しました。  
Pure JavaScriptの画像編集ライブラリをWebpackでトランスパイルしてGASで動かそうと試みましたが、うまくいきませんでした。
-->

---

# <carbon-image /> 画像レスポンスも工夫が必要
ContentService.setMimeType できるけれど、CSV, iCal, JavaScript, JSON, Text, vCard のみ

GAS では画像を直接レスポンス出来ないので、Google Drive にアップロードして共有 URL を作成して配信させる

<OgpImage url="https://github.com/k2tzumi/slack-strava-unfurling" />

<!--
「画像レスポンスも工夫が必要」です。　　
setMimeTypeで設定できるMIMEタイプは厳密に定められており、画像は直接レスポンスできません。  
それを回避する為にGoogle Driveにアップロードして共有URLを作成し、それを配信するという手法を取りました。
-->

---

# <carbon-save /> セッション管理も時前で行う必要がある
データの永続化が特殊

* <carbon-data-base /> PropertiesService  
  KVS（Key-Value Store）として利用  
  スクリプト単位で永続化  
  JSON 文字列で複雑なデータも保存  
  容量制限：500KB  

* <carbon-password /> Hidden パラメータ持ち回り  
  Slack の Interactive Components に hidden データを埋め込んで状態管理  
  Slack ならユーザーには見えづらいので、やりやすい

<!--
セッション管理も自前で行う必要がありました。  
PropertiesServiceをKVSとして利用したり、SlackのInteractive Componentsにパラメータを埋め込んで状態管理をしたり、自前で実装しました
-->

---

# <carbon-construction /> まだまだ壁があります

* <carbon-unlink /> リダイレクトができない  

  HTML をレスポンスさせて強制的にリフレッシュさせることはできますが、使い所が限定的です  
  `addOns.common.openLinkUrlPrefixes` の指定も必要

<!--
まだできないこともあります。例えば「リダイレクト」です。  
HTMLをレスポンスさせて強制的にリフレッシュさせることはできますが、使い所が限定的です。
-->

---

# <carbon-view /> 気になっていること  
ライブラリ問題に一石を投じるか！？

<OgpImage url="https://qiita.com/tofu-k/items/433072aaa44805c3bae9" />

<OgpImage url="https://zenn.dev/mascii/articles/go-wasm-on-google-apps-script" />

<!--
最後に、個人的に注目していくこととして、WebAssemblyです。  
WASM＋GASの知見をお持ちの方がいらっしゃいましたら、ぜひお話させていただければと思います。
-->

---
layout: end
---

# <carbon-face-satisfied /> ご清聴ありがとうございました

<!--
ご清聴ありがとうございました
-->
