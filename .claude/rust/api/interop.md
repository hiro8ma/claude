# 相互運用性

## 共通トレイトの実装（C-COMMON-TRAITS）

新しい型は積極的に標準トレイトを実装する。

```rust
#[derive(Debug, Clone, PartialEq, Eq, Hash, Default)]
pub struct MyType {
    // ...
}
```

### 実装すべきトレイト

| トレイト | 条件 |
|---------|------|
| `Copy` | 小さく、コピーが安い |
| `Clone` | ほぼすべての型 |
| `Eq`, `PartialEq` | 比較可能な型 |
| `Ord`, `PartialOrd` | 順序付け可能な型 |
| `Hash` | ハッシュマップのキーになりうる型 |
| `Debug` | すべての公開型 |
| `Display` | ユーザー向け表示が必要な型 |
| `Default` | 意味のあるデフォルト値がある型 |

## 変換トレイト（C-CONV-TRAITS）

`From`と`TryFrom`を実装。`Into`と`TryInto`は直接実装しない。

```rust
// Good - Fromを実装
impl From<u32> for MyType {
    fn from(value: u32) -> Self {
        Self { value }
    }
}

// TryFromでエラー可能な変換
impl TryFrom<i32> for MyType {
    type Error = ConversionError;

    fn try_from(value: i32) -> Result<Self, Self::Error> {
        if value < 0 {
            Err(ConversionError::Negative)
        } else {
            Ok(Self { value: value as u32 })
        }
    }
}
```

参照変換には`AsRef`と`AsMut`。

```rust
impl AsRef<[u8]> for MyType {
    fn as_ref(&self) -> &[u8] {
        &self.data
    }
}
```

## コレクショントレイト（C-COLLECT）

コレクションは`FromIterator`と`Extend`を実装。

```rust
impl<T> FromIterator<T> for MyCollection<T> {
    fn from_iter<I: IntoIterator<Item = T>>(iter: I) -> Self {
        let mut collection = Self::new();
        collection.extend(iter);
        collection
    }
}

impl<T> Extend<T> for MyCollection<T> {
    fn extend<I: IntoIterator<Item = T>>(&mut self, iter: I) {
        for item in iter {
            self.push(item);
        }
    }
}
```

## Serdeサポート（C-SERDE）

データ構造は`Serialize`と`Deserialize`を実装。

```rust
#[cfg(feature = "serde")]
use serde::{Deserialize, Serialize};

#[derive(Debug, Clone)]
#[cfg_attr(feature = "serde", derive(Serialize, Deserialize))]
pub struct Config {
    pub name: String,
    pub value: i32,
}
```

`Cargo.toml`:

```toml
[features]
serde = ["dep:serde"]

[dependencies]
serde = { version = "1", features = ["derive"], optional = true }
```

## スレッド安全性（C-SEND-SYNC）

`Send`と`Sync`は自動実装。テストで確認。

```rust
#[test]
fn test_send_sync() {
    fn assert_send<T: Send>() {}
    fn assert_sync<T: Sync>() {}

    assert_send::<MyType>();
    assert_sync::<MyType>();
}
```

## エラー型（C-GOOD-ERR）

エラー型は`std::error::Error`、`Send`、`Sync`を実装。

```rust
#[derive(Debug)]
pub struct MyError {
    kind: ErrorKind,
    message: String,
}

impl std::fmt::Display for MyError {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        write!(f, "{}: {}", self.kind, self.message)  // 小文字、句点なし
    }
}

impl std::error::Error for MyError {}

// ()をエラー型として使わない
// fn process() -> Result<(), ()>  // Bad
fn process() -> Result<(), MyError>  // Good
```

## 数値フォーマット（C-NUM-FMT）

ビット演算向け型はフォーマットトレイトを実装。

```rust
use std::fmt;

impl fmt::Binary for Flags {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        fmt::Binary::fmt(&self.bits, f)
    }
}

impl fmt::LowerHex for Flags { ... }
impl fmt::UpperHex for Flags { ... }
impl fmt::Octal for Flags { ... }
```

## ジェネリックI/O（C-RW-VALUE）

Reader/Writerは値で受け取る。

```rust
// Good - 値で受け取り、参照も受け入れ可能
pub fn read_data<R: Read>(reader: R) -> io::Result<Vec<u8>> {
    let mut buf = Vec::new();
    reader.read_to_end(&mut buf)?;
    Ok(buf)
}

// 呼び出し側
read_data(&mut file)?;      // &mut Fileを渡す
read_data(cursor)?;         // Cursorを渡す
```
