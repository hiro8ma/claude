# フォーマット

## 行の長さ

ソフトリミット99文字。可読性を優先して判断。

```go
// OK - 長くても読みやすい
user, err := userService.GetUserByEmailAndOrganization(ctx, email, orgID)

// 分割した方が読みやすい場合
result, err := service.ProcessComplexOperation(
    ctx,
    inputData,
    WithTimeout(30*time.Second),
    WithRetries(3),
)
```

## グループ化

関連する宣言をブロックでグループ化。

```go
// import
import (
    "fmt"
    "strings"

    "github.com/org/repo/pkg"
)

// 定数
const (
    StatusActive   = "active"
    StatusInactive = "inactive"
)

// 変数
var (
    ErrNotFound = errors.New("not found")
    ErrInvalid  = errors.New("invalid")
)

// 型
type (
    UserID   int64
    OrderID  int64
)
```

## インポート順序

1. 標準ライブラリ
2. サードパーティ
3. 自プロジェクト

空行で分離。

```go
import (
    "context"
    "fmt"

    "github.com/pkg/errors"
    "go.uber.org/zap"

    "github.com/myorg/myproject/internal/user"
    "github.com/myorg/myproject/pkg/util"
)
```

## 不要なelseを避ける

早期リターンでelseを削除。

```go
// Bad
func process(x int) int {
    if x > 0 {
        return x * 2
    } else {
        return 0
    }
}

// Good
func process(x int) int {
    if x > 0 {
        return x * 2
    }
    return 0
}
```

## ネストを減らす

早期リターン、早期continueでネストを浅く。

```go
// Bad - 深いネスト
for _, item := range items {
    if item.Valid {
        if item.Type == "special" {
            if item.Value > 0 {
                process(item)
            }
        }
    }
}

// Good - 早期continue
for _, item := range items {
    if !item.Valid {
        continue
    }
    if item.Type != "special" {
        continue
    }
    if item.Value <= 0 {
        continue
    }
    process(item)
}
```

## 組み込み名のシャドウイング禁止

Go の組み込み識別子をシャドウしない。

```go
// Bad
var error string         // errorをシャドウ
var string string        // stringをシャドウ
len := getLength()       // lenをシャドウ
copy := original.Clone() // copyをシャドウ

// Good
var errMsg string
var str string
length := getLength()
clone := original.Clone()
```

組み込み識別子一覧:
- 型: `bool`, `byte`, `error`, `int`, `string`, etc.
- 関数: `append`, `cap`, `close`, `copy`, `delete`, `len`, `make`, `new`, `panic`, `print`, `println`, `recover`
- 定数: `true`, `false`, `iota`, `nil`
