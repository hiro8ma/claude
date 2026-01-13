# 構造体

## フィールド名の明示

構造体初期化時は必ずフィールド名を指定する。

```go
// Good
user := User{
    Name:  "alice",
    Email: "alice@example.com",
}

// Bad
user := User{"alice", "alice@example.com"}
```

例外: 3フィールド以下のテストテーブル。

## ゼロ値フィールドの省略

コンテキストを提供する場合を除き、ゼロ値フィールドは省略。

```go
// Good
user := User{
    Name: "alice",
}

// Bad（Active: falseは冗長）
user := User{
    Name:   "alice",
    Active: false,
}

// OK（falseであることを明示したい場合）
config := Config{
    Enabled: false,  // 明示的に無効化
}
```

## ゼロ値構造体

`var`を使用。空リテラルではなく。

```go
// Good
var user User

// Bad
user := User{}
```

## 構造体埋め込み

- 埋め込みは先頭に配置し、空行で分離
- mutexは埋め込まない
- 利便性のためだけに埋め込まない
- 外部型に論理的に属するメソッドのみ埋め込む

```go
// Good
type Server struct {
    http.Server  // 埋め込みは先頭

    addr string
    port int
}

// Bad - mutexを埋め込み
type SMap struct {
    sync.Mutex
    data map[string]string
}

// Good - mutexはフィールドとして
type SMap struct {
    mu   sync.Mutex
    data map[string]string
}
```

## マップ初期化

- 空のマップ: `make()`を使用
- 固定要素: リテラルを使用

```go
// 空のマップ
m := make(map[string]int)

// 固定要素
m := map[string]int{
    "one": 1,
    "two": 2,
}
```

## 容量ヒント

要素数が分かる場合は容量を指定。

```go
// Good
m := make(map[string]int, len(items))
s := make([]int, 0, len(items))

// Bad
m := make(map[string]int)
s := make([]int, 0)
```
