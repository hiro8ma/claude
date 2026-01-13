# デバッグ可能性

## すべての型にDebug（C-DEBUG）

すべての公開型は`Debug`トレイトを実装する。

```rust
#[derive(Debug)]
pub struct User {
    id: u64,
    name: String,
}

#[derive(Debug)]
pub enum Status {
    Active,
    Inactive,
    Pending,
}
```

手動実装が必要な場合:

```rust
use std::fmt;

pub struct Connection {
    socket: TcpStream,
    buffer: Vec<u8>,
}

impl fmt::Debug for Connection {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        f.debug_struct("Connection")
            .field("socket", &self.socket)
            .field("buffer_len", &self.buffer.len())  // 内容は省略
            .finish()
    }
}
```

機密情報を隠す:

```rust
pub struct ApiKey(String);

impl fmt::Debug for ApiKey {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        f.debug_tuple("ApiKey")
            .field(&"[REDACTED]")
            .finish()
    }
}
```

## 空でないDebug表現（C-DEBUG-NONEMPTY）

概念的に空の値でも、意味のある出力を提供。

```rust
// Good
impl fmt::Debug for EmptyStruct {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        f.debug_struct("EmptyStruct").finish()
    }
}
// 出力: EmptyStruct

// Good - 空のコレクション
let empty_vec: Vec<i32> = vec![];
println!("{:?}", empty_vec);
// 出力: []

// Good - 空の文字列
let empty_str = "";
println!("{:?}", empty_str);
// 出力: ""

// Bad - 空の出力
// 出力: (何もなし)
```

## Debugフォーマットのカスタマイズ

```rust
impl fmt::Debug for MyType {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        // 通常形式
        if f.alternate() {
            // {:#?} で呼ばれた場合（整形出力）
            f.debug_struct("MyType")
                .field("field1", &self.field1)
                .field("field2", &self.field2)
                .finish()
        } else {
            // {:?} で呼ばれた場合（コンパクト）
            write!(f, "MyType({}, {})", self.field1, self.field2)
        }
    }
}
```

## Displayとの使い分け

```rust
pub struct Error {
    code: u32,
    message: String,
}

// Debug - 開発者向け、詳細情報
impl fmt::Debug for Error {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        f.debug_struct("Error")
            .field("code", &self.code)
            .field("message", &self.message)
            .finish()
    }
}
// 出力: Error { code: 404, message: "Not found" }

// Display - ユーザー向け、簡潔
impl fmt::Display for Error {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "{}", self.message)
    }
}
// 出力: Not found
```

## テストでの活用

```rust
#[test]
fn test_user() {
    let user = User::new("alice");

    // アサーション失敗時にDebug出力が表示される
    assert_eq!(user.name(), "bob", "User: {:?}", user);
}
```
