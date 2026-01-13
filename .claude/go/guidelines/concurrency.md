# 並行処理

## Mutex

`sync.Mutex`と`sync.RWMutex`はゼロ値で有効。ポインタや埋め込みにしない。

```go
// Bad
type SMap struct {
    sync.Mutex  // 埋め込みは避ける
    data map[string]string
}

// Good
type SMap struct {
    mu   sync.Mutex
    data map[string]string
}
```

## チャネルサイズ

チャネルサイズは0（バッファなし）か1を使用。それ以上は正当な理由が必要。

```go
// 推奨
ch := make(chan int)    // バッファなし
ch := make(chan int, 1) // バッファ1

// 避ける（正当な理由がない場合）
ch := make(chan int, 100)
```

## ゴルーチン管理

fire-and-forgetは禁止。すべてのゴルーチンには以下が必要:

- 予測可能な終了時間
- 停止を通知する手段
- 完了を待機する手段

```go
// Bad
go func() {
    doSomething()
}()

// Good - WaitGroupを使用
var wg sync.WaitGroup
wg.Add(1)
go func() {
    defer wg.Done()
    doSomething()
}()
wg.Wait()

// Good - チャネルで終了を通知
done := make(chan struct{})
go func() {
    defer close(done)
    doSomething()
}()
<-done
```

## init()でのゴルーチン禁止

`init()`内でゴルーチンを生成しない。コンストラクタ内で生成し、`Close()`/`Shutdown()`メソッドを公開する。

```go
// Bad
func init() {
    go backgroundTask()
}

// Good
type Service struct {
    done chan struct{}
}

func NewService() *Service {
    s := &Service{done: make(chan struct{})}
    go s.backgroundTask()
    return s
}

func (s *Service) Close() {
    close(s.done)
}
```

## 境界でのコピー

スライス/マップを引数で受け取り保存する場合、コピーを作成。
戻り値として返す場合も、内部状態の変更を防ぐためコピー。

```go
type Store struct {
    items []string
}

func (s *Store) SetItems(items []string) {
    s.items = make([]string, len(items))
    copy(s.items, items)
}

func (s *Store) GetItems() []string {
    result := make([]string, len(s.items))
    copy(result, s.items)
    return result
}
```
