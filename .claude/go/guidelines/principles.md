# スタイル原則

Goコードの基本原則（重要度順）。

## 1. 明確性（Clarity）

コードの目的と根拠が読者に明確であること。

```go
// Bad - 目的が不明確
func process(d []byte) error {
    // ...
}

// Good - 目的が明確
func validateJSON(data []byte) error {
    // ...
}
```

読みやすさは著者視点ではなく**読者視点**で評価する。

## 2. シンプリシティ（Simplicity）

目標を達成する最も単純な方法で記述。

```go
// Bad - 不要な抽象化
type DataProcessor interface {
    Process(data []byte) ([]byte, error)
}

// Good - 単純な関数
func processJSON(data []byte) ([]byte, error) {
    return json.Marshal(data)
}
```

### 最小メカニズムの原則

優先順位:
1. 言語構文
2. 標準ライブラリ
3. 外部ライブラリ

## 3. 簡潔性（Concision）

高いシグナル・ノイズ比を持つコード。

避けるべき要素:
- 繰り返しコード
- 余分な構文
- 不透明な命名
- 不要な抽象化

## 4. 保守性（Maintainability）

将来のプログラマが容易に修正可能。

```go
// Bad - 暗黙の仮定
func process(data string) {
    parts := strings.Split(data, ",")
    name := parts[0]  // 要素が存在する前提
}

// Good - 仮定を明示
func process(data string) error {
    parts := strings.Split(data, ",")
    if len(parts) < 1 {
        return errors.New("invalid format")
    }
    name := parts[0]
    // ...
}
```

## 5. 一貫性（Consistency）

コードベース全体で一貫した外観・動作。

一貫性は他の原則を上書きしないが、判定が同じ場合は一貫性を優先。

## 過度なエンジニアリングを避ける

```go
// Bad - 将来のために複雑化
type Config struct {
    providers map[string]Provider
    factory   ProviderFactory
    registry  *Registry
}

// Good - 必要なものだけ
type Config struct {
    Host    string
    Port    int
    Timeout time.Duration
}
```

必要になるまで抽象化しない。
