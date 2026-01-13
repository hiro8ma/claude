# プロジェクトレイアウト

golang-standards/project-layoutに基づく標準的なGoプロジェクト構成。

## Goディレクトリ

### `/cmd`

メインアプリケーション。ディレクトリ名は実行ファイル名と一致させる。

```
cmd/
├── myapp/
│   └── main.go
└── mytool/
    └── main.go
```

- コードは最小限に
- 再利用可能なコードは `/pkg` へ
- 非共有コードは `/internal` へ

### `/internal`

プライベートなコード。Goコンパイラが外部からのインポートを禁止。

```
internal/
├── app/           # アプリケーション固有
│   └── myapp/
└── pkg/           # 内部共有ライブラリ
    └── lib/
```

### `/pkg`

外部から利用可能なライブラリ。公開前に十分検討すること。

```
pkg/
└── lib/
    └── domain/    # 公開ドメイン
```

### `/vendor`

依存関係（Go 1.14以降は通常不要）。

```bash
go mod vendor
```

## サービスディレクトリ

### `/api`

API定義ファイル。

- OpenAPI/Swagger仕様
- JSONスキーマ
- Protocol Buffers定義

### `/web`

Webアプリケーション固有。

- 静的アセット
- サーバーサイドテンプレート
- SPA

## 共通ディレクトリ

### `/configs`

設定ファイルテンプレートとデフォルト設定。

### `/init`

システム初期化設定（systemd、upstart等）。

### `/scripts`

ビルド・インストール用スクリプト。Makefileを小さく保つ。

### `/build`

パッケージングとCI設定。

```
build/
├── package/       # コンテナ、パッケージ設定
└── ci/            # CI設定
```

### `/deployments`

デプロイメント設定。

- IaaS/PaaS設定
- Kubernetes manifests
- Terraform/Helm

### `/test`

外部テストとテストデータ。

## その他

### `/docs`

設計・ユーザードキュメント（godoc以外）。

### `/tools`

プロジェクト支援ツール。`/pkg`と`/internal`からインポート可能。

### `/examples`

使用例。

### `/third_party`

外部ツール、フォークコード。

### `/assets`

リポジトリ関連アセット（画像、ロゴ等）。

## 避けるべきディレクトリ

### `/src`

**使用禁止**。Javaパターンを避ける。`$GOPATH/src`と混同を招く。
