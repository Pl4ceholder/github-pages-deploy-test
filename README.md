# GitHub Pages Deploy Test（main / dev）

GitHub Actionsで `main` と `dev` の2ブランチを、1つのGitHub Pagesサイト内の別URLに公開するサンプルです。共同開発者向けの運用手順もこのREADMEにまとめています。

## 公開URL

リポジトリ: https://github.com/Pl4ceholder/github-pages-deploy-test

- **main（本番用）**: https://pl4ceholder.github.io/github-pages-deploy-test/
- **dev（開発・動作確認用）**: https://pl4ceholder.github.io/github-pages-deploy-test/dev/

ページ上部に `PRODUCTION`（main）または `DEVELOPMENT`（dev）が表示されます。

## main と dev の役割

| ブランチ | 用途 | 公開URL | 更新タイミング |
|---|---|---|---|
| `main` | 本番として見せる内容 | サイト直下 `/` | `main` へPushしたとき |
| `dev` | 開発中の内容・動作確認 | `/dev/` | `dev` へPushしたとき |

この構成では、どちらか一方のブランチだけを公開するのではありません。Actionsが毎回 `main` と `dev` の両方を取得し、1つの公開データにまとめます。

- `main` のファイルは公開サイトのルートに配置されます。
- `dev` のファイルは公開サイトの `dev/` ディレクトリに配置されます。
- `dev` にPushしても `main` のファイルは `main` ブランチの内容のままです。逆も同様です。
- ただし、デプロイ処理は両ブランチをまとめて再公開します。
- 2つのURLはどちらもインターネットから閲覧可能です。`dev` は非公開環境ではありません。

## 共同開発の基本ルール

1. 普段の開発・確認作業は `dev` ブランチで行います。
2. `dev` に変更をPushすると、自動で開発用URLへ反映されます。
3. 開発用URLで表示や動作を確認します。
4. 問題がなければPull Requestを作成し、`dev` の変更を `main` に取り込みます。
5. `main` に取り込まれると、本番用URLにも自動で反映されます。

**通常の作業で `main` に直接コミットしない**運用を推奨します。Pull Requestを使うことで、他の共同開発者が変更内容を確認してから本番へ反映できます。

## ブランチの切り替え方

### 初回セットアップ

```bash
# リポジトリを取得
git clone https://github.com/Pl4ceholder/github-pages-deploy-test.git
cd github-pages-deploy-test

# devブランチへ切り替え
git switch dev
```

### 既にリポジトリを取得済みの場合

```bash
# リモートの最新ブランチ情報を取得
git fetch origin

# 開発作業をするとき
git switch dev
git pull origin dev

# 本番ブランチの内容を確認するとき
git switch main
git pull origin main
```

### devで作業してPushする

```bash
git switch dev
# ファイルを編集
git add .
git commit -m "変更内容の説明"
git push origin dev
```

Push後、Actionsの `Deploy main and dev to GitHub Pages` が実行されます。成功したら開発用URLを確認してください。

### devの変更をmainへ反映する

GitHub上でPull Requestを作成します。

- **base（取り込み先）**: `main`
- **compare（変更元）**: `dev`

差分を確認してPull Requestをマージします。`main` へのPushが発生し、本番用URLが更新されます。

## .env の扱い

- `.env.example` は設定項目のサンプルです。必要に応じてコピーしてローカル用の `.env` を作成してください。
- `.env` と `.env.*` はGit管理対象外です（`.env.example` を除く）。
- GitHub Actionsはビルド時に `APP_ENV=main` / `APP_ENV=dev` の `.env` ファイルを一時生成し、ページ表示用の `env-config.js` に反映します。
- `.env` 自体は公開サイトに配置されません。
- **パスワード、APIキー、OAuth Client Secretなどの秘密情報は `.env.example` やGit管理対象ファイルに書かないでください。** GitHub Actionsで秘密情報が必要になった場合は、Repository Settings → Secrets and variables → Actions に登録します。

このサンプルは静的HTMLなので、ブラウザが `.env` を直接読む仕組みではありません。`.env` はビルド時の設定として扱います。

## GitHub側の初期設定

1. Repository → **Settings → Pages → Build and deployment → Source** を `GitHub Actions` にします。
2. Repository → **Settings → Environments → github-pages → Deployment branches and tags** で `main` と `dev` の両方を許可します（テスト用途なら `All branches` でも構いません）。
3. `main` と `dev` の両ブランチが存在することを確認します。
4. Actionsタブで `Deploy main and dev to GitHub Pages` の実行が成功することを確認します。

## 手動デプロイ

Actionsタブ → `Deploy main and dev to GitHub Pages` → `Run workflow` から実行できます。手動実行時のメッセージはページの「デプロイメッセージ」に表示されます。

## 注意事項

- この方式はGitHub Pagesの公開先を2つ作るものではなく、1つのサイトの中に `/` と `/dev/` を作る方式です。
- `dev` の内容も公開されます。開発中であっても、公開して問題のない内容だけをPushしてください。
- `dev` ブランチが存在しない場合、WorkflowのCheckoutが失敗します。
- GitHub PagesのSourceを `GitHub Actions` に設定してください。`Deploy from a branch` のままだと、このWorkflowによる公開と競合します。
- リポジトリにWrite以上の権限を持つ共同開発者がPushできます。閲覧権限だけのユーザーはデプロイできません。
