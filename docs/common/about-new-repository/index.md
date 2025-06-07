


# ROS2 C++ プロジェクト チーム開発運用マニュアル

---

## 目次

1. [リポジトリ作成・初期化](#リポジトリ作成初期化)
2. [Raspberry Pi 側でのセットアップ](#raspberry-pi-側でのセットアップ)
3. [SSH鍵の生成とGitHub登録](#ssh鍵の生成とgithub登録)
4. [リモートリポジトリのクローン](#リモートリポジトリのクローン)
5. [リポジトリ初期コミット](#リポジトリ初期コミット)
6. [.gitignoreの設定例](#gitignoreの設定例)
7. [コミット・pull・pushの基本手順](#コミットpullpushの基本手順)
8. [ブランチ運用とPull Request](#ブランチ運用とpull-request)
9. [トラブルシューティング](#トラブルシューティング)
10. [運用ルールまとめ](#運用ルールまとめ)

---

## リポジトリ作成・初期化

### 1.1 GitHubでリポジトリを新規作成

1. [GitHub](https://github.com/)にログインし、「New repository」をクリック。
2. Repository name を入力（例: `robot-control`）
3. 「Initialize this repository with a README」は**チェックを外す**（空リポジトリ推奨）
4. Public / Private を選択
5. 作成後、リポジトリのSSHまたはHTTPS URLを控える。

---

## Raspberry Pi 側でのセットアップ

### 2.1 Gitのインストール（初回のみ）

```bash
sudo apt update
sudo apt install git
````

### 2.2 ユーザー情報の設定（初回のみ）

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

---

## SSH鍵の生成とGitHub登録

### 3.1 新しいSSH鍵を作成

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
# そのままEnter連打でOK（パスフレーズは任意）
```

### 3.2 公開鍵の確認とGitHub登録

```bash
cat ~/.ssh/id_ed25519.pub
# 出力された1行全部をコピー
```

* GitHub → 右上アイコン → Settings → SSH and GPG keys → New SSH key
* Titleは自由、Key欄にコピーした公開鍵を貼り付け、「Add SSH key」

---

## リモートリポジトリのクローン

### 4.1 プロジェクトの保存先ディレクトリに移動

```bash
cd ~/your_workspace/
```

### 4.2 クローン

```bash
git clone git@github.com:YourUserName/robot-control.git
cd robot-control
```

---

## リポジトリ初期コミット

### 5.1 プロジェクトファイルの配置

（例：`include`, `src`, `CMakeLists.txt`などをこのディレクトリ直下に配置）

### 5.2 .gitignoreの作成

```bash
nano .gitignore
# 必要な内容を記載（例は下記）
```

### 5.3 最初のコミットとpush

```bash
git add .
git commit -m "初回コミット: プロジェクト初期構成"
git branch -M main
git push -u origin main
```

---

## .gitignoreの設定例

```gitignore
/build/
/install/
/log/
*.swp
*.swo
*.pyc
.vscode/
.DS_Store
```

---

## コミット・pull・pushの基本手順

### 7.1 作業前のpull（他人の更新を取得）

```bash
git pull origin main
```

### 7.2 ファイルの編集

（VSCodeでリモート接続し編集推奨）

### 7.3 変更の確認

```bash
git status
```

### 7.4 変更の追加

```bash
git add .
# または
git add ファイル名
```

### 7.5 コミット

```bash
git commit -m "変更内容を簡潔に記述"
```

### 7.6 push

```bash
git push origin ブランチ名
# 例：git push origin main
```

---

## ブランチ運用とPull Request

### 8.1 新規ブランチの作成

```bash
git checkout -b feature/xxxx
```

### 8.2 変更内容のpush

```bash
git push origin feature/xxxx
```

### 8.3 Pull Requestの作成

1. GitHubでリポジトリページを開く
2. `feature/xxxx`ブランチで「Compare & pull request」をクリック
3. レビューを受け、承認されたらmainにマージ

### 8.4 mainブランチの更新をpull

```bash
git checkout main
git pull origin main
```

---

## トラブルシューティング

### SSHの「Permission denied (publickey)」エラーが出た場合

* 鍵を作成し直して、\*\*ユーザーの「SSH and GPG keys」\*\*に再登録すること
* Deploy key（読み取り専用）に登録しないこと

### 「project」ディレクトリごとpushしてしまった場合

* 必要ならGitHubのリポジトリを一度削除し、ディレクトリ直下に`include`や`src`がある形で再pushする

### SSH初回接続時の「authenticity of host」メッセージ

* フィンガープリントがGitHub公式と一致していれば`yes`でOK

---

## 運用ルールまとめ

* **コミット・push/pullは必ずRaspberry Pi上で統一して実行**
* **mainブランチに直接push禁止、必ずfeatureブランチ→Pull Requestで運用**
* **作業前は必ずpullして最新化**
* **コンフリクトが出た場合は落ち着いて`git status`/`git diff`で内容を確認し、修正すること**
* **.gitignoreを守り、ビルド生成物や不要ファイルはリポジトリに含めない**
* **README.md等にセットアップ手順、ルール、トラブル事例を随時追記する**

---

## よく使うGitコマンドまとめ

| コマンド                        | 意味                      |
| --------------------------- | ----------------------- |
| git pull origin main        | リモートmainを取得して手元のmainに反映 |
| git checkout -b feature/xxx | 新しいブランチを作ってそのまま切り替え     |
| git add .                   | 変更した全てのファイルを追加          |
| git commit -m "コメント"        | 追加分をコミット（コメントは必ずつける）    |
| git push origin feature/xxx | 今のブランチをリモートへアップロード      |
| git checkout main           | mainブランチに戻る             |
| git branch                  | 今存在するブランチ一覧を表示          |
| git status                  | 今の変更状況を確認               |

