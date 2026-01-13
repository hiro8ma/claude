# Go開発ガイド

Uber Go Style Guideに準拠したGo開発ガイドライン。

## ディレクトリ構成

```
go/
├── guidelines/          # ガイドライン（コアルール）
│   ├── interface.md         # インターフェース
│   ├── concurrency.md       # 並行処理
│   ├── error.md             # エラー処理
│   ├── panic-exit.md        # Panic/Exit
│   ├── enum.md              # 列挙型
│   ├── atomic.md            # アトミック操作
│   ├── globals.md           # グローバル変数
│   ├── init.md              # init()関数
│   └── field-tags.md        # フィールドタグ
│
├── performance/         # パフォーマンス
│   └── optimization.md      # 最適化
│
├── style/              # スタイル（書き方）
│   ├── naming.md            # 命名規則
│   ├── variable.md          # 変数宣言
│   ├── struct.md            # 構造体
│   ├── formatting.md        # フォーマット
│   └── literals.md          # リテラル・引数
│
├── patterns/           # パターン
│   ├── test.md              # テストパターン
│   ├── functional-options.md # Functional Options
│   └── time.md              # 時間処理
│
├── linting/            # リンティング
│   └── setup.md             # セットアップ
│
└── commands/           # 開発コマンド
    ├── build.md             # ビルド
    └── test.md              # テスト実行
```

## 参考

- [Uber Go Style Guide](https://github.com/uber-go/guide)
- [Effective Go](https://go.dev/doc/effective_go)
