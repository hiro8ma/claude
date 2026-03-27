---
name: security-check
description: コードや設定変更時にセキュリティチェックを自動実行する
---

# セキュリティチェック

コードの実装・設定変更時に以下を確認してください。

## APIキー・シークレット

- 生のAPIキーをソースコードにハードコードしない
- `.env` ファイルは `.gitignore` に含まれているか確認
- `NEXT_PUBLIC_` で公開するキーにはGCPコンソールでAPI制限をかける
- 課金が発生するAPI（Gemini, Maps, Vertex AI等）のキーには必ずリファラー制限・IP制限をかける
- 不要なAPIキーは削除する

## GCP API有効化時

- 既存の「制限なし」キーがないか先に確認
- 新しいAPI用のキーは専用で作成し、適切な制限を設定
- 課金アラートを設定する

## コミット前

- `git diff` にAPIキー（`AIzaSy` 等）、パスワード、トークンが含まれていないか
- ローカルパス（`/Users/...`）が含まれていないか
- `.env` ファイルがステージングされていないか

## 参考

- インシデント事例: `security/docs/gcp-api-key-incident-2026.md`
- Firebase APIキー管理: https://firebase.google.com/docs/projects/api-keys
- GCP既知の脆弱性: https://trufflesecurity.com/blog/google-api-keys-werent-secrets-but-then-gemini-changed-the-rules
