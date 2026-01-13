# 型安全性

## Newtype（C-NEWTYPE）

基盤となる型を区別。

```rust
// 単位の混同を防ぐ
pub struct Miles(pub f64);
pub struct Kilometers(pub f64);

fn travel_distance(miles: Miles) -> Kilometers {
    Kilometers(miles.0 * 1.60934)
}

// コンパイルエラー: 型が異なる
// let km = travel_distance(Kilometers(100.0));
```

ID型の区別:

```rust
pub struct UserId(pub u64);
pub struct OrderId(pub u64);

fn get_user(id: UserId) -> User { ... }
fn get_order(id: OrderId) -> Order { ... }

// 混同を防ぐ
let user_id = UserId(1);
let order_id = OrderId(1);
// get_user(order_id);  // コンパイルエラー
```

## カスタム型（C-CUSTOM-TYPE）

`bool`や`Option`の代わりに意味のある型を使用。

```rust
// Bad - 意味が不明
fn create_widget(small: bool, round: bool) -> Widget { ... }
let w = create_widget(true, false);  // 何を意味する？

// Good - 明確
pub enum Size { Small, Large }
pub enum Shape { Round, Square }

fn create_widget(size: Size, shape: Shape) -> Widget { ... }
let w = create_widget(Size::Small, Shape::Square);
```

オプションの明確化:

```rust
// Bad
fn find_user(id: Option<u64>) -> Option<User> { ... }

// Good
pub enum UserQuery {
    ById(u64),
    ByEmail(String),
    All,
}

fn find_user(query: UserQuery) -> Option<User> { ... }
```

## Bitflags（C-BITFLAG）

フラグセットには`bitflags`クレートを使用。

```rust
use bitflags::bitflags;

bitflags! {
    #[derive(Debug, Clone, Copy, PartialEq, Eq, Hash)]
    pub struct Permissions: u32 {
        const READ    = 0b0001;
        const WRITE   = 0b0010;
        const EXECUTE = 0b0100;
        const ALL     = Self::READ.bits() | Self::WRITE.bits() | Self::EXECUTE.bits();
    }
}

// 使用
let perms = Permissions::READ | Permissions::WRITE;
if perms.contains(Permissions::READ) {
    println!("Can read");
}
```

## Builderパターン（C-BUILDER）

複雑な構造の構築に専用ビルダーを使用。

### 非消費型ビルダー

```rust
pub struct ServerBuilder {
    host: String,
    port: u16,
}

impl ServerBuilder {
    pub fn new() -> Self {
        Self {
            host: "localhost".to_string(),
            port: 8080,
        }
    }

    pub fn host(&mut self, host: &str) -> &mut Self {
        self.host = host.to_string();
        self
    }

    pub fn port(&mut self, port: u16) -> &mut Self {
        self.port = port;
        self
    }

    pub fn build(&self) -> Server {
        Server {
            host: self.host.clone(),
            port: self.port,
        }
    }
}

// 使用
let server = ServerBuilder::new()
    .host("0.0.0.0")
    .port(3000)
    .build();
```

### 消費型ビルダー

```rust
impl ServerBuilder {
    pub fn host(mut self, host: &str) -> Self {
        self.host = host.to_string();
        self
    }

    pub fn build(self) -> Server {
        Server {
            host: self.host,
            port: self.port,
        }
    }
}
```

消費型はクローン不要で効率的。非消費型は同じビルダーから複数インスタンスを作成可能。
