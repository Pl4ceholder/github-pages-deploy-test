# GitHub Pages Deploy Test（main / dev）

GitHub Actionsで `main` と `dev` の2環境を1つのGitHub Pagesサイトに公開するサンプルです。

## 公開URL

* `main`（本番用）: https://pl4ceholder.github.io/github-pages-deploy-test/
* `dev`（開発確認用）: https://pl4ceholder.github.io/github-pages-deploy-test/dev/

## セットアップ

1. GitHubで新しいリポジトリを作成します。
2. このフォルダの内容を `main` ブランチへPushします。
3. GitHubの **Settings → Pages → Build and deployment → Source** を `GitHub Actions` に設定します。
4. `dev` ブランチを作成してPushします（例: `git switch -c dev \&\& git push -u origin dev`）。
5. Actionsタブで `Deploy main and dev to GitHub Pages` が成功することを確認します。
6. Settings → Pages に表示されたサイトURLの末尾に `/dev/` を付けると開発環境を確認できます。

## 自動デプロイの動き

* `main` へのPushでWorkflowが実行され、`main` と `dev` の最新内容をまとめて公開します。
* `dev` へのPushでも同じWorkflowが実行され、両方の最新内容をまとめて公開します。
* `main` のファイルはサイト直下、`dev` のファイルは `/dev/` 以下に配置されます。
* 各環境の `deploy-info.json` はActionsが生成し、ブランチ名・日時・実行者・コミットなどを表示します。
* Actionsタブから手動実行もできます。

## 注意点

* これは2つのブランチを1つのPagesサイト内の別パスに配置する方式です。GitHub Pagesのデプロイ先自体は1つです。
* `dev` ブランチが存在しないとCheckoutで失敗します。最初に `dev` ブランチを作成してください。
* 両方のブランチの内容が同じサイトに含まれるため、`dev` の内容もインターネット上に公開されます。
* リポジトリにWrite以上の権限があるメンバーがPushできます。一般公開リポジトリの閲覧者が自由にデプロイできるわけではありません。
* 公開リポジトリ・Pagesには秘密情報や個人情報を置かないでください。

