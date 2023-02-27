---
title: Discord Bot チュートリアル
description:
img: first-blog-post.jpg
category: dev
---

こんにちは。B3のArcです。2022年2月18日に部内でDiscord Botの開発チュートリアルを行いました。当時の資料を元に加筆・修正を加えた物を公開してみます。

# 近頃のDiscord Bot界隈

最初に、近頃のDiscord Bot開発者界隈を取り巻く状況について軽く触れておきます。

## むかし

昔はBot開発用の2大ライブラリとして、Python用の[discord.py](https://discordpy.readthedocs.io/en/latest/)とNode.js用の[discord.js](https://discord.js.org/)がありました。

## 2021年8月28日

この日にdiscord.pyの開発終了が[アナウンスされました](https://gist.github.com/Rapptz/4a2f62751b9600a31a0d3c78100287f1)。Discord公式とのトラブルがあったようです(詳しくはググってね)。後述しますが、*現在は開発が再開されたようです。*

これにより、Discord公式のREST APIの仕様変更などに対応できなくなったdiscord.py製botは死ぬ運命となりました(結果的にそうならなかったけど)。また、discord.pyの後継を名乗るプロジェクトが林立する事態となりました。

Python界隈がこの有様(だった)ので、このチュートリアルではdiscord.jsを使っていきます。

## 2022年3月6日

……と思っていたら3月6日にdiscord.pyの開発**再開**が[アナウンスされました](https://gist.github.com/Rapptz/c4324f17a80c94776832430007ad40e6)。~~何が何だか分からん~~　再び2大ライブラリ時代に戻るんじゃないかなと思っています。たぶん……

# 環境構築・初期設定

この章では、Discord Botを作るための環境構築・初期設定を行います。

## 用語集

初めに、基本的な用語について説明しておきます。

- Node.js
  - サーバーサイドJavaScript環境(ブラウザではなくサーバー上でJSを動かすための環境)の一種です。Discord BotはNode.js上で動きます。
  - パッケージマネージャとして`npm`という物を使います。
  - 最新のdiscord.js(v13)を使うためには、Node v16.6.0以降が必要です。
  - [公式サイト](https://nodejs.org/ja/)
- TypeScript
  - JavaScriptを静的型付けライクに拡張した言語です。静的型付け最高！
  - TypeScriptは直接実行することができないため、基本的には`tsc`というコンパイラでJavaScriptに変換してから実行します。
  - 文法はCやJavaなどに近い感じです。
  - [公式サイト](https://www.typescriptlang.org/)

## Node.jsのインストール

[ダウンロードページ](https://nodejs.org/ja/download/)から、Node.jsのLTS版をダウンロード・インストールしてください。

ターミナルで`node --version`を実行した際、`v16.14.0`のように表示されていればインストール成功です。

## プロジェクトディレクトリの作成

適当な名前のディレクトリを作ってください。

次に、ターミナルでディレクトリに移動し、`npm init`を実行してください。色々聞かれますが、とりあえず全てEnterを押せばいいです。

`package.json`が作成されていることを確認してください。

## パッケージのインストール

開発に必要なパッケージをインストールします。次のコマンドを実行してください。

```bash
npm install --save discord.js dotenv typescript @types/node@16 ts-node tsconfig-paths
```

## TypeScriptの設定

`npx tsc --init`を実行してください。`tsconfig.json`が生成されれば成功です。

`tsconfig.json`には、TypeScriptのコンパイラ(TSC)の設定情報が含まれています。

`tsconfig.json`には設定項目が大量にありますが、中身については省略します。`baseUrl`の項目だけ次のように変えてください。

```jsonc
{
  // ...
  "baseUrl": "./src",
  // ...
}
```

## スクリプトの設定

`package.json`の`"scripts"`の箇所を次のように編集してください。

```jsonc
{
  // ...
  "scripts": {
    "start": "ts-node --files -r tsconfig-paths/register src/index.ts"
  },
  // ...
}
```

これにより、`npm start`で`npx ts-node --files -r tsconfig-paths/register src/index.ts`を実行できるようになりました。

`ts-node`は、`.ts`ファイルを事前コンパイル無しで直接実行するためのパッケージです(正確にはJITコンパイルしているようです)。TypeScriptのコードを走らせるには「tscでコンパイル」→「Nodeでjsを実行」 の2ステップを要していましたが、`ts-node`を使うことで1ステップに抑えることができます。

いろいろ引数がついていますが、ここでは説明を省略します。気になる方は`tsconfig-paths`などで検索してください。

## Hello World

`src`ディレクトリを作成し、その中に`index.ts`を作成してください。

そして、次のコードをコピペしてください。

```typescript
console.log("Hello World!")
```

最後に、プロジェクトルートに居る状態で`npm start`を実行してください。`Hello World!`と表示されれば成功です！

# Botの登録・ログイン

この章では、Discord Botをサーバーに追加した後、Botをサーバーにログインさせます。

概ね[Discord.js Guide](https://guide.discordjs-japan.org/) v12版に沿っていますが、現行のv13に合わせて記述を変更している箇所もあります。

## Botをサーバーに追加する

Botをサーバーに追加していきます。最初にテスト用のサーバーを立てておいてください。

### Botの登録

まずはBotをDiscordに登録します。

1. Discordにログインした状態で、Discord Developer Portalの[Application](https://discord.com/developers/applications/)ページに移動してください。
2. 右上の`New Application`をクリックして、いい感じの名前をつけて`Create`してください。

<img src="/works/discord-tutorial/create_app.png">


3. 左のメニューから`Bot`をクリックして、`Add Bot -> Yes, do it!`をクリックしてください。`A wild bot has appeared!`みたいなメッセージが表示されればOKです。

<img src="/works/discord-tutorial/add_bot.png">

### Botをサーバーに招待

次に、Botをサーバーに招待します。

1. 左メニューから`OAuth2 -> URL Generator`を開いてください。
2. `SCOPES`の中の`bot`, `applications.commands`にチェックを入れてください。
3. `BOT PERMISSIONS`の中の`Send Messages`にチェックを入れてください。これにより、Botにメッセージ送信権限が付与されます。他の権限が必要な場合は適宜チェックを増やしてください。

<img src="/works/discord-tutorial/permission.png">

4. 下の方にある`Generated URL`をコピーして、Webブラウザに貼り付けてください。事前に作成したサーバーを選択して、「はい」などのボタンを押してください。
5. Discordクライアント上で、サーバーにBotが追加されたことを確認してください。


## Botをサーバーにログインさせる

### 環境変数とdotenv

これからBotをサーバーにログインさせますが、その前に環境変数について説明しておきます。

Botをログインさせる際には「トークン」という値が必要になります。この値が外部に流出した場合、他の人がBotをサーバーにログインさせて、スパムメッセージを送信するなどの悪事を働くことができてしまいます。このため、トークンはソースコードに**埋め込まずに**管理する必要があります(トークンを埋め込んだソースコードをGitにPushしたら外部流出したことになります)。

このような機密情報は、実行環境の**環境変数**に設定して、ソースコードから環境変数を読み込む必要があります。しかし、環境変数を設定するのは割とだるいです。

Nodeでは、実行時に`.env`というファイルから環境変数を読み込んで設定する`dotenv`というモジュールがあります。実はこのモジュールは前章でインストールしていました。`.env`ファイルを`.gitignore`に設定することで、機密情報を安全に、便利に扱うことができます。

トークンを`.env`ファイルに保存しておきましょう。`.env`をルートディレクトリに作成して、次の内容を書き込んでください。
```
TOKEN=<Botのトークン>
```
`<Botのトークン>`の箇所を実際の値で置き換えてください。Botのトークンは次のようにして得ることができます。

1. Developer Portalから前節で作成したアプリケーションを選択し、左メニューの`Bot`を選択します。
2. `TOKEN`という所に`Copy`ボタンがあるのでクリックしてください。トークンがクリップボードにコピーされます。



### ログイン処理の実装

Botをサーバーにログインさせる処理を書いていきます。

```typescript
// 1: インポート
import * as dotenv from "dotenv";
import { Client, ClientOptions } from "discord.js";

// 2: .envを読み込み、環境変数に登録
dotenv.config();

// 3: クライアントを初期化
const clientOptions: ClientOptions = {
  intents: ['GUILD_MESSAGES', 'GUILDS'],
}
const client = new Client(clientOptions)

// 4: ログイン完了時に実行するコールバック関数を登録
client.once("ready", () => {
  console.log("I'm  ready!");
});

// 5: ログイン
client.login(process.env.TOKEN);
```

コードの中身を説明していきます。

#### インポート

使用するクラスなどをインポートします。

#### .envを読み込み、環境変数に登録

`.env`を読み込んで、環境変数に登録します。環境変数へは`process.env.<変数名>`でアクセスすることができます。

#### クライアントを初期化

- `v12`以前と異なり、現行の`v13`では`ClientOptions`が必須になっています。
- `ClientOptions`の`intents`で、どのイベントをBotが受信するかを制御します。ここでは上記2つを指定しておけば良いでしょう。
- 最後にクライアントのインスタンスを作成します。

#### ログイン完了時に実行するコールバック関数を登録

- ログイン完了時に`ready`イベントが発火されます。その時に実行する関数(コールバック関数)を登録しておきます。
- `()=>{...}`という表記は、JS・TS特有の「アロー関数」という物です。(詳しくはググってね)

#### ログイン

- 環境変数`TOKEN`の値を引数にしてログインします。

### 実行してみる

ここまで書けたら実際に実行してみましょう。

`npm start`を実行して数秒待ってください。次のことが確認できれば成功です！

- コンソールに`I'm ready!`と表示される。
- Discordアプリ上で、登録したBotがオンラインになっている。

確認できたら、Ctrl+CなどでBotを止めておいてください。

# スラッシュコマンド

この章では、スラッシュコマンドの実装を行います。

この記事を参考にしています: [discord.js でスラッシュコマンド（Slash commands）を使う - Qiita](https://qiita.com/gaato/items/55b32bc4777905ac162a)

## 2種類のスラッシュコマンド

スラッシュコマンドには「ギルドコマンド」と「グローバルコマンド」の2種類があります。

- ギルドコマンド: 特定のサーバーを指定して登録するコマンド。
- グローバルコマンド: Botが参加している全てのサーバーに登録されるコマンド。

グローバルコマンドは、登録後実際に使えるようになるまで1時間ほどかかるようです。このため、本チュートリアルではギルドコマンドを使用します。

## 環境変数の設定

ギルドコマンドを登録するためには、対象サーバーのIDが必要です。`.env`に保存しておきます。

サーバーIDは次の方法で取得することができます。

1. Discordアプリの設定画面を開き、「詳細設定」の「開発者モード」をオンにしておきます。

<img src="/works/discord-tutorial/devmode.png">

1. サーバーアイコン上で右クリックして、「IDをコピー」を選択します。サーバーIDがクリップボードにコピーされます。

<img src="/works/discord-tutorial/copy_server_id.png">

`.env`に次の項目を追記してください。`<サーバーID>`は実際の値に置き換えてください。
```
SERVER_ID=<サーバーID>
```

## シンプルなコマンド

まず、`pong`というメッセージを送信するだけのシンプルなコマンド`/ping`を作成してみます。

### 環境変数の型定義を記述する

環境変数の型定義ファイルを作っておかないと後々面倒なので、ここで作ってしまいます。

`src`に`@types`ディレクトリを作成して、中に`global.d.ts`を作り、以下の内容を入力してください。

```typescript
declare namespace NodeJS {
  interface ProcessEnv {
    readonly TOKEN: string;
    readonly SERVER_ID: string;
  }
}
```

### コマンドを実装する

実際にコマンドを作っていきます。次のコードを参考にして、`ready`イベントのコールバックを編集し、`interactionCreate`イベントのコールバックを追加してください。`async`キーワードが新しく加わっていることに注意してください。

```typescript
import { ApplicationCommandData, Client, ClientOptions } from "discord.js"; // インポート部分が変わっています

// ...

// ログイン完了時に実行するコールバック関数を登録
client.once("ready", async () => {
  const commands: ApplicationCommandData[] = [{
    name: "ping",
    description: "pongと返します。"
  }]
  await client.application?.commands.set(commands, process.env.SERVER_ID)

  console.log("I'm  ready!");
});

// コマンド受信時のコールバック関数を登録
client.on("interactionCreate", async (interaction) => {
  if (!interaction.isCommand()) {
    return;
  }
  if (interaction.commandName === "ping") { // pingコマンドが来たら:
    await interaction.reply("pong"); // pongとreplyする
  }
});
```

書けたら`npm start`して、アプリ上でコマンドを使えるかどうか確認してみましょう。

## 引数付きコマンド

引数を受け取るコマンドを作っていきます。ここでは、引数をそのまま出力する`echo`コマンドを作っていきます。

### コマンドの登録

次のコードを参考にして、`commands`に要素を追加してください。引数は`options`プロパティで設定します。

```typescript
const commands: ApplicationCommandData[] = [
    {
      name: "ping",
      description: "pongと返します。",
    },
    {
      name: "echo",
      description: "入力された文字をそのまま返します。",
      options: [
        {
          type: "STRING",
          name: "value",
          description: "文字列",
          required: true,
        },
      ],
    },
  ];
```

### コマンドの中身を実装

次のコードを参考にして、コマンドの中身を実装してください。引数は`interaction.options.getString()`などで受け取ることができます。

```typescript
if (interaction.commandName === "ping") {
  await interaction.reply("pong");
}
if (interaction.commandName === "echo"){
  const value = interaction.options.getString('value', true) // 引数valueを受け取る
  await interaction.reply(value)
}
```

書けたら`npm start`して、アプリ上でコマンドを使えるかどうか確認してみましょう。

## 遅延応答

「コマンドを受信したら、データベースにアクセスして処理をした後に返信したい」といったケースなど、時間のかかる処理を実装したくなることがあると思います。このような処理をそのまま書いたらどうなるでしょうか？

`ping`コマンドを次のように編集してみます。ここでは4000ミリ秒後に"pong"と返信するコードを書いています。

```typescript
if (interaction.commandName === "ping") {
  setTimeout(async ()=>{
    await interaction.reply("pong");
  }, 4000) // 4000ミリ秒後に"pong"と返信
}
```

`ping`コマンドを実際に実行してみましょう。すると、`アプリケーションが応答しませんでした`などのメッセージが表示されると思います。

実は、スラッシュコマンドは**3秒以内に返信しないとエラー**になります。

これを回避するため、`deferReply()`, `followUp()`という関数を使います。

```typescript
if (interaction.commandName === "ping") {
  await interaction.deferReply(); // 追加
  setTimeout(async ()=>{
    await interaction.followUp("pong"); // 変更
  }, 4000) // 4000ミリ秒後に"pong"と返信
}
```

このコードにした上で`ping`コマンドを実行すると、`<bot名>が考え中…`というメッセージが表示された4秒後に`pong`と表示されるはずです。意図通りの挙動になりました。

ということで、時間のかかる処理を行う時には、返信を遅延させるために`deferReply()`や`followUp()`を使う必要があります。

詳しくはこのページが参考になります: [replyとdeferReplyの違い - Discord.js Japan User Group](https://scrapbox.io/discordjs-japan/reply%E3%81%A8deferReply%E3%81%AE%E9%81%95%E3%81%84)

# 分からないことがあったら

分からないことがある時は次のサイトが頼りになります。

- [Discord.js Guide](https://discordjs.guide/#before-you-begin)
- [discord.js Documentation](https://discord.js.org/#/docs/discord.js/stable/general/welcome)
- [Discord.js Japan User Group (Scrapbox)](https://scrapbox.io/discordjs-japan/)
  - 日本語Wiki。やりたいことをここで検索すれば大体出てくる説があります。

# おわり

スラッシュコマンドに対応したDiscord Botを作りました。後はいろいろ工夫して頑張ってください(雑)。