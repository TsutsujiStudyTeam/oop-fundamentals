# oop-fundamentals
[本には書いてないオブジェクト指向](http://www.ytp.ne.jp/wiki/Wiki.jsp?page=%E6%9C%AC%E3%81%AB%E3%81%AF%E6%9B%B8%E3%81%84%E3%81%A6%E3%81%AA%E3%81%84%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E6%8C%87%E5%90%91) を、[book-to-skill](https://github.com/virgiliojr94/book-to-skill) スキルを使用しスキル化したものです。

**今後の展望**: 
- book-to-skillの仕様により英語で作成されてしまうため、ゆくゆく日本語化したい
- ゆくゆく作成するであろうChampフレームワークのプラグインなどに付帯スキルとして組み込むなどの利用ができるとよい。
- プラグイン化する場合
  - **privateメソッド禁止** などは、ルールファイル化したい
  - ユーザーの入力要件や要件定義書からデータ構造を抽出するスキル（or サブエージェント）を定義し、クラス図または相応のUMLでアウトプットさせる -> それを入力に本スキルでの実装という流れにした方がよりクオリティがあがりそう

## ディレクトリ構成

```
oop-fundamentals
│
├─chapters           # SKILL.mdの補助ファイル群。本には書いてないオブジェクト指向原本の要約などが格納されるフォルダ
├─glossary.md        # SKILL.mdの補助ファイル。重要用語をまとめている。
├─patterns.md        # SKILL.mdの補助ファイル。設計パターンや手法をまとめている。
├─cheatsheet.md      # SKILL.mdの補助ファイル。設計時に満たすべきポイントのチェックリストをまとめている。
└─SKILL.md           # スキル本体
```

## [備忘] このスキルを作成した手順
1. Chrome拡張「[HTMLからMarkdownへ](https://chromewebstore.google.com/detail/html-to-markdown/kofbbilhmnkcmibjbioafflgmpkbnmme?hl=ja)」をインストール
2. [本には書いてないオブジェクト指向](http://www.ytp.ne.jp/wiki/Wiki.jsp?page=%E6%9C%AC%E3%81%AB%E3%81%AF%E6%9B%B8%E3%81%84%E3%81%A6%E3%81%AA%E3%81%84%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E6%8C%87%E5%90%91) ページを、1.でインストールしたChrome拡張を使用しMarkdown形式で保存
3. [book-to-skill](https://github.com/virgiliojr94/book-to-skill) を使用し、2.で保存したMarkdownを読み込ませてスキルを生成

