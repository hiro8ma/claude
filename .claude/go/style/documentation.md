# ドキュメンテーション

## ドキュメントコメント

すべてのエクスポートされた名前に必須。

```go
// User represents a registered user in the system.
type User struct {
    ID    int
    Name  string
    Email string
}

// Authenticate verifies the user's credentials.
// It returns an error if authentication fails.
func (u *User) Authenticate(password string) error {
    // ...
}
```

## コメントの形式

- 対象の名前で始める
- 完全な文として記述
- 大文字で始め句点で終わる

```go
// Good
// Server handles incoming HTTP requests.
type Server struct{}

// Bad
// handles incoming HTTP requests
type Server struct{}
```

## パッケージコメント

package句の直前に配置。空行を挟まない。

```go
// Package httputil provides HTTP utility functions
// for handling common web operations.
package httputil
```

## 行の長さ

80カラムを目安。可読性を優先。

## パラメータのドキュメント

明らかなパラメータは列挙しない。

```go
// Bad - 明らかな情報
// Open opens the file at the given path.
// path is the path to the file to open.
func Open(path string) (*File, error)

// Good - 非明示的な情報のみ
// Open opens the file at the given path.
// The file must exist and be readable.
func Open(path string) (*File, error)
```

## クリーンアップのドキュメント

明示的なクリーンアップ要件を記述。

```go
// Open opens a database connection.
// The caller must call Close when done to release resources.
//
// Example:
//
//     db, err := Open("postgres://...")
//     if err != nil {
//         return err
//     }
//     defer db.Close()
func Open(dsn string) (*DB, error)
```

## エラーのドキュメント

返すセンチネルエラーと型を記述。

```go
// ErrNotFound is returned when the requested resource does not exist.
var ErrNotFound = errors.New("not found")

// Get retrieves a value by key.
// It returns ErrNotFound if the key does not exist.
func (s *Store) Get(key string) (string, error)
```

## 並行性のドキュメント

不明確な場合のみ記述。

```go
// Counter is safe for concurrent use.
type Counter struct {
    mu sync.Mutex
    n  int
}
```

## 名前付き戻り値

ドキュメント目的で使用。本体では使用しない。

```go
// Good - 複数の同じ型
func Coordinates() (latitude, longitude float64)

// Good - 明示的に返す
func Parse(input string) (result *Parsed, err error) {
    parsed := &Parsed{}
    // ...
    return parsed, nil  // naked returnではない
}
```

## Exampleコード

```go
func ExampleUser_Authenticate() {
    user := &User{Name: "alice"}
    err := user.Authenticate("password")
    if err != nil {
        log.Fatal(err)
    }
    // Output:
}
```

## TODOコメント

```go
// TODO(username): Implement caching for better performance.
// See https://github.com/org/repo/issues/123
```
