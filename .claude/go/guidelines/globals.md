# グローバル変数

## ミュータブルなグローバルを避ける

変更可能なグローバル変数はテスト困難でレースを引き起こす。依存性注入を使用。

```go
// Bad - グローバルなミュータブル状態
var db *Database

func init() {
    db = connectDB()
}

func GetUser(id int) (*User, error) {
    return db.Query(id)
}

// Good - 依存性注入
type UserService struct {
    db *Database
}

func NewUserService(db *Database) *UserService {
    return &UserService{db: db}
}

func (s *UserService) GetUser(id int) (*User, error) {
    return s.db.Query(id)
}
```

## 許容されるグローバル

イミュータブルな値は許容。

```go
// OK - 定数的な値
var (
    _defaultTimeout = 30 * time.Second
    _maxRetries     = 3
)

// OK - コンパイル時に決定される正規表現
var _emailRegex = regexp.MustCompile(`^[a-z]+@[a-z]+\.[a-z]+$`)

// OK - エラー値
var ErrNotFound = errors.New("not found")
```

## シングルトンの回避

シングルトンパターンもグローバル状態の一形態。

```go
// Bad - シングルトン
var instance *Service
var once sync.Once

func GetService() *Service {
    once.Do(func() {
        instance = &Service{}
    })
    return instance
}

// Good - 明示的な生成と注入
func main() {
    svc := NewService()
    handler := NewHandler(svc)
    // ...
}
```

## テスト容易性

依存性注入によりテストが容易に。

```go
// テスト困難
func ProcessData() error {
    return globalDB.Save(data)  // グローバルDB
}

// テスト容易
type Processor struct {
    db Database
}

func (p *Processor) ProcessData() error {
    return p.db.Save(data)
}

// テスト
func TestProcessor(t *testing.T) {
    mockDB := &MockDatabase{}
    p := &Processor{db: mockDB}
    // ...
}
```
