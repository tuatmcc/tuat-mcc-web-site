# 東京農工大学 MCC のホームページ

農工大 マイクロコンピューターサークルの WEB サイトです。
https://www.tuatmcc.com/

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

## 記事更新方法

[content ディレクトリ](https://github.com/tuatmcc/tuat-mcc-web-site/tree/develop/content)内にマークダウンの記事が入っているので、編集します。

1. 本リポジトリを clone
1. ブランチを切る。めんどくさかったら develop ブランチで良い
1. マークダウンを追加・編集
1. develop->master のプルリクエストを立てて、本番環境に反映する
