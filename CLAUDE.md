# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## ビルド・テストコマンド

```bash
# ビルド
go build ./...

# テスト実行
go test ./...

# 単一パッケージのテスト
go test ./pkg/パッケージ名

# 単一テスト実行
go test -run TestFuncName ./pkg/パッケージ名

# レースコンディション検出付きテスト
go test -race ./...

# リント
golangci-lint run
```

## Goコードスタイルガイド (Uber Go Style Guide準拠)

### インターフェース

- インターフェースへのポインタは使用しない（値として渡す）
- コンパイル時のインターフェース準拠チェック: `var _ http.Handler = (*Handler)(nil)`

### 並行処理

- `sync.Mutex`/`sync.RWMutex`はゼロ値で有効、埋め込まない
- チャネルサイズは0か1を使用（それ以上は正当な理由が必要）
- ゴルーチンは必ず終了条件を持つこと（fire-and-forget禁止）
- `init()`内でゴルーチンを生成しない

### エラー処理

| マッチング不要 | 静的メッセージ | `errors.New` |
|---|---|---|
| マッチング不要 | 動的メッセージ | `fmt.Errorf` |
| マッチング必要 | 静的メッセージ | トップレベル`var` |
| マッチング必要 | 動的メッセージ | カスタム型 |

- エラーラップ: 呼び出し元がアクセスする場合`%w`、隠蔽する場合`%v`
- 命名: エクスポート時は`Err`接頭辞、非エクスポートは`err`接頭辞
- 型アサーションは必ずcomma-okイディオム使用: `t, ok := i.(string)`

### panic/exit

- `panic`は避け、エラーを返す
- `os.Exit`/`log.Fatal`は`main()`のみで呼び出す
- ロジックは`run()`関数にラップして単一exit pointを維持

### パフォーマンス

- プリミティブ変換は`fmt`より`strconv`を使用
- map/sliceは容量ヒントを指定: `make(map[T1]T2, hint)`, `make([]T, 0, cap)`

### 命名規則

- パッケージ名: 小文字、アンダースコアなし、短く、単数形
- 関数: MixedCaps、テストは`TestFunc_Scenario`形式可
- 非エクスポートグローバル: `_`接頭辞（例: `_defaultPort`）

### 変数宣言

- ゼロ値には`var`使用: `var user User`（`user := User{}`ではなく）
- 空スライスは`var s []T`（`s := []T{}`ではなく）
- 構造体ポインタは`&T{...}`使用（`new(T)`ではなく）

### 構造体

- 初期化時はフィールド名を明示
- ゼロ値フィールドはコンテキストを提供する場合のみ記載
- 埋め込みは先頭に配置し空行で分離、mutexは埋め込まない

### テスト

テーブル駆動テストを使用:
```go
tests := []struct{
    name string
    give string
    want string
}{
    {"ケース1", "入力", "期待値"},
}

for _, tt := range tests {
    t.Run(tt.name, func(t *testing.T) {
        // テストロジック
    })
}
```

テーブルテスト内での条件分岐は避ける。

### 時間処理

- 時刻には`time.Time`、期間には`time.Duration`を使用
- 比較は`Before()`, `After()`, `Equal()`メソッドを使用
- カレンダー日付には`AddDate()`、経過時間には`Add()`

### 推奨リンター

golangci-lintで以下を有効化:
- errcheck
- goimports
- revive
- govet
- staticcheck
