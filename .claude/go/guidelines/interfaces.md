# インターフェース

## 定義場所

消費するパッケージで定義。実装パッケージでは定義しない。

```go
// Good - 消費側で定義
// package myservice
type UserStore interface {
    Get(id int) (*User, error)
}

type Service struct {
    store UserStore
}

// package storage - 具象型を返す
func NewStore() *Store {
    return &Store{}
}
```

## 具象型を返す

エクスポート関数はインターフェースではなく具象型を返す。

```go
// Good
func NewClient() *Client {
    return &Client{}
}

// Bad
func NewClient() ClientInterface {
    return &Client{}
}
```

## インターフェースの粒度

小さく保つ。

```go
// Good - 単一メソッド
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

// 必要に応じて合成
type ReadWriter interface {
    Reader
    Writer
}
```

## 受け入れるインターフェース

可能な限り小さいインターフェースを受け入れる。

```go
// Good - 必要最小限
func Copy(dst io.Writer, src io.Reader) (int64, error)

// Bad - 過剰な要求
func Copy(dst io.ReadWriteCloser, src io.ReadWriteCloser) (int64, error)
```

## 未使用インターフェース

使用されないインターフェースを定義しない。

```go
// Bad - 実装が1つで拡張予定なし
type Logger interface {
    Log(msg string)
}

type FileLogger struct{}

// Good - 必要になるまで待つ
type FileLogger struct{}

func (l *FileLogger) Log(msg string) {}
```

## テストダブル

テストダブルをエクスポートしない。

```go
// Good
// package storage
type Store struct{}  // 具象型

// package storage_test
type mockStore struct{}  // テストパッケージ内で定義
```

## コンパイル時の準拠チェック

型がインターフェースを実装していることを保証。

```go
var _ http.Handler = (*MyHandler)(nil)
var _ io.ReadCloser = (*MyReader)(nil)
```

## 型アサーション

常にチェック。

```go
// Good
value, ok := data.(string)
if !ok {
    return errors.New("expected string")
}

// Bad
value := data.(string)  // パニックの可能性
```

## anyの使用

Go 1.18以降は`any`を使用。

```go
// Good
func Process(data any) error

// Bad
func Process(data interface{}) error
```
