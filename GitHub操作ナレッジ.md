# GitHub 操作ナレッジ — コマンドとトラブル対処の詳細

このドキュメントは、**ローカルフォルダを GitHub リポジトリに繋いで push するまで**の一連の操作を、コマンド単位でまとめたナレッジです。  
初心者の方が「何をしているか」を把握しやすいよう、各コマンドの意味と、発生しうるエラー・対処法を記載しています。

---

## 目次

1. [事前準備](#1-事前準備)
2. [基本の流れ：ローカルを GitHub に繋ぐ](#2-基本の流れローカルを-github-に繋ぐ)
3. [認証：HTTPS と Personal Access Token（PAT）](#3-認証https-と-personal-access-tokenpat)
4. [発生したエラーと対処法（実例）](#4-発生したエラーと対処法実例)
5. [コマンド早見表](#5-コマンド早見表)

---

## 1. 事前準備

### 1.1 必要なもの

| 項目 | 内容 |
|------|------|
| GitHub アカウント | [github.com](https://github.com) で作成 |
| Git | 未導入なら [git-scm.com](https://git-scm.com) からインストール |
| 作業フォルダ | 例：`/Users/あなたのユーザー名/Documents/private` |

### 1.2 Git の動作確認

```bash
git --version
```

- **意味**：インストールされている Git のバージョンを表示する。
- **期待する結果**：`git version 2.x.x` のように表示されれば OK。

### 1.3 Git のユーザー設定（初回のみ）

```bash
git config --global user.name "あなたの名前"
git config --global user.email "GitHubに登録したメールアドレス"
```

- **意味**：このパソコンで Git が行うコミットに「誰が」行ったかを記録するための名前とメールを設定する。
- **補足**：`--global` は「このパソコン上のすべてのリポジトリで使う」という意味。

---

## 2. 基本の流れ：ローカルを GitHub に繋ぐ

### 2.1 作業フォルダに移動する

```bash
cd /Users/s30845/Documents/private
```

- **意味**：ターミナルの「現在のフォルダ」を、Git で管理したいフォルダに変更する。
- **あなたの環境**：パスは実際のフォルダに合わせて変更する。

### 2.2 このフォルダを Git の管理下にする（初回のみ）

```bash
git init
```

- **意味**：このフォルダの中に `.git` という隠しフォルダを作り、「変更の履歴を Git で管理する」状態にする。
- **注意**：すでに `.git` がある場合は「Reinitialized」と出るだけ。二重に init しても問題ない。

### 2.3 GitHub のリポジトリを「送り先」として登録する

```bash
git remote add origin https://github.com/iharak058070-afk/test.git
```

- **意味**：このローカルリポジトリの「送り先」に、`origin` という名前で GitHub の URL を登録する。
- **origin**：慣例で「メインのリモート」の名前として使う。
- **URL**：`https://github.com/所有者/リポジトリ名.git` の形。所有者・リポジトリ名は自分のものに置き換える。

**「origin は既にあります」と出た場合**

```bash
git remote remove origin
git remote add origin https://github.com/iharak058070-afk/test.git
```

- **意味**：既存の `origin` を削除してから、正しい URL で付け直す。

### 2.4 送りたいファイルを「ステージ」する

```bash
git add .
```

- **意味**：カレントフォルダ（`.`）以下の「変更・新規ファイル」を、次のコミットに含めるために「ステージング」する。
- **補足**：何も変更や新規ファイルがないと、この後のコミットで「nothing to commit」になる。

### 2.5 コミット（保存の単位）を作る

```bash
git commit -m "はじめてのコミット"
```

- **意味**：ステージした内容を、1 つの「コミット」として履歴に記録する。`-m` はコミットメッセージを指定するオプション。
- **補足**：メッセージは後から履歴を見たときに分かりやすいように書く。

### 2.6 ブランチ名を main にする

```bash
git branch -M main
```

- **意味**：現在のブランチ名を `main` に変更する（GitHub のデフォルトブランチ名に合わせる）。
- **補足**：昔は `master` が多かったが、現在は `main` が一般的。

### 2.7 GitHub に送る（push）

```bash
git push -u origin main
```

- **意味**：ローカルの `main` ブランチの内容を、リモートの `origin` の `main` ブランチに送る。
- **`-u`**：次回以降は `git push` だけでも同じ `origin main` に送れるように「追跡」を設定する。

---

## 3. 認証：HTTPS と Personal Access Token（PAT）

### 3.1 push 時に「ユーザー名・パスワード」を聞かれたら

- **Username**：GitHub のユーザー名（例：`UT-ihara`）。
- **Password**：**GitHub のログインパスワードではなく、Personal Access Token（PAT）** を入力する。  
  （HTTPS ではパスワード認証が廃止されているため。）

### 3.2 Personal Access Token（PAT）の作り方

1. GitHub にログインする。
2. 右上のアイコン → **Settings**。
3. 左メニュー一番下の **Developer settings**。
4. **Personal access tokens** → **Tokens (classic)**。
5. **Generate new token (classic)** をクリック。
6. **Note**：用途が分かる名前（例：`Cursor push`）。
7. **Expiration**：有効期限（例：90 days または No expiration）。
8. **repo** にチェックを入れる（リポジトリへのアクセスに必要）。
9. **Generate token** で発行。
10. 表示された **長い文字列をコピー**する（再表示されないため）。
11. パスワード入力欄には、このトークンを貼り付ける。

### 3.3 別アカウントで push しようとして 403 が出た場合

- **エラー例**：`Permission to 所有者/リポジトリ.git denied to 別のユーザー名`
- **原因**：このパソコンに保存されている認証情報が「別のユーザー」のものになっている。
- **対処**：
  - **そのリポジトリに push したいアカウント**で PAT を作り、そのアカウントの認証だけが使われるようにする。
  - macOS なら **キーチェーンアクセス** で「github.com」を検索し、古い認証情報を削除してから、もう一度 `git push` し、正しいユーザー名と PAT を入力する。

### 3.4 他の人に push 権限を付与する（コラボレーター）

- リポジトリの**所有者**が、GitHub のリポジトリページで行う。
1. リポジトリを開く → **Settings**。
2. 左の **Collaborators and teams**（または **Collaborators**）。
3. **Add people** で、付与したい GitHub ユーザー名を入力。
4. 権限を **Write** にし、追加する。
- 追加された側は、**自分の PAT** で `git push` すれば、そのリポジトリに push できる。

---

## 4. 発生したエラーと対処法（実例）

ここからは、実際に発生したエラーと、そのときの対処をコマンド付きでまとめます。

---

### エラー1：nothing to commit（コミットするものが無い）

**表示例**

```
nothing to commit (create/copy files and use "git add" to track)
```

**原因**  
フォルダが空、または `git add .` で追加できる変更・新規ファイルがなかった。そのためコミットが作られず、push する「元」がない。

**対処**

1. 最低 1 つはファイルを作る（例：README.md）。
   ```bash
   echo "# test" > README.md
   ```
2. 改めてステージしてコミットする。
   ```bash
   git add .
   git commit -m "はじめてのコミット"
   git push -u origin main
   ```

---

### エラー2：Permission denied（403）— 別アカウントで push している

**表示例**

```
remote: Permission to iharak058070-afk/test.git denied to UT-ihara.
fatal: unable to access '...': The requested URL returned error: 403
```

**原因**  
リポジトリの所有者は `iharak058070-afk` なのに、認証情報が `UT-ihara` のものになっている。  
→ 所有者が **UT-ihara** をコラボレーターに追加するか、認証を **iharak058070-afk** の PAT に切り替える必要がある。

**対処**

- **A. コラボレーターに追加する（所有者が実施）**  
  Settings → Collaborators and teams → Add people → `UT-ihara` を追加し、権限を Write に。
- **B. 正しいアカウントで認証し直す**  
  キーチェーンで古い GitHub 認証を削除し、`git push` でリポジトリ所有者のユーザー名と PAT を入力する。

---

### エラー3：rejected (fetch first) — リモートに自分に無い変更がある

**表示例**

```
! [rejected] main -> main (fetch first)
hint: Updates were rejected because the remote contains work that you do not have locally.
hint: use 'git pull' before pushing again.
```

**原因**  
GitHub 側（リモート）にはすでにコミットがあり、ローカルにはその内容がない。  
そのまま push するとリモートの履歴が上書きされる恐れがあるため、Git が push を拒否している。

**対処**

まずリモートの変更を取り込んでから push する。

```bash
git pull origin main --no-rebase
git push -u origin main
```

- **`git pull origin main`**：リモートの `main` を取得し、現在のブランチにマージする。
- **`--no-rebase`**：マージで取り込む（rebase は使わない）。

---

### エラー4：refusing to merge unrelated histories

**表示例**

```
fatal: refusing to merge unrelated histories
```

**原因**  
ローカルは「このフォルダで `git init` して作った履歴」、リモートは「GitHub 上で別に作った履歴」で、**共通の親コミットが無い**。  
Git は安全のため、そのままマージしない。

**対処**

「無関係な履歴でもまとめてよい」と明示して pull する。

```bash
git pull origin main --no-rebase --allow-unrelated-histories
```

- **`--allow-unrelated-histories`**：共通の祖先がなくてもマージを許可する。
- その後、問題なければ push する。
  ```bash
  git push -u origin main
  ```

---

### エラー5：Need to specify how to reconcile divergent branches

**表示例**

```
fatal: Need to specify how to reconcile divergent branches.
hint: git config pull.rebase false   # merge
hint: git config pull.rebase true    # rebase
hint: git config pull.ff only        # fast-forward only
```

**原因**  
「pull のときに、分岐したブランチをどうまとめるか」が設定されておらず、Git が判断できない。

**対処**

「マージでまとめる」かつ「無関係な履歴を許可する」を指定して pull する。

```bash
git pull origin main --no-rebase --allow-unrelated-histories
```

その後、同じように push する。

```bash
git push -u origin main
```

---

### エラー6：You have not concluded your merge (MERGE_HEAD exists)

**表示例**

```
error: You have not concluded your merge (MERGE_HEAD exists).
hint: Please, commit your changes before merging.
fatal: Exiting because of unfinished merge.
```

**原因**  
以前の `git pull` でマージが始まったが、**マージを完了するためのコミット**をしていない。  
そのため「マージの途中」のままになっており、次の pull ができない。

**対処**

1. 状態を確認する。
   ```bash
   git status
   ```
2. **「All conflicts fixed but you are still merging」** と出ている場合  
   → コンフリクトは解消済みなので、マージを完了するコミットをするだけ。
   ```bash
   git commit -m "Merge remote main"
   git push -u origin main
   ```
3. **Unmerged paths** がある場合  
   → 該当ファイルの `<<<<<<<` / `=======` / `>>>>>>>` を編集して解消し、ステージしてからコミット。
   ```bash
   git add .
   git commit -m "Merge remote main"
   git push -u origin main
   ```
4. マージ自体をやめたい場合（マージをなかったことにしたい場合のみ）。
   ```bash
   git merge --abort
   ```
   その後、必要なら `git pull origin main --no-rebase --allow-unrelated-histories` からやり直す。

---

### エラー7：push が again rejected (non-fast-forward)

**表示例**

```
! [rejected] main -> main (non-fast-forward)
hint: Updates were rejected because the tip of your current branch is behind its remote counterpart.
```

**原因**  
再度、リモートにローカルに無いコミットがある状態。  
（前回のマージ完了前に push しようとした、または別の環境でリモートが更新された、など。）

**対処**

もう一度リモートの変更を取り込んでから push する。

```bash
git pull origin main --no-rebase
git push -u origin main
```

まだ「unrelated histories」と言われる場合は、以下を使う。

```bash
git pull origin main --no-rebase --allow-unrelated-histories
git push -u origin main
```

---

## 5. コマンド早見表

| 目的 | コマンド |
|------|----------|
| Git のバージョン確認 | `git --version` |
| ユーザー名・メール設定（初回） | `git config --global user.name "名前"` / `user.email "メール"` |
| フォルダに移動 | `cd パス` |
| リポジトリ初期化 | `git init` |
| リモート登録 | `git remote add origin https://github.com/所有者/リポジトリ.git` |
| リモート変更 | `git remote set-url origin URL` または `git remote remove origin` のあと `add` |
| ステージング | `git add .` |
| 状態確認 | `git status` |
| コミット | `git commit -m "メッセージ"` |
| ブランチ名を main に | `git branch -M main` |
| プッシュ | `git push -u origin main` |
| リモートの変更を取り込む（通常） | `git pull origin main --no-rebase` |
| 無関係な履歴もマージ | `git pull origin main --no-rebase --allow-unrelated-histories` |
| マージ完了（コンフリクト解消後） | `git add .` → `git commit -m "Merge remote main"` |
| マージを中止 | `git merge --abort` |

---

## 用語の整理

| 用語 | 意味 |
|------|------|
| リポジトリ | 1 つのプロジェクトの「入れ物」。ローカルとリモート（GitHub）の両方にある。 |
| リモート / origin | インターネット上の保存場所。`origin` はその名前の慣例。 |
| コミット | 変更を 1 つのまとまりとして履歴に記録すること。 |
| ステージング | 次のコミットに含めるファイルを選ぶこと。`git add` で行う。 |
| push | ローカルのコミットをリモートに送ること。 |
| pull | リモートの変更を取得してローカルに取り込むこと。 |
| マージ | 2 つの履歴（ブランチ）を 1 つにまとめること。 |
| コンフリクト | 同じファイルの同じ場所を両方で変更したため、自動でマージできない状態。 |

---

このナレッジは、**GitHub と Cursor の初心者ガイド** で説明している流れを、コマンドとエラー対処に絞って詳細化したものです。  
同じ手順やエラーに当たったときに、このドキュメントを参照すると把握しやすくなります。
