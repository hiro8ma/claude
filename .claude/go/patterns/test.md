# テストパターン

## テーブル駆動テスト

繰り返しのテストロジックにはテーブル駆動テストを使用。

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name string
        a, b int
        want int
    }{
        {"positive", 1, 2, 3},
        {"negative", -1, -2, -3},
        {"zero", 0, 0, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := Add(tt.a, tt.b)
            if got != tt.want {
                t.Errorf("Add(%d, %d) = %d, want %d", tt.a, tt.b, got, tt.want)
            }
        })
    }
}
```

## テーブルテスト内での条件分岐禁止

テーブルテスト内で条件分岐が必要な場合は、別のテストに分割する。

```go
// Bad
for _, tt := range tests {
    t.Run(tt.name, func(t *testing.T) {
        got, err := Process(tt.input)
        if tt.wantErr {
            if err == nil {
                t.Fatal("expected error")
            }
            return
        }
        if err != nil {
            t.Fatal(err)
        }
        // さらに条件分岐...
    })
}

// Good - エラーケースは別テストに
func TestProcess_Success(t *testing.T) {
    tests := []struct{ ... }{...}
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got, err := Process(tt.input)
            if err != nil {
                t.Fatal(err)
            }
            // アサーション
        })
    }
}

func TestProcess_Error(t *testing.T) {
    tests := []struct{ ... }{...}
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            _, err := Process(tt.input)
            if err == nil {
                t.Fatal("expected error")
            }
        })
    }
}
```

## テスト関数の命名

`Test<対象>_<シナリオ>`形式。

```go
func TestUser_Create(t *testing.T) {}
func TestUser_Create_DuplicateEmail(t *testing.T) {}
func TestUser_Delete(t *testing.T) {}
func TestUser_Delete_NotFound(t *testing.T) {}
```

## サブテスト

`t.Run()`で関連テストをグループ化。

```go
func TestUser(t *testing.T) {
    t.Run("Create", func(t *testing.T) {
        // ...
    })
    t.Run("Delete", func(t *testing.T) {
        // ...
    })
}
```

## 並列テスト

独立したテストは`t.Parallel()`で並列実行。

```go
func TestProcess(t *testing.T) {
    tests := []struct{...}{...}

    for _, tt := range tests {
        tt := tt  // ループ変数をキャプチャ
        t.Run(tt.name, func(t *testing.T) {
            t.Parallel()
            // テストロジック
        })
    }
}
```
