# 将来対応

## Sealedトレイト（C-SEALED）

外部クレートによる実装を防ぐ。

```rust
mod private {
    pub trait Sealed {}
}

/// このトレイトは外部で実装できません。
pub trait MyTrait: private::Sealed {
    fn method(&self);
}

// クレート内でのみ実装可能
impl private::Sealed for MyType {}
impl MyTrait for MyType {
    fn method(&self) { ... }
}
```

ドキュメントに明記:

```rust
/// This trait is sealed and cannot be implemented outside of this crate.
pub trait DatabaseDriver: private::Sealed { ... }
```

## プライベートフィールド（C-STRUCT-PRIVATE）

フィールドを公開することは強いコミットメント。

```rust
// Bad - フィールドが公開
pub struct Config {
    pub timeout: u64,  // 検証できない
}

// Good - ゲッター/セッターで制御
pub struct Config {
    timeout: u64,
}

impl Config {
    pub fn timeout(&self) -> u64 {
        self.timeout
    }

    pub fn set_timeout(&mut self, timeout: u64) -> Result<(), Error> {
        if timeout == 0 {
            return Err(Error::InvalidTimeout);
        }
        self.timeout = timeout;
        Ok(())
    }
}
```

構造体リテラルでの構築を防ぐ:

```rust
pub struct Point {
    pub x: i32,
    pub y: i32,
    _private: (),  // プライベートフィールドで構築を制限
}

impl Point {
    pub fn new(x: i32, y: i32) -> Self {
        Self { x, y, _private: () }
    }
}
```

## Newtypeで実装を隠蔽

```rust
// 内部実装を隠す
pub struct MyIterator(std::vec::IntoIter<i32>);

impl Iterator for MyIterator {
    type Item = i32;

    fn next(&mut self) -> Option<Self::Item> {
        self.0.next()
    }
}

// または impl Trait
pub fn iter() -> impl Iterator<Item = i32> {
    vec![1, 2, 3].into_iter()
}
```

## 不要なトレイト境界を避ける（C-STRUCT-BOUNDS）

構造体定義でトレイト境界を追加しない。

```rust
// Bad - 不要な境界
pub struct Container<T: Clone> {
    value: T,
}

// Good - 境界なし
pub struct Container<T> {
    value: T,
}

// 境界は必要な場所でのみ
impl<T: Clone> Container<T> {
    pub fn clone_value(&self) -> T {
        self.value.clone()
    }
}
```

例外:

1. 関連型を参照する場合
2. `?Sized`境界
3. `Drop`実装の要件

```rust
// 例外1: 関連型
pub struct Wrapper<I: Iterator> {
    current: Option<I::Item>,
    iter: I,
}

// 例外2: ?Sized
pub struct Ref<'a, T: ?Sized> {
    value: &'a T,
}
```

## 非破壊的拡張

### enumの拡張

`#[non_exhaustive]`で将来のバリアント追加を許可。

```rust
#[non_exhaustive]
pub enum Error {
    NotFound,
    PermissionDenied,
    // 将来追加可能
}

// 外部クレートでは _ パターンが必要
match error {
    Error::NotFound => { ... }
    Error::PermissionDenied => { ... }
    _ => { ... }  // 必須
}
```

### 構造体の拡張

```rust
#[non_exhaustive]
pub struct Config {
    pub name: String,
    pub value: i32,
    // 将来フィールド追加可能
}
```
