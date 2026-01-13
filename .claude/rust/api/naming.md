# API命名規則

## ケーシング（C-CASE）

RFC 430に準拠。

| 種類 | 規則 | 例 |
|------|------|-----|
| モジュール | snake_case | `my_module` |
| 型・トレイト | UpperCamelCase | `MyStruct` |
| 列挙バリアント | UpperCamelCase | `VariantName` |
| 関数・メソッド | snake_case | `do_something` |
| 定数・静的変数 | SCREAMING_SNAKE_CASE | `MAX_SIZE` |
| 型パラメータ | 単一大文字 | `T`, `E` |
| ライフタイム | 短い小文字 | `'a`, `'ctx` |

### 頭字語

単語として扱う。全大文字にしない。

```rust
// Good
struct Uuid;
struct HttpClient;
fn btree_map();

// Bad
struct UUID;
struct HTTPClient;
fn b_tree_map();
```

### クレート名

`-rs`や`-rust`サフィックスは不要。

```rust
// Good: serde, tokio, reqwest
// Bad: serde-rs, tokio-rust
```

## 変換メソッド（C-CONV）

| プレフィックス | コスト | 所有権 | 例 |
|---------------|--------|--------|-----|
| `as_` | 無料 | 借用→借用 | `str::as_bytes()` |
| `to_` | 高コスト | 借用→所有/同等抽象度 | `Path::to_str()` |
| `into_` | 可変 | 所有→所有 | `String::into_bytes()` |

```rust
impl MyType {
    // 無料の参照変換
    fn as_slice(&self) -> &[u8] { ... }

    // コストのある変換
    fn to_vec(&self) -> Vec<u8> { ... }

    // 所有権を消費
    fn into_inner(self) -> Inner { ... }
}
```

## ゲッター命名（C-GETTER）

`get_`プレフィックスは使用しない。

```rust
impl Container {
    // Good
    fn first(&self) -> Option<&T> { ... }
    fn first_mut(&mut self) -> Option<&mut T> { ... }

    // Bad
    fn get_first(&self) -> Option<&T> { ... }
}
```

例外: 唯一の明確な対象がある場合のみ`get`を使用。

```rust
impl Cell<T> {
    fn get(&self) -> T { ... }  // OK
}
```

## イテレータメソッド（C-ITER）

コレクションは3つのメソッドを提供。

```rust
impl MyCollection<T> {
    fn iter(&self) -> Iter<'_, T> { ... }       // &T
    fn iter_mut(&mut self) -> IterMut<'_, T> { ... }  // &mut T
    fn into_iter(self) -> IntoIter<T> { ... }   // T
}
```

## イテレータ型名（C-ITER-TY）

メソッド名に対応する型名を使用。

```rust
// iter() -> Iter
// iter_mut() -> IterMut
// into_iter() -> IntoIter
```

## 機能フラグ命名（C-FEATURE）

プレースホルダー単語を避ける。

```toml
[features]
# Good
std = []
serde = ["dep:serde"]

# Bad
use-std = []
with-serde = ["dep:serde"]
```

## 単語順序（C-WORD-ORDER）

エラー型は「動詞-目的語-Error」順。

```rust
// Good
struct ParseIntError;
struct RecvTimeoutError;
struct SendError<T>;

// Bad
struct IntParseError;
struct TimeoutRecvError;
```
