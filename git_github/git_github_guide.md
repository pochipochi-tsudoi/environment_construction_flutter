# Git/GitHub 概要と Git インストール手順

目的: Git と GitHub の要点、そして Git のインストール（Windows/macOS 共通手順）をコンパクトにまとめます。

## Git とは

- 分散型のバージョン管理システム。ローカルに全履歴を保持し、高速に履歴操作できる。
- 変更をスナップショット（コミット）として記録し、いつでも過去に戻れる。
- ブランチで安全に並行開発でき、マージやリベースで変更を統合できる。
- 個人開発から大規模チームまで標準的に使われるツール。

## GitHub とは

- Git リポジトリをホストするクラウドサービス（他に GitLab/Bitbucket 等もある）。
- リモートの保存・共有、Pull Request（変更提案・レビュー）、Issues、Actions（CI/CD）等を提供。
- Web UI と連携機能により、コラボレーションを強力に支援する。

## Git と GitHub の関係

- Git = バージョン管理ツール本体（ローカルでも完結できる）。
- GitHub = Git リポジトリを保管・共同開発するためのオンラインサービス。
- Git は GitHub 以外のリモートにも接続できる。GitHub は Git を使って動いている。

## Git のインストール（Windows / macOS 共通手順）

1. 公式サイトを開く: https://git-scm.com/downloads
2. 自分の OS のインストーラをダウンロード。
3. インストーラを起動し、特に理由がなければ既定（デフォルト）設定のまま完了まで進める。
4. 端末を開き、バージョンを確認して完了:
   - Windows: PowerShell または コマンドプロンプトで `git --version`
   - macOS: ターミナルで `git --version`

補足:
- macOS では Xcode Command Line Tools を求められたら画面の案内に従ってインストールしてもよい（`xcode-select --install`）。
- Windows では「Git Bash」ターミナルも同梱される。通常のターミナルでも利用可。

## インストール後の初期設定（共通）

以下は最初の一回だけ設定します。お好きな名前/メールに置き換えてください。

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```
