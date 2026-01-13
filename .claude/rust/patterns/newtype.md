# Newtypeパターン

## 基本形

既存の型をラップして新しい型を作成。

```rust
// 型安全なID
pub struct UserId(u64);
pub struct OrderId(u64);

// コンパイル時に区別される
fn process_user(id: UserId) {}
fn process_order(id: OrderId) {}

let user_id = UserId(1);
let order_id = OrderId(1);

process_user(user_id);    // OK
// process_user(order_id);  // コンパイルエラー
```

## Derefの実装

内部値へのアクセスを簡略化。

```rust
use std::ops::Deref;

pub struct Email(String);

impl Deref for Email {
    type Target = str;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}

let email = Email("user@example.com".to_string());
println!("Length: {}", email.len());  // &strのメソッドが使える
```

## バリデーション

コンストラクタでバリデーション。

```rust
#[derive(Debug, Clone)]
pub struct Email(String);

impl Email {
    pub fn new(value: impl Into<String>) -> Result<Self, ValidationError> {
        let value = value.into();
        if value.contains('@') && value.len() > 3 {
            Ok(Self(value))
        } else {
            Err(ValidationError::InvalidEmail)
        }
    }

    pub fn as_str(&self) -> &str {
        &self.0
    }
}

// 一度作成されれば常に有効
let email = Email::new("user@example.com")?;
```

## トレイトの実装

```rust
use std::fmt;

pub struct UserId(u64);

impl fmt::Display for UserId {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "user_{}", self.0)
    }
}

impl From<u64> for UserId {
    fn from(id: u64) -> Self {
        Self(id)
    }
}

impl From<UserId> for u64 {
    fn from(id: UserId) -> Self {
        id.0
    }
}
```

## 単位型

物理単位を型で表現。

```rust
#[derive(Debug, Clone, Copy)]
pub struct Meters(f64);

#[derive(Debug, Clone, Copy)]
pub struct Kilometers(f64);

impl Meters {
    pub fn new(value: f64) -> Self {
        Self(value)
    }

    pub fn to_kilometers(self) -> Kilometers {
        Kilometers(self.0 / 1000.0)
    }
}

impl std::ops::Add for Meters {
    type Output = Self;

    fn add(self, other: Self) -> Self {
        Self(self.0 + other.0)
    }
}

// 異なる単位の加算はコンパイルエラー
let m1 = Meters::new(100.0);
let m2 = Meters::new(200.0);
let total = m1 + m2;  // OK

let km = Kilometers(1.0);
// let invalid = m1 + km;  // コンパイルエラー
```

## 外部トレイトの実装

Newtypeで孤児ルールを回避。

```rust
// Vec<T>にDisplayを実装できない（孤児ルール）
// しかしNewtypeでは可能

pub struct MyVec<T>(Vec<T>);

impl<T: fmt::Display> fmt::Display for MyVec<T> {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "[")?;
        for (i, item) in self.0.iter().enumerate() {
            if i > 0 {
                write!(f, ", ")?;
            }
            write!(f, "{}", item)?;
        }
        write!(f, "]")
    }
}
```
