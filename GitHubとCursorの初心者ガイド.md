# GitHub と Cursor の初心者向けガイド

プログラミングを始めたばかりの方に向けて、**GitHub**（コードの保管・共有サービス）と **Cursor**（AI 付きコードエディタ）の基本的な使い方をまとめました。

---

## 目次

1. [Cursor とは](#cursor-とは)
2. [Cursor の基本操作](#cursor-の基本操作)
3. [GitHub とは](#github-とは)
4. [GitHub の基本操作](#github-の基本操作)
5. [Cursor と GitHub を一緒に使う](#cursor-と-github-を一緒に使う)
6. [よくあるトラブルと対処法](#よくあるトラブルと対処法)

---

## Cursor とは

**Cursor** は、AI（人工知能）が組み込まれたコードエディタです。  
Visual Studio Code をベースにしており、コードの補完・説明・修正・質問への回答を AI に任せられます。

### Cursor でできること

- **コードの自動補完**：入力中に次のコードを提案してくれる
- **チャットで質問**：コードの意味や書き方を AI に聞ける
- **コードの編集依頼**：自然な言葉で「この関数を追加して」などと指示できる
- **エラーの説明と修正案**：エラーが出たときに原因と直し方を教えてくれる

### Cursor のインストール

1. [Cursor 公式サイト](https://cursor.com/) にアクセス
2. **Download** から OS 用のインストーラーをダウンロード
3. インストール後、Cursor を起動してアカウント作成（無料で利用可能）

---

## Cursor の基本操作

### プロジェクトを開く

- **ファイル → Open Folder** でフォルダを選択すると、そのフォルダが「プロジェクト」として開きます
- または、ターミナルで `cursor .` と打つと、今いるフォルダが Cursor で開きます

### よく使うショートカット（Mac）

| 操作             | ショートカット      |
|------------------|---------------------|
| AI チャットを開く | `Cmd + L`           |
| コマンドパレット   | `Cmd + Shift + P`   |
| ファイル検索       | `Cmd + P`           |
| 保存             | `Cmd + S`           |

### AI の使い方

1. **チャット（Cmd + L）**  
   - 画面横にチャットが開く  
   - 「このコードの説明をして」「〇〇という機能を追加して」などと入力  
   - 必要なら `@ファイル名` で特定のファイルを指定できる  

2. **インライン編集**  
   - コードを選択して **Cmd + K** を押す  
   - 指示を書くと、その部分だけ AI が書き換えてくれる  

3. **ターミナル**  
   - **Ctrl + `**（バッククォート）でターミナルを表示  
   - ここで `git` や `npm` などのコマンドを実行する  

---

## GitHub とは

**GitHub** は、プログラムのソースコードを保存・共有するための Web サービスです。  
「Git」というバージョン管理の仕組みの上に成り立っています。

### GitHub でできること

- **コードのバックアップ**：パソコンが壊れてもコードを残せる
- **変更履歴の管理**：いつ、何を変えたかが分かる
- **共同開発**：他の人と同じプロジェクトで作業できる
- **公開・非公開**：リポジトリを公開したり、自分だけにしたりできる

### 用語の整理

| 用語       | 意味                                                         |
|------------|--------------------------------------------------------------|
| リポジトリ | プロジェクトのコード一式が入っている「入れ物」                 |
| コミット   | 「この時点の変更を保存する」という単位                        |
| プッシュ   | 自分のパソコンから GitHub へ変更を送ること                    |
| プル       | GitHub から自分のパソコンへ変更を取ってくること              |
| ブランチ   | 本流（main）とは別の「作業用の枝」。機能ごとに分けて作業する |

### GitHub アカウントの作成

1. [GitHub](https://github.com/) にアクセス
2. **Sign up** からメールアドレスでアカウント作成
3. 無料プランで十分に使えます

---

## GitHub の基本操作

### 1. Git のインストール（まだの場合）

- **Mac**：ターミナルで `git --version` を実行。入っていなければ Xcode のコマンドラインツールを入れるか、[Git 公式](https://git-scm.com/) からインストール
- **Windows**：[Git for Windows](https://git-scm.com/download/win) をダウンロードしてインストール

### 2. GitHub にログインする（初回だけ）

Cursor のターミナル、または通常のターミナルで：

```bash
git config --global user.name "あなたの名前"
git config --global user.email "GitHubに登録したメール"
```

GitHub のサイトで **Settings → SSH and GPG keys** から SSH キーを登録すると、パスワードなしでプッシュできます（設定は任意）。

### 3. 新しいリポジトリを作る（GitHub 上で）

1. GitHub にログイン
2. 右上の **+** → **New repository**
3. リポジトリ名を入力（例：`my-project`）
4. **Create repository** をクリック

### 4. 自分のパソコンで Git を始める

プロジェクトのフォルダで、Cursor のターミナルを開き：

```bash
# このフォルダを Git の管理下に置く
git init

# 変更を「ステージング」する（保存したいファイルを選ぶ）
git add .

# 初回のコミット
git commit -m "最初のコミット"
```

### 5. GitHub とつなぐ

GitHub で作ったリポジトリのページに、「…or push an existing repository from the command line」という欄があります。そこで表示されるコマンドをそのまま使います（リポジトリ名はあなたのものに変わります）：

```bash
git remote add origin https://github.com/あなたのユーザー名/リポジトリ名.git
git branch -M main
git push -u origin main
```

これで、ローカルのコードが GitHub にアップロードされます。

### 6. 日常の流れ（変更を保存して GitHub に送る）

```bash
# 変更したファイルを全部ステージング
git add .

# コミット（メッセージは何をしたか分かるように書く）
git commit -m "〇〇の機能を追加"

# GitHub に送る
git push
```

---

## Cursor と GitHub を一緒に使う

### 基本的な流れ

1. **Cursor** でコードを書く・AI に修正してもらう
2. **Cursor のターミナル**（または外のターミナル）で `git add .` → `git commit -m "メッセージ"` → `git push`
3. **GitHub** のサイトで、コードが反映されているか確認

### Cursor から GitHub に接続する

1. Cursor 左の **Source Control**（枝分かれマーク）をクリック
2. **Publish to GitHub** や **Publish Branch** が出ていれば、そこから GitHub にリポジトリを作成・プッシュできる
3. すでに `git init` と `remote` を設定している場合は、**Source Control** で変更を確認し、✓ でコミットしてから、上の「…」メニューやプッシュボタンで `git push` できる

### 他人のリポジトリを自分のパソコンに持ってくる

```bash
# クローン（ダウンロード）
git clone https://github.com/ユーザー名/リポジトリ名.git
```

クローンしたフォルダを Cursor の **Open Folder** で開けば、そのまま編集できます。

---

## よくあるトラブルと対処法

### 「Permission denied」や「Authentication failed」が出る

- GitHub にログインする方法が古くなっていることがあります
- **GitHub → Settings → Developer settings → Personal access tokens** でトークンを作り、パスワードの代わりにそのトークンを使う
- または SSH キーを登録して `git@github.com:...` 形式の URL を使う

### 「Updates were rejected」と出て push できない

- 先に GitHub 側で変更が入っている場合がある
- 次のように **pull** してから **push** する：

```bash
git pull origin main
# 競合がなければ
git push origin main
```

### コミットメッセージを間違えた

- 直前にしたコミットだけ直す場合：

```bash
git commit --amend -m "正しいメッセージ"
```

### どのファイルが変更されているか分からない

- Cursor の **Source Control** を見る
- またはターミナルで `git status` を実行

---

## まとめ

- **Cursor**：コードを書き、AI に聞きながら編集する場所
- **Git**：変更を「コミット」という単位で記録する仕組み
- **GitHub**：そのコミットをオンラインで保存・共有する場所

まずは「Cursor で少しコードを書く → ターミナルで `git add .` → `git commit -m "メッセージ"` → `git push`」の流れに慣れると、自然と GitHub の使い方も身につきます。  
分からないことがあれば、Cursor の AI チャットで「このエラーの意味は？」「この git のコマンドは何をしている？」と聞いてみてください。
