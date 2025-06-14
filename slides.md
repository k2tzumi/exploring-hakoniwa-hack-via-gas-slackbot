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
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
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


<!-- 本セッションでは、撮影やSNS拡散を歓迎しています。ご自由に写真を撮影して、XなどのSNSでシェアしてください。 　　
ただし、以下の点にご注意ください。　　

著作権などの法的な問題を避けるために、スライドや登壇者の写真や動画を無断で商用利用しないでください。　　
他の参加者のプライバシーや迷惑にならないように、撮影や投稿する際には配慮してください。　　
SNSでシェアする際には、ハッシュタグ「#phpcon_nagoya #s」をつけてください。　　
これにより、本セッションの関連情報を簡単に検索できるようになります。 -->

---
layout: section
---

# 今日のお題

---
layout: section
---

# Google Apps Script(GAS)の知見
立ち食いしてってね

---
layout: two-cols-header
---

# 本日のスコープ
GASの開発手法には２つのパターンがある

::left::

### 📊 コンテナバインド型
<v-click>

Google Workspace ファイルに紐づく

* Sheets, Docs, Forms 等に埋め込み
* シンプルトリガー（onOpen等）でスクリプト実行
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
* 外部からのHTTPリクエスト対応
* 色々ゴニョゴニョできる

</v-click>

<v-click>

<div class="speech-bubble">
⚠️ 今回はスタンドアロン型webappをベースとします！
</div>

</v-click>

---
layout: two-cols-header
---

# clasp導入によるモダン開発環境の構築
🚀　claspでTypeScript開発環境を手に入れよう

::left::

* **clasp導入のメリット**  
  * 普段遣いのローカルのエディタで開発できる  
Webベースのスクリプトエディタからの卒業
  * コードをgit管理できる
  * Typescriptで開発できる
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
  * classpインストール
    ```console
    $ npm install -g @google/clasp
    ```
  * Typescriptで開発  
    ```bash
    # TypeScript環境をセットアップ
    npm init -y
    npm install -D typescript @types/google-apps-script
    ```
    `tsconfig.json`作成
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


https://github.com/fossamagna/gas-webpack-plugin
