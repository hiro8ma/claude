# Panic と Exit

## panicを避ける

本番コードでは`panic`を避け、エラーを返す。panicはカスケード障害を引き起こす。

```go
// Bad
func process(data []byte) {
    if data == nil {
        panic("data is nil")
    }
    // ...
}

// Good
func process(data []byte) error {
    if data == nil {
        return errors.New("data is nil")
    }
    // ...
    return nil
}
```

## panicが許容されるケース

- 初期化時の回復不能なエラー
- プログラマのミスを示す場合（不正な引数など）

```go
// 許容: 初期化時
func MustCompile(pattern string) *Regexp {
    r, err := Compile(pattern)
    if err != nil {
        panic(err)
    }
    return r
}

// 使用側
var re = MustCompile(`^\d+$`) // 初期化時のみ使用
```

## main()でのみexit

`os.Exit`や`log.Fatal`は`main()`でのみ呼び出す。他の関数はエラーを返す。

```go
// Bad
func loadConfig() *Config {
    cfg, err := readConfig()
    if err != nil {
        log.Fatal(err)  // main以外でexit
    }
    return cfg
}

// Good
func loadConfig() (*Config, error) {
    cfg, err := readConfig()
    if err != nil {
        return nil, fmt.Errorf("load config: %w", err)
    }
    return cfg, nil
}

func main() {
    cfg, err := loadConfig()
    if err != nil {
        log.Fatal(err)  // mainでexit
    }
    // ...
}
```

## run()パターン

ロジックを`run()`関数にラップし、単一のexit pointを維持する。

```go
func main() {
    if err := run(); err != nil {
        fmt.Fprintf(os.Stderr, "error: %v\n", err)
        os.Exit(1)
    }
}

func run() error {
    cfg, err := loadConfig()
    if err != nil {
        return err
    }

    srv, err := newServer(cfg)
    if err != nil {
        return err
    }

    return srv.Run()
}
```

## deferとexit

`os.Exit`は`defer`を実行しない。リソースリークに注意。

```go
// Bad - deferが実行されない
func main() {
    f, _ := os.Open("file")
    defer f.Close()  // 実行されない

    if err := process(); err != nil {
        os.Exit(1)
    }
}

// Good - run()パターンでdeferを保証
func main() {
    if err := run(); err != nil {
        os.Exit(1)
    }
}

func run() error {
    f, err := os.Open("file")
    if err != nil {
        return err
    }
    defer f.Close()  // 正常に実行される

    return process()
}
```
