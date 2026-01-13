# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## コマンド

### Go

```bash
go build ./...              # ビルド
go test ./...               # 全テスト
go test -run TestName ./pkg # 単一テスト
go test -race ./...         # レース検出
golangci-lint run           # リント
```

### Rust

```bash
cargo build                 # ビルド
cargo build --release       # リリースビルド
cargo test                  # 全テスト
cargo test test_name        # 単一テスト
cargo clippy                # リント
cargo fmt                   # フォーマット
```

## 開発ガイド

詳細は `.claude/` を参照。

### Go（Uber Go Style Guide準拠）

| カテゴリ | 内容 |
|---------|------|
| [guidelines/](.claude/go/guidelines/) | インターフェース、並行処理、エラー処理、panic/exit、enum、atomic、globals、init、field-tags |
| [performance/](.claude/go/performance/) | 最適化（strconv、容量ヒント、文字列連結等） |
| [style/](.claude/go/style/) | 命名規則、変数宣言、構造体、フォーマット、リテラル |
| [patterns/](.claude/go/patterns/) | テスト、Functional Options、時間処理 |
| [linting/](.claude/go/linting/) | golangci-lint設定 |
| [commands/](.claude/go/commands/) | ビルド、テストコマンド |

### Rust（公式スタイルガイド準拠）

| カテゴリ | 内容 |
|---------|------|
| [guidelines/](.claude/rust/guidelines/) | 所有権・借用、エラー処理、unsafe、並行処理 |
| [style/](.claude/rust/style/) | フォーマット、命名規則、アイテム、式、文、型 |
| [patterns/](.claude/rust/patterns/) | Builder、Newtype、Typestate、イテレータ |
| [linting/](.claude/rust/linting/) | Clippy/rustfmt設定 |
| [cargo/](.claude/rust/cargo/) | Cargo.toml規約 |
| [commands/](.claude/rust/commands/) | ビルド、テストコマンド |
