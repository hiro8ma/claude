# モノレポ構成

境界コンテキストごとにサービスを分離したモノレポ構成。

## メリット

- 共通ライブラリ（pubsub, bigquery, database等）の共有
- Dockerfileの共通化
- 一貫したCI/CD設定
- コードレビューとリファクタリングの容易さ

## ディレクトリ構成

※例はGoだが、構造は他言語にも適用可能

```
.
├── Makefile                    # タスクランナー
├── README.md
├── build/                      # コンテナビルド
│   ├── Dockerfile
│   ├── docker-compose.yml
│   └── Dockerfile.Prod
├── cmd/                        # エントリーポイント
│   ├── service1/
│   │   └── main.*              # メイン + DI設定
│   ├── service2/
│   └── service3/
├── configs/                    # 設定ファイル
│   └── ...
├── database/                   # DB設定
│   ├── migrations/             # スキーママイグレーション
│   └── seeds/                  # 開発用シードデータ
├── internal/                   # 非公開（サービス実装）
│   ├── service1/
│   │   ├── adapter/            # アダプター層
│   │   ├── domain/             # ドメイン層
│   │   └── usecase/            # ユースケース層
│   ├── service2/
│   │   └── ...
│   └── shared/                 # 非公開共通ライブラリ
│       ├── pagination/
│       ├── pubsub/
│       └── ...
├── pkg/                        # 公開パッケージ
│   └── domain/                 # 公開ドメイン
│       ├── model/              # 公開オブジェクト
│       └── event/              # 公開イベント定義
├── api/                        # API定義
│   ├── proto/                  # Protocol Buffers
│   └── openapi/                # OpenAPI/Swagger
├── scripts/                    # CI/ビルドスクリプト
└── secrets/                    # 認証情報（.gitignore）
```

## Internal設計

### Application閉鎖レイヤー

各マイクロサービス（`internal/service1`等）。

### Pkg開放レイヤー

共通化ライブラリ。**業務ロジックはライブラリ化しない**。

## サービス内構成

```
internal/service1/
├── adapter/
│   ├── domain/
│   │   ├── repository/
│   │   │   ├── repository.go
│   │   │   └── repository_dto.go
│   │   ├── externalservice/
│   │   └── service/
│   └── handler/
│       ├── rest/
│       ├── grpc/
│       ├── pubsub/
│       └── handlers.go
├── domain/
│   ├── model/
│   ├── repository/
│   ├── externalservice/
│   ├── service/
│   └── factory/
└── usecase/
    ├── usecase.go
    ├── usecase_impl.go
    ├── usecase_dto.go
    └── mock/
```
