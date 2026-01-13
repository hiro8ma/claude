# Rust開発ガイド

Rust公式スタイルガイドとベストプラクティスに準拠。

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

## 参考

- [Rust Style Guide](https://github.com/rust-lang/rust/tree/HEAD/src/doc/style-guide/src)
- [The Rust Programming Language](https://doc.rust-lang.org/book/)
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
