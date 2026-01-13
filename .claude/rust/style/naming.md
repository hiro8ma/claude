# 命名規則

## 型名

UpperCamelCase。

```rust
struct MyStruct;
enum MyEnum { VariantA, VariantB }
trait MyTrait {}
type MyType = i32;
```

## 関数・メソッド

snake_case。

```rust
fn calculate_sum() {}
fn process_data() {}

impl MyStruct {
    fn get_value(&self) -> i32 {}
    fn set_value(&mut self, v: i32) {}
}
```

## 変数・フィールド

snake_case。

```rust
let user_name = "alice";
let total_count = 42;

struct User {
    first_name: String,
    last_name: String,
}
```

## 定数・静的変数

SCREAMING_SNAKE_CASE。

```rust
const MAX_SIZE: usize = 1024;
static DEFAULT_NAME: &str = "unknown";
```

## モジュール

snake_case。

```rust
mod my_module;
mod utils;
```

## マクロ

snake_case。

```rust
macro_rules! my_macro {
    () => {};
}
```

## 列挙型バリアント

UpperCamelCase。

```rust
enum Color {
    Red,
    Green,
    DarkBlue,
}
```

## ライフタイム

短い小文字。`'a`、`'b`など。意味がある場合は説明的に。

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {}

// 説明的なライフタイム
struct Context<'context> {
    data: &'context str,
}
```

## 予約語の回避

生識別子`r#`またはアンダースコア接尾辞を使用。

```rust
// Good
let r#type = "value";
let type_ = "value";

// Bad - 綴りを変えない
// let typ = "value";
```

## 変換メソッドの命名

| プレフィックス | コスト | 所有権 |
|---------------|--------|--------|
| `as_` | 無コスト | 借用→借用 |
| `to_` | コストあり | 借用→所有 |
| `into_` | 可変 | 所有→所有 |

```rust
impl MyType {
    fn as_str(&self) -> &str {}
    fn to_string(&self) -> String {}
    fn into_inner(self) -> Inner {}
}
```

## ゲッター

`get_`プレフィックスを付けない。

```rust
impl User {
    // Good
    fn name(&self) -> &str {}
    fn age(&self) -> u32 {}

    // Bad
    // fn get_name(&self) -> &str {}
}
```
