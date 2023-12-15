# ページ追加方法

## Material for MkDocs

本ドキュメントは Material For MkDocs を使用しています。  
ここに書かれていないような詳しい使い方はこちらを参考にすると良いです。

[公式ページ](https://squidfunk.github.io/mkdocs-material/)

## ディレクトリ構造

`docs`ディレクトリは markdown ファイルを保管する場所です。  
ディレクトリ内には、`common` (共通)、`mechanical` (機械班)、`circuit` (回路班)、`control` (制御班)、`education` (教育資料)があります。

`mkdocs.yml`は追加した markdown ファイルを登録する設定ファイルです。

```
docs
|- common (共通)
|- mechanical (機械班)
|- circuit (回路班)
|- control (制御班)
|- education (教育資料)
mkdocs.yml
```

## ブランチ規則

`main`ブランチから作業用のブランチを切って作業を行います。

### Style

```
<type>/<alias>
```

### Type

`main`: プロダクション用ブランチ  
`feature`: 開発用ブランチ

### 例

Power Drive のドキュメントを作成する場合のブランチ名

```
feature/ad-power-drive-document
```

## コミット規則

### Template

```
<type>: <subject>
```

### Type

- **feat**: 新機能
- **change**: 修正・削除

### Subject

コミットの説明文になります。原則日本語を使ってください。

### 例

Power Drive のドキュメントを追加した場合

```
feat: Power Driveのドキュメントを追加
```

## ページ追加

1. (任意) `mkdocs-material`をインストールして、`mkdocs serve`コマンドを打つ。
2. 追加したいページのジャンルのディレクトリに移動します。
   例）機械班なら`mechanical`
3. 追加するトピックの名前でディレクトリを作成します。
   例）「Fusion の使い方」なら`how-to-use-fusion`
4. 作成したディレクトリに`index.md`を作成する。
5. `mkdocs.yml`に追加する。

```yml title="mkdocs.yml"
# ...

nav:
  - ホーム:
      - index.md
  - 共通:
      - common/index.md
      - ページ追加方法: common/create-new-document/index.md
  - 機械:
      - mechanical/index.md
      # 例の場合だとここに追加!
      # タイトル: ファイルの場所 の形式で書く
      - Fusionの使い方: mechanical/how-to-use-fusion/index.md
  - 回路:
      - circuit/index.md
  - 制御:
      - control/index.md
  - 教育:
      - education/index.md
# ...
```

6. Markdown 形式でページ内容を書く

```markdown title="(例) mechanical/how-to-use-fusion/index.md"
# Fusion の使い方

機械班では Fusion を使ってます。使い方を紹介。

## 新規ファイルを作成

...
```

7. コミット  
   変更・追加したファイルをコミットしてください。
