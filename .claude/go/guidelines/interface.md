# インターフェース

## ポインタを使用しない

インターフェースへのポインタは使用しない。インターフェースは値として渡す。

```go
// Bad
func process(w *io.Writer) { ... }

// Good
func process(w io.Writer) { ... }
```

## コンパイル時の準拠チェック

型がインターフェースを実装していることをコンパイル時に検証する。

```go
type Handler struct { ... }

// Handler が http.Handler を実装していることを保証
var _ http.Handler = (*Handler)(nil)
```

## レシーバとインターフェース

- 値レシーバのメソッドは値とポインタ両方で呼び出せる
- ポインタレシーバのメソッドはアドレス可能な値でのみ呼び出せる
- mapの値はアドレス不可なため、ポインタレシーバのメソッドを呼べない

```go
type S struct{}

func (s S) ValueMethod()    {}
func (s *S) PointerMethod() {}

var s S
s.ValueMethod()    // OK
s.PointerMethod()  // OK (Go が自動的に &s に変換)

m := map[int]S{1: {}}
m[1].ValueMethod()    // OK
m[1].PointerMethod()  // コンパイルエラー
```
