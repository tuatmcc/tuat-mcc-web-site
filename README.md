

# 東京農工大学 MCC のホームページ

農工大 マイクロコンピューターサークルの WEB サイトです。
https://www.tuatmcc.com/

![screenshot](https://user-images.githubusercontent.com/84656786/221634518-e646b4d9-bb18-497e-9d91-bcf4fc575747.png)


## 方針

古典的な WEB サイトを、現代のフロントエンド・フレームワークを駆使して書き直すコンセプトです。
古臭そうな見た目をしてる割に、がっつりレスポンシブ・SPA で動きます。

## 開発環境の起動

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

```

## リポジトリ運用

とりあえずリポジトリをcloneしてください．

### 記事を更新したい

[content ディレクトリ](https://github.com/tuatmcc/tuat-mcc-web-site/tree/develop/content)内にマークダウンの記事が入っているので、編集します。

1. `master`ブランチから`docs/foobar`ブランチを切る
2. マークダウンを追加・編集
3. `docs/foobar` -> `master` のプルリクエストを立てて、本番環境に反映する

画像は[static ディレクトリ](https://github.com/tuatmcc/tuat-mcc-web-site/tree/develop/static)に入れて`<img>`タグで追加します． `static/works/hello.png`を表示する場合のタグは次のとおりです．
```html
<img src="/works/hello.png">
```

### Webサイト側の保守をしたい

1. `develop`ブランチから`feature/foobar`ブランチを切る
2. いろいろ編集
3. `feature/foobar` -> `develop`のプルリクエストを立ててマージ (この時点では本番環境にデプロイされません)
4. (本番環境へのデプロイをしたい場合) `develop` -> `master`のプルリクエストを立てて，本番環境に反映する
