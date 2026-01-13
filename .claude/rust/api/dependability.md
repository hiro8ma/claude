# 信頼性

## 引数のバリデーション（C-VALIDATE）

入力の有効性を強制。

### 静的強制

型で無効な入力を防ぐ。

```rust
// Ascii型でASCII文字のみを保証
pub struct Ascii(u8);

impl Ascii {
    pub fn new(byte: u8) -> Option<Self> {
        if byte.is_ascii() {
            Some(Self(byte))
        } else {
            None
        }
    }

    pub fn get(&self) -> u8 {
        self.0
    }
}

// 一度作成されれば常に有効
fn process_ascii(c: Ascii) {
    // バリデーション不要
}
```

### 動的強制

処理時にバリデーション。

```rust
pub fn parse_port(s: &str) -> Result<u16, ParseError> {
    let port: u16 = s.parse()?;
    if port == 0 {
        return Err(ParseError::InvalidPort);
    }
    Ok(port)
}
```

### オプショナルチェック

パフォーマンス重視のコードにuncheckedバリアントを提供。

```rust
impl MyVec {
    pub fn get(&self, index: usize) -> Option<&T> {
        if index < self.len {
            Some(unsafe { self.get_unchecked(index) })
        } else {
            None
        }
    }

    /// # Safety
    ///
    /// `index`は`self.len()`未満でなければならない
    pub unsafe fn get_unchecked(&self, index: usize) -> &T {
        &*self.ptr.add(index)
    }
}
```

## デストラクタは失敗しない（C-DTOR-FAIL）

`Drop`実装はパニックしない。

```rust
impl Drop for Connection {
    fn drop(&mut self) {
        // エラーを無視（ログは可）
        let _ = self.socket.shutdown();
    }
}

// エラーハンドリングが必要な場合は別メソッドを提供
impl Connection {
    pub fn close(self) -> io::Result<()> {
        self.socket.shutdown()?;
        // dropは呼ばれない（selfを消費）
        std::mem::forget(self);
        Ok(())
    }
}
```

## デストラクタはブロックしない（C-DTOR-BLOCK）

デストラクタでブロッキング操作を避ける。

```rust
impl Server {
    /// サーバーを正常にシャットダウン
    pub async fn shutdown(self) -> Result<(), Error> {
        self.notify_clients().await?;
        self.wait_for_connections().await?;
        Ok(())
    }
}

impl Drop for Server {
    fn drop(&mut self) {
        // ブロックしない
        // 非同期シャットダウンが呼ばれなかった場合の
        // 最小限のクリーンアップのみ
    }
}
```

## エラーメッセージの形式

小文字で開始し、句点を付けない。

```rust
impl fmt::Display for MyError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        // Good
        write!(f, "connection failed: {}", self.reason)

        // Bad
        // write!(f, "Connection failed: {}.", self.reason)
    }
}
```

これにより、エラーチェーンで自然に連結できる:

```rust
// "operation failed: connection failed: timeout"
```
