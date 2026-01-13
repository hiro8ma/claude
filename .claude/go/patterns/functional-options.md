# Functional Options パターン

拡張可能なコンストラクタを実装するパターン。

## 基本形

```go
type Server struct {
    addr    string
    timeout time.Duration
    logger  Logger
}

type Option func(*Server)

func WithTimeout(d time.Duration) Option {
    return func(s *Server) {
        s.timeout = d
    }
}

func WithLogger(l Logger) Option {
    return func(s *Server) {
        s.logger = l
    }
}

func NewServer(addr string, opts ...Option) *Server {
    s := &Server{
        addr:    addr,
        timeout: 30 * time.Second,  // デフォルト値
        logger:  defaultLogger,
    }
    for _, opt := range opts {
        opt(s)
    }
    return s
}
```

## 使用例

```go
// デフォルト設定
srv := NewServer(":8080")

// カスタム設定
srv := NewServer(":8080",
    WithTimeout(60*time.Second),
    WithLogger(customLogger),
)
```

## インターフェースによる実装

より型安全な実装:

```go
type Option interface {
    apply(*Server)
}

type timeoutOption time.Duration

func (t timeoutOption) apply(s *Server) {
    s.timeout = time.Duration(t)
}

func WithTimeout(d time.Duration) Option {
    return timeoutOption(d)
}
```

## 利点

- 後方互換性を保ちながらオプションを追加可能
- デフォルト値を明確に定義
- 呼び出し側のコードが読みやすい
- 必須パラメータと任意パラメータを分離

## 適用場面

- コンストラクタのパラメータが多い
- 将来パラメータが増える可能性がある
- 多くの場合デフォルト値で十分

## 避けるべき場面

- パラメータが2-3個で固定
- すべてのパラメータが必須
- シンプルな内部ユーティリティ
