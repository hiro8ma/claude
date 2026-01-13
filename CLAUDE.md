# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## コマンド

```bash
go build ./...              # ビルド
go test ./...               # 全テスト
go test -run TestName ./pkg # 単一テスト
go test -race ./...         # レース検出
golangci-lint run           # リント
```

## Go開発ガイド

詳細は `.claude/go/` を参照。Uber Go Style Guide準拠。

| カテゴリ | 内容 |
|---------|------|
| [guidelines/](.claude/go/guidelines/) | インターフェース、並行処理、エラー処理、panic/exit、enum、atomic、globals、init、field-tags |
| [performance/](.claude/go/performance/) | 最適化（strconv、容量ヒント、文字列連結等） |
| [style/](.claude/go/style/) | 命名規則、変数宣言、構造体、フォーマット、リテラル |
| [patterns/](.claude/go/patterns/) | テスト、Functional Options、時間処理 |
| [linting/](.claude/go/linting/) | golangci-lint設定 |
| [commands/](.claude/go/commands/) | ビルド、テストコマンド |
