# write-zenn-article スキル

Zenn テックブログ記事の下書きを生成するスキルです。実行時にお気に入り上位5記事を読み、その構成・文体を真似た下書きを `zenn-cli` で作成した記事ファイルに書き込みます。

手順・ルールは `SKILL.md` にだけ書いています（このファイルに重複して書くとずれるため）。

## 使用方法

```bash
# トピックを指定
/write-zenn-article Go 1.24の新機能

# 引数なし（トピックを対話で聞かれる）
/write-zenn-article
```

## 出力

- `articles/<slug>.md`（slug は zenn-cli が自動生成）
- `published: false` の下書き。公開はレビュー後に決める
