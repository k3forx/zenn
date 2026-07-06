---
name: zenn-cli
description: zenn-cli（このリポジトリのZenn記事管理CLI）を操作するスキル。記事や本の新規作成（new:article / new:book）、ローカルプレビュー（preview）、一覧表示（list:articles / list:books）の正しいコマンド・オプションと、実行前のバージョン確認・更新手順をまとめた統合リファレンス。ユーザーが「新しい記事作って」「zennで記事/本を追加」「プレビュー見せて」「ローカルで記事を確認」「記事一覧」「zenn cli」などと言ったときに必ず起動すること。`/zenn-cli` で明示的に呼ばれた場合も同じ。記事の文章レビューは review-zenn-article、体裁の一括修正は format が担当なので、そちらの話題ではこのスキルを使わない。
argument-hint: <サブコマンド> (例: new:article / preview / list:articles)
version: 1.0.0
author: kanata-miyahana
type: user-invocable
---

# zenn-cli 操作スキル

このリポジトリの Zenn 記事を `zenn-cli` で操作するための統合リファレンスです。全コマンドは `npx zenn <command>` で実行します（グローバルインストールは前提にしない）。

## 最初に必ず: バージョンチェック

**どのサブコマンドを実行する場合でも、まず zenn-cli が最新かどうかを確認し、古ければ更新してから本題に入ってください。** 記事の frontmatter 仕様や CLI の挙動は zenn-cli のバージョンで変わるため、古いまま操作すると Zenn 側と食い違うことがあります。

```bash
# インストール済みバージョン
npm ls zenn-cli
# 最新バージョン
npm view zenn-cli version
```

- **2つが一致** → 何もせず、そのままサブコマンドへ進む
- **インストール済みが古い** → 次で最新に更新し、更新した事実（旧→新のバージョン）をユーザーに一言報告してから進む

```bash
npm install zenn-cli@latest
```

`npm install zenn-cli@latest` は `package.json` の `zenn-cli` のバージョン範囲も書き換えます。この差分はコミットに含めてよい変更なので、更新したことを報告に残してください。勝手にコミットはしません（ユーザーがコミットを求めたときのみ）。

## new:article（記事を追加）

```bash
npx zenn new:article --title "タイトル" --type tech --emoji 😸 --machine-readable
```

| オプション | 説明 |
|---|---|
| `--title` | 記事タイトル |
| `--type` | `tech`（技術記事）/ `idea`（アイデア記事） |
| `--emoji` | アイキャッチ絵文字（1文字） |
| `--published` | 付けると公開。デフォルトは未公開（下書き） |
| `--publication-name` | Zenn Publication に紐付ける場合のみ |
| `--machine-readable` | 生成されたファイル名だけを標準出力する |

- **`--slug` は指定しない。** slug（記事ID＝公開URL）は zenn-cli に自動生成させる規約です（CLAUDE.md）。生成後の slug は改名しない（公開URLが変わるため）。
- 生成直後のファイルは `published: false`。公開はレビュー後にユーザーが決めるので、勝手に `--published` を付けない。
- `--machine-readable` を付けると作成された `articles/<slug>.md` のパスが取れるので、続けて編集する場合はその出力を使う。
- `topics` は CLI から設定できず、生成後の frontmatter で編集する。表記（大文字小文字）はユーザーの入力のまま尊重し、`Go`→`go` のような小文字化はしない。

## new:book（本を追加）

```bash
npx zenn new:book --title "本のタイトル"
```

| オプション | 説明 |
|---|---|
| `--title` | 本のタイトル |
| `--published` | `true` / `false`（デフォルト `false`） |
| `--summary` | 紹介文（有料でも公開される） |
| `--price` | 価格。有料は 200〜5000、無料は 0（デフォルト） |

記事同様、`--slug` は指定せず自動生成に任せる。

## preview（ローカルプレビュー）

`npx zenn preview` はサーバーを立ち上げっぱなしにするため、**必ずバックグラウンドで起動**する（`Bash` の `run_in_background: true`）。フォアグラウンドで実行すると応答が返らずセッションが固まります。

```bash
npx zenn preview
```

- デフォルトは `http://localhost:8000`。起動後、この URL をユーザーに伝える。ポートを変えたいときは `--port 3000`（`-p`）。
- ホットリロード有効なので、記事を編集すればブラウザ側に反映される。切りたいときは `--no-watch`。
- レンダリング結果を自分で確認・スクショしたい場合は、起動後に claude-in-chrome 系ツールで `http://localhost:8000` を開く。
- 確認が終わったら、起動したバックグラウンドプロセスを止める。

## list:articles / list:books（一覧表示）

```bash
npx zenn list:articles
npx zenn list:articles --format tsv   # または json
```

`--format tsv` / `--format json` で機械可読な出力になる。件数を数えたり slug 一覧を取ったりするときはこちらを使う。`list:books` も同じ。

## トラブルシューティング

- **`zenn: command not found`** → グローバル未インストール。必ず `npx zenn ...` で実行する。
- **preview を実行したら応答が返らない** → フォアグラウンド実行が原因。バックグラウンドで起動し直す。
- **frontmatter や挙動が Zenn の公式ドキュメントと違う** → まず「バージョンチェック」に戻り、`npm install zenn-cli@latest` で更新する。
- 公式ガイド: https://zenn.dev/zenn/articles/zenn-cli-guide
