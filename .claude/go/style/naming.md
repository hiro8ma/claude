# 命名規則

## パッケージ名

- 小文字のみ
- アンダースコアなし
- 短く簡潔に
- 単数形
- 汎用的な名前は避ける（`common`, `util`, `lib`）

```go
// Good
package user
package http
package json

// Bad
package userService
package http_util
package common
```

## 関数名

- MixedCaps（キャメルケース）を使用
- テスト関数はアンダースコアでシナリオを区切れる

```go
// 通常の関数
func ProcessUser() {}
func getUserByID() {}

// テスト関数
func TestUser_Create(t *testing.T) {}
func TestUser_Delete_NotFound(t *testing.T) {}
```

## 変数名

### 非エクスポートグローバル変数

`_`接頭辞を付ける。

```go
var (
    _defaultPort = 8080
    _maxRetries  = 3
)
```

### エラー変数

- エクスポート: `Err`接頭辞
- 非エクスポート: `err`接頭辞（`_`なし）

```go
var (
    ErrNotFound   = errors.New("not found")      // エクスポート
    errBadRequest = errors.New("bad request")    // 非エクスポート
)
```

## インポートエイリアス

パッケージ名がインポートパスの最後の要素と異なる場合、または競合する場合のみ使用。

```go
import (
    "net/http"

    client "example.com/client-go"          // パッケージ名がclient-go
    prometheus "github.com/prometheus/client_golang/prometheus"
)
```

## 関数の順序

1. エクスポート関数
2. `newXYZ()`/`NewXYZ()`コンストラクタ
3. メソッド（レシーバでグループ化）
4. ユーティリティ関数

呼び出し順にソートし、関連するものをグループ化する。
