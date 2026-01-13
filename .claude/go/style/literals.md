# リテラルと引数

## Raw文字列リテラル

エスケープを避けるためバッククォートを使用。

```go
// Bad
regex := "\\d+\\.\\d+"
path := "C:\\Users\\name"
query := "SELECT * FROM users WHERE name = \"alice\""

// Good
regex := `\d+\.\d+`
path := `C:\Users\name`
query := `SELECT * FROM users WHERE name = "alice"`
```

複数行文字列にも有効:

```go
sql := `
    SELECT id, name, email
    FROM users
    WHERE status = 'active'
    ORDER BY created_at DESC
`
```

## 裸のパラメータを避ける

意味が不明な引数にはコメントを付ける。

```go
// Bad - 何を意味するか不明
printUser("alice", true, false)

// Good - コメントで明確化
printUser("alice", true /* isAdmin */, false /* isActive */)

// Better - 構造体やオプションで明確化
printUser(UserOptions{
    Name:     "alice",
    IsAdmin:  true,
    IsActive: false,
})
```

## マジックナンバーを避ける

意味のある名前を付ける。

```go
// Bad
if len(password) < 8 {
    return errors.New("too short")
}
time.Sleep(30 * time.Second)

// Good
const minPasswordLength = 8
if len(password) < minPasswordLength {
    return errors.New("too short")
}

const retryInterval = 30 * time.Second
time.Sleep(retryInterval)
```

## Printf形式の関数命名

`f`サフィックスでフォーマット関数を示す。

```go
// 標準に従う
log.Printf("user: %s", name)
fmt.Errorf("failed: %w", err)

// カスタム関数も同様
func Debugf(format string, args ...interface{}) {
    // ...
}
```
