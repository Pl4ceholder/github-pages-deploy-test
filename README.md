# GitHub Pages Deploy Test

GitHub Actionsで静的サイトをGitHub Pagesへ自動デプロイするための最小サンプルです。

## セットアップ

1. GitHubで新しいリポジトリを作成します（例: `github-pages-deploy-test`）。
2. このフォルダ内のファイルをリポジトリの `main` ブランチへPushします。
3. GitHubのリポジトリで **Settings → Pages → Build and deployment → Source** を `GitHub Actions` に設定します。
4. **Actions** タブで `Deploy to GitHub Pages` が成功することを確認します。
5. **Settings → Pages** に表示されるURLを開きます。

## 自動デプロイ

- `main` ブランチへのPushで自動実行されます。
- Actionsタブから `Deploy to GitHub Pages` → `Run workflow` で手動実行できます。
- 手動実行時はデプロイメッセージを指定できます。
- デプロイ時にActionsが `deploy-info.json` を生成し、ページ上に日時・実行番号・実行者・コミットを表示します。

## 誰がデプロイできる？

- リポジトリにWrite以上の権限があるメンバーはPushや手動実行ができます。
- 一般公開しただけでは、誰でもWorkflowを実行できるわけではありません。
- 外部の人はForkからPull Requestを送り、権限のあるメンバーがレビュー・マージする運用が安全です。

## 注意

公開リポジトリの場合、GitHub Pagesのサイトはインターネット上に公開されます。秘密情報や個人情報を置かないでください。
