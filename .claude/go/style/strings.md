# 文字列処理

## 単純な連結

少数の文字列には`+`を使用。

```go
// Good - シンプル
greeting := "Hello, " + name + "!"

// フォーマットが必要な場合
message := fmt.Sprintf("User %s (ID: %d)", name, id)
```

## フォーマット済み文字列

`fmt.Sprintf()`を使用。

```go
url := fmt.Sprintf("https://%s/api/v%d/users", host, version)

// 直接出力の場合
fmt.Fprintf(w, "Request: %s %s", method, path)
```

## 段階的な構築

`strings.Builder`を使用（O(n)）。

```go
// Good - O(n)
var b strings.Builder
for _, item := range items {
    b.WriteString(item)
    b.WriteString(", ")
}
result := b.String()

// Bad - O(n²)
var result string
for _, item := range items {
    result += item + ", "
}
```

## 複数行リテラル

バッククォートを使用。

```go
// Good
query := `
    SELECT id, name, email
    FROM users
    WHERE status = 'active'
    ORDER BY created_at DESC
`

// Bad - 連結
query := "SELECT id, name, email\n" +
    "FROM users\n" +
    "WHERE status = 'active'"
```

## %qの使用

文字列をクォートして出力。

```go
fmt.Printf("name: %q\n", name)  // name: "Alice"

// 特殊文字のエスケープにも有用
input := "hello\nworld"
fmt.Printf("input: %q\n", input)  // input: "hello\nworld"
```

## バイト列と文字列

変換は一度だけ。

```go
// Good
data := []byte(str)
process(data)
more(data)

// Bad - 何度も変換
process([]byte(str))
more([]byte(str))
```

## 文字列比較

`==`で比較。

```go
if a == b {
    // ...
}

// 大文字小文字を無視
if strings.EqualFold(a, b) {
    // ...
}
```

## 空文字列チェック

```go
if s == "" {
    // ...
}

// 長さでも可
if len(s) == 0 {
    // ...
}
```

## 文字列の結合

`strings.Join`を使用。

```go
result := strings.Join(parts, ", ")
```

## プレフィックス/サフィックス

```go
if strings.HasPrefix(path, "/api/") {
    // ...
}

if strings.HasSuffix(filename, ".json") {
    // ...
}
```

## トリミング

```go
// 空白を除去
s = strings.TrimSpace(s)

// 特定の文字を除去
s = strings.Trim(s, "\n\r")

// プレフィックス/サフィックスを除去
s = strings.TrimPrefix(s, "http://")
s = strings.TrimSuffix(s, "/")
```

## 変換

プリミティブ変換には`strconv`を使用（`fmt`より高速）。

```go
// Good
s := strconv.Itoa(42)
n, err := strconv.Atoi("42")

// Bad
s := fmt.Sprintf("%d", 42)
```
