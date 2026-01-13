# init() 関数

## init()を避ける

`init()`は決定性を損ない、テストを困難にする。代わりにヘルパー関数を`main()`から呼び出す。

```go
// Bad
var db *Database

func init() {
    var err error
    db, err = connectDB()
    if err != nil {
        log.Fatal(err)  // init内でexit
    }
}

// Good
func main() {
    db, err := connectDB()
    if err != nil {
        log.Fatal(err)
    }
    // dbを使用...
}
```

## init()の問題点

1. **実行順序が不明確**: パッケージの依存関係で決まる
2. **テスト困難**: init()を無効化できない
3. **エラー処理困難**: panicかlog.Fatalしかない
4. **副作用が隠れる**: importするだけで実行される

## 許容されるケース

### プラグイン登録

```go
// database/sql スタイルのドライバ登録
func init() {
    sql.Register("mysql", &MySQLDriver{})
}
```

### 複雑な式の事前計算

```go
var _defaultPort = computeDefaultPort()

func init() {
    // 複雑な初期化で、エラーが起きない場合
}
```

### 正規表現のコンパイル

```go
var _re = regexp.MustCompile(`...`)  // init不要、varで十分
```

## リファクタリング例

```go
// Before
var cfg *Config

func init() {
    cfg = loadConfig()
}

func GetSetting(key string) string {
    return cfg.Get(key)
}

// After
type App struct {
    cfg *Config
}

func NewApp() (*App, error) {
    cfg, err := loadConfig()
    if err != nil {
        return nil, err
    }
    return &App{cfg: cfg}, nil
}

func (a *App) GetSetting(key string) string {
    return a.cfg.Get(key)
}
```

## init()内でのゴルーチン禁止

`init()`内でゴルーチンを生成しない。

```go
// Bad
func init() {
    go backgroundWorker()  // 制御不能
}

// Good - コンストラクタで生成
type Worker struct {
    done chan struct{}
}

func NewWorker() *Worker {
    w := &Worker{done: make(chan struct{})}
    go w.run()
    return w
}

func (w *Worker) Close() {
    close(w.done)
}
```
