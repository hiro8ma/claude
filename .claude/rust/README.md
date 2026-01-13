# Rust開発ガイド

Rust公式スタイルガイドとAPI Guidelinesに準拠。

## ディレクトリ構成

```
rust/
├── guidelines/          # ガイドライン（コアルール）
│   ├── ownership.md         # 所有権・借用
│   ├── error.md             # エラー処理
│   ├── unsafe.md            # unsafe
│   └── concurrency.md       # 並行処理
│
├── style/              # スタイル（書き方）
│   ├── formatting.md        # フォーマット基本
│   ├── naming.md            # 命名規則
│   ├── items.md             # アイテム（関数・構造体等）
│   ├── expressions.md       # 式
│   ├── statements.md        # 文
│   └── types.md             # 型
│
├── api/                # API設計ガイドライン
│   ├── naming.md            # API命名規則
│   ├── interop.md           # 相互運用性
│   ├── documentation.md     # ドキュメンテーション
│   ├── predictability.md    # 予測可能性
│   ├── flexibility.md       # 柔軟性
│   ├── type-safety.md       # 型安全性
│   ├── dependability.md     # 信頼性
│   ├── debuggability.md     # デバッグ可能性
│   ├── future-proofing.md   # 将来対応
│   └── macros.md            # マクロ設計
│
├── patterns/           # パターン
│   ├── builder.md           # Builderパターン
│   ├── newtype.md           # Newtypeパターン
│   ├── typestate.md         # Typestateパターン
│   └── iterator.md          # イテレータパターン
│
├── linting/            # リンティング
│   └── setup.md             # Clippy/rustfmt設定
│
├── cargo/              # Cargo設定
│   └── toml.md              # Cargo.toml規約
│
└── commands/           # 開発コマンド
    ├── build.md             # ビルド
    └── test.md              # テスト実行
```

## 言語共通アーキテクチャ

[.claude/architecture/](../architecture/) を参照:
- ヘキサゴナル/クリーンアーキテクチャ
- ドメイン駆動設計 (DDD)
- イベント駆動アーキテクチャ
- モノレポ構成

## 参考

- [Rust Style Guide](https://github.com/rust-lang/rust/tree/HEAD/src/doc/style-guide/src)
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
- [The Rust Programming Language](https://doc.rust-lang.org/book/)
