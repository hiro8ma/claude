# エラー処理

## エラー型の選択

| 条件 | メッセージ | 使用する型 |
|------|-----------|-----------|
| マッチング不要 | 静的 | `errors.New` |
| マッチング不要 | 動的 | `fmt.Errorf` |
| マッチング必要 | 静的 | トップレベル`var` |
| マッチング必要 | 動的 | カスタム型 |

```go
// マッチング不要・静的
return errors.New("connection failed")

// マッチング不要・動的
return fmt.Errorf("connection to %s failed", addr)

// マッチング必要・静的
var ErrNotFound = errors.New("not found")
return ErrNotFound

// マッチング必要・動的
type NotFoundError struct {
    Name string
}
func (e *NotFoundError) Error() string {
    return fmt.Sprintf("%s not found", e.Name)
}
```

## エラーラップ

- 呼び出し元がアクセスする場合: `%w`
- 隠蔽する場合: `%v`
- "failed to"などの冗長なフレーズは避ける

```go
// Good
return fmt.Errorf("connect: %w", err)

// Bad
return fmt.Errorf("failed to connect: %w", err)
```

## エラー命名

- エクスポート: `Err`接頭辞（例: `ErrNotFound`）
- 非エクスポート: `err`接頭辞（例: `errInvalidInput`）
- カスタム型: `Error`接尾辞（例: `NotFoundError`）

## 型アサーション

必ずcomma-okイディオムを使用。panicを避ける。

```go
// Bad
t := i.(string) // iがstringでなければpanic

// Good
t, ok := i.(string)
if !ok {
    // エラー処理
}
```

## エラーは一度だけ処理

各エラーは一箇所でのみ処理する:

- ログを出力して機能を縮退する、または
- ラップして上位に返す、または
- マッチして特定の処理を行う

```go
// Bad - 二重処理
if err != nil {
    log.Printf("error: %v", err)
    return err  // ログ出力後にまた返している
}

// Good - 一度だけ処理
if err != nil {
    return fmt.Errorf("operation: %w", err)
}
```
