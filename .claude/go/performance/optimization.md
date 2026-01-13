# パフォーマンス最適化

## 文字列変換

プリミティブ変換には`strconv`を使用。`fmt`より高速。

```go
// Good
s := strconv.Itoa(42)
n, err := strconv.Atoi("42")

// Bad
s := fmt.Sprintf("%d", 42)
```

## 文字列とバイトスライスの変換

変換は一度だけ行い、結果を再利用。

```go
// Good
b := []byte(s)
process1(b)
process2(b)

// Bad
process1([]byte(s))
process2([]byte(s))
```

## マップの容量

要素数が分かる場合は容量ヒントを指定。再ハッシュを削減。

```go
// Good
m := make(map[string]int, len(items))

// Bad
m := make(map[string]int)
for _, item := range items {
    m[item.Key] = item.Value  // 頻繁な再ハッシュ
}
```

## スライスの容量

要素数が分かる場合は容量を指定。再アロケーションを削減。

```go
// Good
s := make([]int, 0, len(items))
for _, item := range items {
    s = append(s, item.Value)
}

// Bad
var s []int
for _, item := range items {
    s = append(s, item.Value)  // 頻繁な再アロケーション
}
```

## deferのオーバーヘッド

`defer`のオーバーヘッドは最小限。可読性を優先して使用。

```go
// Good - deferを使用
func process(f *os.File) error {
    defer f.Close()
    // 処理...
    return nil
}
```

## 文字列連結

大量の連結には`strings.Builder`を使用。

```go
// Good
var b strings.Builder
for _, s := range items {
    b.WriteString(s)
}
result := b.String()

// Bad
var result string
for _, s := range items {
    result += s  // 毎回新しい文字列を生成
}
```

## 正規表現のコンパイル

正規表現は一度だけコンパイルし、再利用。

```go
// Good
var re = regexp.MustCompile(`^\d+$`)

func validate(s string) bool {
    return re.MatchString(s)
}

// Bad
func validate(s string) bool {
    re := regexp.MustCompile(`^\d+$`)  // 毎回コンパイル
    return re.MatchString(s)
}
```

## JSONエンコード/デコード

構造体タグを使用し、リフレクションを最適化。

```go
type User struct {
    ID   int    `json:"id"`
    Name string `json:"name"`
}
```

大量データにはストリーミングAPI:

```go
// Good - ストリーミング
dec := json.NewDecoder(r)
for dec.More() {
    var item Item
    if err := dec.Decode(&item); err != nil {
        return err
    }
    process(item)
}

// Bad - 全データをメモリに
var items []Item
if err := json.Unmarshal(data, &items); err != nil {
    return err
}
```
