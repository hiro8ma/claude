# 時間処理パターン

## 型の使い分け

- 時刻（瞬間）: `time.Time`
- 期間: `time.Duration`

```go
// Good
func schedule(at time.Time, duration time.Duration) {}

// Bad
func schedule(timestamp int64, durationMs int) {}
```

## 時刻の比較

メソッドを使用。演算子ではなく。

```go
// Good
if t1.Before(t2) { ... }
if t1.After(t2) { ... }
if t1.Equal(t2) { ... }

// Bad
if t1 < t2 { ... }  // コンパイルエラー
```

## 日付計算

- カレンダー日付: `AddDate()`
- 経過時間: `Add()`

```go
// 1ヶ月後（カレンダー上）
nextMonth := now.AddDate(0, 1, 0)

// 24時間後（正確な経過時間）
tomorrow := now.Add(24 * time.Hour)
```

## 期間の指定

`time.Duration`を引数に使用。生の整数値ではなく。

```go
// Good
func poll(interval time.Duration) {}
poll(10 * time.Second)

// Bad
func poll(intervalMs int) {}
poll(10000)
```

## 外部システムとの連携

可能な限り`time.Time`/`time.Duration`を使用。
やむを得ない場合:

- フィールド名に単位を含める（例: `IntervalMillis`）
- 文字列はRFC 3339形式

```go
type Config struct {
    // Good
    Timeout time.Duration

    // やむを得ない場合
    TimeoutMillis int64 `json:"timeout_millis"`

    // 文字列の場合はRFC 3339
    StartTime string `json:"start_time"` // "2024-01-15T09:00:00Z"
}
```

## テストでの時間

時刻を注入可能にする:

```go
// Bad - テスト困難
func isExpired(expiry time.Time) bool {
    return time.Now().After(expiry)
}

// Good - 時刻を注入
func isExpired(expiry, now time.Time) bool {
    return now.After(expiry)
}

// Good - インターフェースで抽象化
type Clock interface {
    Now() time.Time
}

type realClock struct{}
func (realClock) Now() time.Time { return time.Now() }

type mockClock struct{ t time.Time }
func (c mockClock) Now() time.Time { return c.t }
```

## タイムゾーン

- 内部処理はUTCを使用
- 表示時のみローカルタイムに変換

```go
// 保存時
createdAt := time.Now().UTC()

// 表示時
loc, _ := time.LoadLocation("Asia/Tokyo")
localTime := createdAt.In(loc)
```
