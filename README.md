# Document

Mechatech 部の全てをまとめたドキュメントです。

## ページ追加方法

[ページ追加方法](docs/common/create-new-document/index.md)

## Material MkDocs 公式リファレンス

Material MkDocs を使用することで見やすいドキュメントを作成することができます。

https://squidfunk.github.io/mkdocs-material/reference/

## ローカル環境

1. Material MkDocs のインストール

```
pip install mkdocs-material
```

2. ローカルサーバーの起動

```
mkdocs serve
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

## コミット規則

### Template

```
<type>: <subject>
```

### Type

- **feat**: 新機能
- **change**: 修正・削除
- **fix**: バグフィックス
- **docs**: ドキュメントに関する変更
- **style**: フォーマット等の変更
- **refactor**: リファクタに関する変更
- **debug**: デバック用のコード
- **test**: テストコードの追加・更新
- **chore**: GitHub Actions 等タスクに関する変更
