# 変数宣言

## トップレベル変数

型が式から明らかな場合は省略。異なる場合は明示。

```go
var (
    _port = 8080           // intは明らか
    _timeout = time.Second // time.Durationは明らか
)

var _format string = "json" // 型を明示したい場合
```

## ローカル変数

### 短縮宣言 `:=`

値を明示的に代入する場合に使用。

```go
s := "hello"
n, err := strconv.Atoi(str)
```

### `var`宣言

ゼロ値を使用する場合。特に空スライス。

```go
// Good
var user User
var items []string

// Bad
user := User{}
items := []string{}
```

## 空スライス

`nil`スライスを返す。空リテラルではなく。

```go
// Good
func getItems() []string {
    if noItems {
        return nil
    }
    return items
}

// Bad
func getItems() []string {
    if noItems {
        return []string{}
    }
    return items
}
```

空チェックは`len()`を使用。`nil`比較ではなく。

```go
// Good
if len(items) == 0 { ... }

// Bad
if items == nil { ... }
```

## 構造体ポインタ

`&T{}`を使用。`new(T)`ではなく。

```go
// Good
user := &User{Name: "alice"}

// Bad
user := new(User)
user.Name = "alice"
```

## スコープの縮小

変数は使用箇所にできるだけ近くで宣言。

```go
// Good
if err := doSomething(); err != nil {
    return err
}

// Bad
err := doSomething()
if err != nil {
    return err
}
```

## ローカル定数

複数の関数で使用しない限り、関数内にスコープを限定。

```go
// Good
func process() {
    const maxRetries = 3
    // ...
}

// Bad（1つの関数でしか使わない場合）
const maxRetries = 3

func process() {
    // ...
}
```
