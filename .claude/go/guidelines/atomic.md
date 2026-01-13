# アトミック操作

## go.uber.org/atomic を使用

標準の`sync/atomic`より型安全な`go.uber.org/atomic`を推奨。

```go
// Bad - sync/atomic は型安全でない
import "sync/atomic"

var running int32
atomic.StoreInt32(&running, 1)
if atomic.LoadInt32(&running) == 1 { ... }

// Good - go.uber.org/atomic
import "go.uber.org/atomic"

var running atomic.Bool
running.Store(true)
if running.Load() { ... }
```

## 利用可能な型

```go
import "go.uber.org/atomic"

var (
    counter atomic.Int64
    flag    atomic.Bool
    name    atomic.String
    value   atomic.Value  // any型
    err     atomic.Error
    dur     atomic.Duration
    t       atomic.Time
)
```

## 基本操作

```go
var counter atomic.Int64

counter.Store(10)       // 設定
v := counter.Load()     // 取得
counter.Add(5)          // 加算
counter.Sub(3)          // 減算
counter.Inc()           // +1
counter.Dec()           // -1
old := counter.Swap(20) // 交換
counter.CAS(20, 30)     // Compare-And-Swap
```

## ユースケース

```go
// フラグ
type Server struct {
    running atomic.Bool
}

func (s *Server) Start() {
    s.running.Store(true)
}

func (s *Server) IsRunning() bool {
    return s.running.Load()
}

// カウンタ
type Stats struct {
    requests atomic.Int64
}

func (s *Stats) RecordRequest() {
    s.requests.Inc()
}
```

## 注意点

複数のアトミック操作を組み合わせる場合、全体としてアトミックにならない。

```go
// Bad - 2つの操作は個別にアトミックだが、全体としてはレース
if counter.Load() < 100 {
    counter.Inc()
}

// Good - Mutexで保護
mu.Lock()
if counter < 100 {
    counter++
}
mu.Unlock()
```
