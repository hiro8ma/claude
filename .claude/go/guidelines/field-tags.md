# フィールドタグ

## マーシャル対象の構造体にはタグを付ける

JSON、YAML等でシリアライズする構造体にはフィールドタグを必ず付ける。

```go
// Bad - タグなし
type User struct {
    ID        int
    Name      string
    Email     string
    CreatedAt time.Time
}
// JSONは {"ID":1,"Name":"alice",...} になる

// Good - タグあり
type User struct {
    ID        int       `json:"id"`
    Name      string    `json:"name"`
    Email     string    `json:"email"`
    CreatedAt time.Time `json:"created_at"`
}
// JSONは {"id":1,"name":"alice",...} になる
```

## タグを付ける理由

1. **API契約の明確化**: フィールド名変更がAPIに影響しない
2. **リファクタリング安全性**: 内部名を変更してもJSON出力は維持
3. **命名規則の変換**: Go(PascalCase) → JSON(snake_case)

## 一般的なタグ

```go
type Config struct {
    // JSON
    Host string `json:"host"`

    // YAML
    Port int `yaml:"port"`

    // 複数形式
    Timeout time.Duration `json:"timeout" yaml:"timeout"`

    // データベース
    ID int `db:"id" json:"id"`

    // バリデーション
    Email string `json:"email" validate:"required,email"`
}
```

## 省略とomitempty

```go
type Response struct {
    // 常に出力
    Status string `json:"status"`

    // ゼロ値なら省略
    Error string `json:"error,omitempty"`

    // JSON出力から除外
    Internal string `json:"-"`
}
```

## インライン展開

```go
type Timestamps struct {
    CreatedAt time.Time `json:"created_at"`
    UpdatedAt time.Time `json:"updated_at"`
}

type User struct {
    ID   int    `json:"id"`
    Name string `json:"name"`
    Timestamps    // 埋め込み
}
// {"id":1,"name":"alice","created_at":"...","updated_at":"..."}
```

## カスタムマーシャラ

特殊な変換が必要な場合:

```go
type Status int

func (s Status) MarshalJSON() ([]byte, error) {
    return json.Marshal(s.String())
}

func (s *Status) UnmarshalJSON(data []byte) error {
    var str string
    if err := json.Unmarshal(data, &str); err != nil {
        return err
    }
    *s = parseStatus(str)
    return nil
}
```
