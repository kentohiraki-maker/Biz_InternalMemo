# Git：Pull / Push メモ

このフォルダで作業するときの基本コマンドです。ターミナルを開いてから使います。

## 作業前（リモートの変更を取り込む）

```bash
cd "/Users/hirakikentou/Library/Mobile Documents/com~apple~CloudDocs/Biz_InternalMemo"
git pull
```

- 他の PC で編集した内容や、GitHub 上で直した内容を **取り込む** ときに実行します。
- 競合（コンフリクト）が出たら、表示されたファイルを編集してから `git add` → `git commit` が必要になることがあります。

## 作業後（変更を GitHub に送る）

```bash
cd "/Users/hirakikentou/Library/Mobile Documents/com~apple~CloudDocs/Biz_InternalMemo"
git status
git add .
git commit -m "変更内容の短い説明"
git push
```

- `git status` … 何が変わったか確認（省略可だが最初はおすすめ）
- `git add .` … 変更をまとめてステージ（特定のファイルだけなら `git add ファイル名`）
- `git commit` … ローカルに記録。`-m` の後にメッセージを書きます。
- `git push` … GitHub（`origin`）へ **送る**

## よくある流れ（まとめ）

| やりたいこと | コマンド |
|-------------|---------|
| 最新を取り込む | `git pull` |
| 変更を送る | `git add .` → `git commit -m "..."` → `git push` |

## リモートの確認（必要なときだけ）

```bash
git remote -v
```

`origin` が `https://github.com/kentohiraki-maker/Biz_InternalMemo.git` を指していれば問題ありません。

## ローカルで新しいフォルダやファイルを作ったとき

### 基本的には特別な対応は不要

新規のファイルやフォルダを作っただけでは、Git は自動では追跡しません。GitHub に載せたいときは、いつもどおり次で十分です。

```bash
git add .
git commit -m "説明"
git push
```

`git add .` で、その時点の変更・新規ファイルをまとめてステージできます。

### 押さえておくとよいこと

1. **GitHub に上げたくないもの**  
   パスワード、APIキー、個人用メモなどは **置かない**か、**`.gitignore` にパターンを書いて除外**する（秘密情報用の仕組みを使うのも可）。

2. **空のフォルダだけ**  
   Git は **中身がないフォルダは記録しません**。フォルダ自体もリポジトリに残したいときは、中に `.gitkeep` のような **空でないファイルを1つ**置くのが一般的です。

3. **すでに `.gitignore` で除外している名前**  
   そのパターンに当てはまる新規ファイルは、`git add` の対象にならず **GitHub にも上がりません**（意図どおりなら問題ありません）。

### まとめ

普通に作業し、載せたいものだけ `add` → `commit` → `push` でよいです。上げたくないものだけ `.gitignore` や置き場所で分けます。
